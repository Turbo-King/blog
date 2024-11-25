# Spring异步流式接口


<!--more-->

## 引言

{{< admonition question "在实际开发中，如何处理比较耗时的接口呢？">}}

是不是大家第一反应是上异步接口，使用`Callable`、`WebAsyncTask`和`DeferredResult`、`CompletableFuture`等均可实现。

{{< /admonition >}}

但这些方法有局限性，处理结果均仅返回单个值。某些场景中，如果需要接口异步处理同时，还持续不断地向客户端响应处理结果，这些方法就不够看了。

Spring 框架提供了多种工具支持异步流式接口，如`ResponseBodyEmitter`、`SeeEmitter`和`StreamingResponseBody`。这些工具的用法简单，接口中直接返回相应的对象或泛型响应实体`ResponseEntity<XXX>`，如此这些接口就是异步的，且执行耗时操作时亦不会阻塞 `Servlet`的请求线程，不影响系统的响应能力。

下面将逐一介绍一下每个工具的使用及其应用场景。

<br>

## ResponseBofyEmitter

`ResponseBodyEmitter`适用于要动态生成内容并逐步发送给客户端的场景，例如：文件上传进度、实时日志等，可以在任务执行过程中逐步向客户端发送更新。

举个例子，经常用GPT你会发现当你提问后，得到的答案并不是一次性响应呈现的，而是逐步动态显示。这样做的好处是，让你感觉它在认真思考、交互体验比直接返回完整答案更为生动和自然。

