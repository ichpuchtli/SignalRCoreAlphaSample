## Basic chat for ASP.NET Core SignalR alpha

## Client

```javascript
let http = new signalR.HttpConnection('http://' + document.location.host + '/chat');
let connection = new signalR.HubConnection(http);

connection.on('Send', data => {
    console.log(data);
});

connection.start()
    .then(() => connection.invoke('Send', 'Hello'));
```

## Server 

```C#
using System;
using System.Threading.Tasks;
using Microsoft.AspNetCore.SignalR;

namespace Sample
{
    public class Chat : Hub
    {
        public Task Send(string message)
        {
            return Clients.All.InvokeAsync("Send", message);
        }
    }
}
```

```C#
using Microsoft.AspNetCore.Builder;
using Microsoft.Extensions.DependencyInjection;

namespace Sample
{
    public class Startup
    {
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddSignalR();
        }

        public void Configure(IApplicationBuilder app)
        {
            app.UseSignalR(routes =>
            {
                routes.MapHub<Chat>("chat");
            });
        }
    }
}

```
