# [C# 实现WebSocket通信](https://www.cnblogs.com/swjian/p/10553689.html)

　本实例可通过web网页端进行测试，下面直接上代码。

　首先要在NuGet导入“Fleck”包，需 .NET Framework 4.5及以上。

## C#端
```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading;

namespace Fleck.Samples.ConsoleApp
{
    class Server
    {
        static void Main()
        {
            FleckLog.Level = LogLevel.Debug;
            var allSockets = new List<IWebSocketConnection>();
            var server = new WebSocketServer("ws://127.0.0.1:50000");
            server.Start(socket =>
                {
                    socket.OnOpen = () =>
                        {
                            Console.WriteLine("Open!");
                            allSockets.Add(socket);
                        };
                    socket.OnClose = () =>
                        {
                            Console.WriteLine("Close!");
                            allSockets.Remove(socket);
                        };
                    socket.OnMessage = message =>
                        {
                            Console.WriteLine(message);
                            allSockets.ToList().ForEach(s => s.Send("Echo: " + message));
                        };
                });


            var input = Console.ReadLine();
            while (input != "exit")
            {
                foreach (var socket in allSockets.ToList())
                {
                    socket.Send(input);
                }
                input = Console.ReadLine();
            }

        }
    }
}
```

## h5端
```js
ws = new WebSocket("ws://127.0.0.1:50000");
ws.onopen = function() { 
    ws.send('websocekt测试'); 
};
ws.onmessage = function(e) {
    alert("收到服务端的消息：" + e.data);
};
```

**使用ipv4**