![GPT回答问题](https://cdn.jsdelivr.net/gh/Turbo-King/images/GPT%E5%9B%9E%E7%AD%94%E9%97%AE%E9%A2%98.gif "GPT回答问题")

使用`ResponseBodyEmitter`来实现这个效果并不难，首先创建`ResponseBodyEmitter`发送器对象，模拟耗时操作逐步调用 send 方法发送消息。

> 注意：**ResponseBodyEmitter**的超时时间，如果设置为 0 或 -1，则表示连接不会超时；如果不设置，到达默认的超时时间后连接会自动断开。其他两种工具也是同样的用法，后面不再进行赘述了。

```java
@GetMapping("/bodyEmitter")
public ResponseBodyEmitter handle() {
    // 创建一个ResponseBodyEmitter，-1代表不超时
    ResponseBodyEmitter emitter = new ResponseBodyEmitter(-1L);
    // 异步执行耗时操作
    CompletableFuture.runAsync(() -> {
        try {
            // 模拟耗时操作
            for (int i = 0; i < 10000; i++) {
                System.out.println("bodyEmitter " + i);
                // 发送数据
                emitter.send("bodyEmitter " + i + " @ " + new Date() + "\n");
                Thread.sleep(2000);
            }
            // 完成
            emitter.complete();
        } catch (Exception e) {
            // 发生异常时结束接口
            emitter.completeWithError(e);
        }
    });
    return emitter;
}
```

实现代码非常简单。通过模拟每2秒响应一次结果，请求接口时可以看到页面数据在动态生成。效果和 GPT 回答基本一致。

![ResponseBodyEmitter使用效果](https://cdn.jsdelivr.net/gh/Turbo-King/images/ResponseBodyEmitter%E4%BD%BF%E7%94%A8%E6%95%88%E6%9E%9C.gif "ResponseBodyEmitter使用效果")

<br>

## SseEmitter

`SeeEmitter`是`ResponseBodyEmitter`的一个子类，它同样能够实现动态内容生成，不过主要将它用在 **服务器向客户端** 推送实时数据，例如：消息推送、状态更新等场景。

SSE在服务器和客户端之间打开一个单向通道，服务端响应的不再是一次性的数据包而是`text/event-stream`类型的数据流信息，在有数据变更时从服务器流式传输到客户端。

![SSE方式传输图](https://cdn.jsdelivr.net/gh/Turbo-King/images/SSE%E6%96%B9%E5%BC%8F%E4%BC%A0%E8%BE%93%E5%9B%BE.jpg “SSE方式传输图”)

整体的实现思路有点类似于在线视频播放，视频流会连续不断地推送到浏览器，你也可以理解成，客户端在完成一次用时很长（网络不畅）的下载。

客户端JS实现，通过一次 HTTP 请求建立连接后，等待接收消息。此时，服务端为每个连接创建一个`SseEmitter`对象，通过这个通道向客户端发送消息。

```html
<body>
<div id="content" style="text-align: center;">
    <h1>SSE 接收服务端事件消息数据</h1>
    <div id="message">等待连接...</div>
</div>
<script>
    let source = null;
    let userId = 7777

    function setMessageInnerHTML(message) {
        const messageDiv = document.getElementById("message");
        const newParagraph = document.createElement("p");
        newParagraph.textContent = message;
        messageDiv.appendChild(newParagraph);
    }

    if (window.EventSource) {
        // 建立连接
        source = new EventSource('http://127.0.0.1:9033/subSseEmitter/'+userId);
        setMessageInnerHTML("连接用户=" + userId);
        /**
         * 连接一旦建立，就会触发open事件
         * 另一种写法：source.onopen = function (event) {}
         */
        source.addEventListener('open', function (e) {
            setMessageInnerHTML("建立连接。。。");
        }, false);
        /**
         * 客户端收到服务器发来的数据
         * 另一种写法：source.onmessage = function (event) {}
         */
        source.addEventListener('message', function (e) {
            setMessageInnerHTML(e.data);
        });
    } else {
        setMessageInnerHTML("你的浏览器不支持SSE");
    }
</script>
</body>
```

在服务端，我们将`SseEmitter`发送器对象进行持久化，以便在消息产生时直接取出对应的`SseEmitter`发送器，并调用`send`方法进行推送。

```java
private static final Map<String, SseEmitter> EMITTER_MAP = new ConcurrentHashMap<>();

@GetMapping("/subSseEmitter/{userId}")
public SseEmitter sseEmitter(@PathVariable String userId) {
    log.info("sseEmitter: {}", userId);
    SseEmitter emitterTmp = new SseEmitter(-1L);
    EMITTER_MAP.put(userId, emitterTmp);
    CompletableFuture.runAsync(() -> {
        try {
            SseEmitter.SseEventBuilder event = SseEmitter.event()
                    .data("sseEmitter" + userId + " @ " + LocalTime.now())
                    .id(String.valueOf(userId))
                    .name("sseEmitter");
            emitterTmp.send(event);
        } catch (Exception ex) {
            emitterTmp.completeWithError(ex);
        }
    });
    return emitterTmp;
}

@GetMapping("/sendSseMsg/{userId}")
public void sseEmitter(@PathVariable String userId, String msg) throws IOException {
    SseEmitter sseEmitter = EMITTER_MAP.get(userId);
    if (sseEmitter == null) {
        return;
    }
    sseEmitter.send(msg);
}
```

而且SSE有一点比较好，客户端与服务端一旦建立连接，即便服务端发生重启，也可以做到自动重连。

![SSE重连](https://cdn.jsdelivr.net/gh/Turbo-King/images/SSE%E9%87%8D%E8%BF%9E.jpg "SSE重连")

<br>

## StreamingResponseBody

`StreamingResponseBody`与其他响应处理方式略有不同，主要用于处理大数据量或持续数据流的传输，支持将数据直接写入`OutputStream`。

例如：当我们需要下载一个超大文件时，使用`StreamingResponseBody`可以避免将文件数据一次性加载到内存中，而是持续不断的把文件流发送给客户端，从而解决下载超大文件时常见的内存溢出问题。

接口实现直接返回`StreamingResponseBody`对象，将数据写入输出流并刷新，调用一次`flush`就会向客户端写入一次数据。

```java
@GetMapping("/streamingResponse")
public ResponseEntity<StreamingResponseBody> handleRbe() {

    StreamingResponseBody stream = out -> {
        String message = "streamingResponse";
        for (int i = 0; i < 1000; i++) {
            try {
                out.write(((message + i) + "\r\n").getBytes());
                out.write("\r\n".getBytes());
                //调用一次flush就会像前端写入一次数据
                out.flush();
                TimeUnit.SECONDS.sleep(1);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    };
    return ResponseEntity.ok().contentType(MediaType.TEXT_HTML).body(stream);
}
```

demo这里输出的是简单的文本流，如果是下载文件那么转换成文件流效果一样。

![StreamingResponseBody实现](https://cdn.jsdelivr.net/gh/Turbo-King/images/StreamingResponseBody%E5%AE%9E%E7%8E%B0.gif "StreamingResponseBody实现")

<br>

## 总结

本文介绍三种实现异步流式接口的工具，算是 Spring 知识点的扫盲。使用起来比较简单，没有什么难点，但它们在实际业务中的应用场景还是很多的，通过这些工具，可以有效提高系统的性能和响应能力。










