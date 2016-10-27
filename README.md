
Code a .net Core WebApi on Ubuntu with Visual Studio Code.
And Host it on a docker.

So let's start

## Install Visual Studio Code
It's easy, go to [Download Visual Studio Code .deb](https://code.visualstudio.com/) and install it.

You will need to code in .Net to get the extension C#
![VSCode Extension](https://github.com/Tkanos/dotnetCoreWebApiDocker/blob/master/img/VSCodeExtension.png)

## Install dotnet

You have all information [here](https://www.microsoft.com/net/core#ubuntu)

For my Ubuntu 16.04 I've done :
```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'

sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
sudo apt-get update

sudo apt-get install dotnet-dev-1.0.0-preview2-003131
```

type the command dotnet to check if all went well.
```bash
dotnet --help
```

## Your first webapi coded on ubuntu

So first create a directory where you will code your first web api on ubuntu
```bash
mkdir FirstWebApi
cd FirstWebApi
```

Then we will create our dotnet solution.
```bash
dotnet new
```

Open thanks to Visual Studio Code your FirstWebApi folder.
On project.json we will need to add the dependencies to work with AspNetCore and Kestrel (for those who don't know Kestrel is a web server on ubuntu instead of using IIS).
```json
"Microsoft.AspNetCore.Mvc": "1.0.0",
"Microsoft.AspNetCore.Server.Kestrel": "1.0.0"
```

on Program.cs we will say to use Kestrel and search for all config on Startup.cs
```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Build();

host.Run(); 
```

I've also changed the namespace to put a more funy one (but it's really not needed), 

So now, we need to create the Startup.cs file and put inside the configuration : 
```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.Extensions.DependencyInjection;

namespace HelloUniverse
{
    public class Startup
    {
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddMvc();
        }

        public void Configure(IApplicationBuilder app)
        {
            app.UseMvc();
        }
    }
}
```

Now We are ready to code our Api Controller.
I've added a new Folder called Controller, and inside i've create a file HelloWorldController.cs

```csharp
using Microsoft.AspNetCore.Mvc;
namespace HelloUniverse
{
    [Route("api/[controller]")]
    public class HelloWorldController : Controller
    {
        // GET api/HelloWorld
        [HttpGet]
        public string Get()
        {
            return "Hello, world!";
        }
    }
}
```

Yeah I know it's very simple.

there are many ways to [setup Kestrel](http://benfoster.io/blog/how-to-configure-kestrel-urls-in-aspnet-core-rc2)
But i prefer to open the project.json file and add

```json
"web" : "Microsoft.AspNetCore.Server.Kestrel --server.urls http://0.0.0.0:5000"
```





## Run

## Dockerize it.

## Remarks
dotnet core is on preview.
So there are missing all the tooling.
Kestrel need more tooling too.

So if i ahve to do a real project maybe i will think to put nginx in front and kestrel behind, to benefit of the tooling of nginx.

And Visual Studio Code without templating is a pain (we can use yeoman), but without a performant templating tool it will be hard to code in ubuntu.

## Links
- [how-to-configure-kestrel-urls-in-aspnet-core-rc2](http://benfoster.io/blog/how-to-configure-kestrel-urls-in-aspnet-core-rc2)



