流式输出（streaming output）接口的测试方式主要取决于具体的实现方式（如 SSE、WebSocket、HTTP Chunked、gRPC Streaming 等）。以下是常见的测试方法：

1. 手动测试

适用于快速验证流式数据：

- cURL
  （适用于 SSE、HTTP chunked）：
      curl -N http://your-api-endpoint
      -N
   选项用于保持连接不关闭，观察流式输出。
- WebSocket 客户端
  （如 Postman、wscat）：
      wscat -c ws://your-websocket-endpoint

2. Python 脚本测试

如果是 HTTP 流式接口（如 SSE 或 Chunked Response）：

    import requests
    
    url = "http://your-api-endpoint"
    with requests.get(url, stream=True) as response:
        for line in response.iter_lines():
            if line:
                print(line.decode('utf-8'))

如果是 WebSocket 流：

    import websocket
    
    def on_message(ws, message):
        print(f"Received: {message}")
    
    ws = websocket.WebSocketApp("ws://your-websocket-endpoint", on_message=on_message)
    ws.run_forever()

3. Postman / API 测试工具

- Postman 支持 SSE/WebSocket，可以直接发送请求并观察流式返回的数据。
- Insomnia 也可以用于 WebSocket 测试。

4. 自动化测试（pytest + requests / WebSocket）

如果要做自动化测试，可以使用 pytest：

    import requests
    
    def test_streaming_api():
        url = "http://your-api-endpoint"
        with requests.get(url, stream=True) as response:
            assert response.status_code == 200
            for line in response.iter_lines():
                assert line is not None  # 确保有数据返回

对于 WebSocket：

    import websocket
    
    def test_websocket():
        ws = websocket.WebSocket()
        ws.connect("ws://your-websocket-endpoint")
        ws.send("ping")
        message = ws.recv()
        assert message is not None
        ws.close()

5. Mock 测试

如果要模拟流式 API，可以用 FastAPI + pytest：

    from fastapi import FastAPI
    from fastapi.responses import StreamingResponse
    import time
    
    app = FastAPI()
    
    def event_stream():
        for i in range(5):
            yield f"data: {i}\n\n"
            time.sleep(1)
    
    @app.get("/stream")
    async def stream():
        return StreamingResponse(event_stream(), media_type="text/event-stream")

然后使用 pytest 进行测试。

6. 性能测试（JMeter / Locust）

如果要做流式接口的性能测试：

- JMeter：使用 WebSocket / HTTP Sampler 进行并发测试。
- Locust
  （适用于 WebSocket）：
      from locust import User, task
      from websocket import create_connection
      
      class WebSocketUser(User):
          @task
          def ws_test(self):
              ws = create_connection("ws://your-websocket-endpoint")
              ws.send("ping")
              ws.recv()
              ws.close()

7. 日志 & Debug

如果流式接口有问题：

- 检查服务器日志：是否正常推送数据？
- 网络抓包：使用 tcpdump 或 Wireshark 看流数据。
- 超时 & 断连问题：确保客户端不会因为 timeout 过早断开。

你是想测试哪种类型的流式接口？HTTP SSE、WebSocket 还是其他类型？
