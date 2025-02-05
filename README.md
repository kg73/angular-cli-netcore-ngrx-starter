[![Build status](https://freshpondmedia.visualstudio.com/FreshPondMediaGit/_apis/build/status/AngularCliNetcoreNgrxStarter%20-%20CI)](https://freshpondmedia.visualstudio.com/FreshPondMediaGit/_build/latest?definitionId=39)

# Angular 8 + Angular Cli 8 + NgRx Platform + .NET Core 2.2.0 + Server Side Rendering (optional) Starter
This is basic demo of how to use an Angualr SPA and .NET Core 2.2.0 with [Microsoft.AspNetCore.SpaServices.Extensions](https://docs.microsoft.com/en-us/aspnet/core/spa/).

### Demo
See a live demo here: [https://angularclinetcorengrxstarter.azurewebsites.net/](https://angularclinetcorengrxstarter.azurewebsites.net/)

# Getting Started?

- **Make sure you have at least Node 8.x or higher (w/ npm 5+) installed!**  
- **This repository uses ASP.Net Core 2.2.0, which has a hard requirement on .NET Core SDK 2.2.103. Please install these items from [here](https://www.microsoft.com/net/learn/get-started/windows)**


### Visual Studio 2017

Make sure you have .NET Core 2.2.0 installed and/or VS2017 15.9.1.
VS2017 should automatically install all the necessary npm & .NET dependencies when you open the project.

Simply push F5 to start debugging !

### Visual Studio Code

> Note: Make sure you have the C# extension & .NET Core Debugger installed.

The project comes with the configured Launch.json files to let you just push F5 to start the project. Keep in mind this may take a bit the first time the project is run, as all of the npm + NuGet packages need to be installed and the .NET and Angular client apps need to be built.

To run the app via the terminal, cd into the directory you cloned the project into then into the ./ClinetApp/ directory. In this example, the "ClientApp" folder contains a pretty standard Angular CLI generated app with the necessary modifications for .NET Core SSR and the NgRx platform components added. The npm dependencies should be installed when you do a "dotnet build", but it's never a bad idea to just do it manually to make any unexpected installation errors evident.

```bash
# cd ./ClientApp/
npm install
``` 

To then start the app, cd ../ (or back into the root directory). The root directory contains a fairly standard .NET Web App, with the main difference being that the new Microsoft.AspNetCore.SpaServices.Extensions serves the Angular app directly from the CLI genreated index.html, not the more typical "_Layout.cshtml" + "Views/Home/Index.cshtml" + "Controllers/HomeController.cs" setup.

```bash
# cd ../
dotnet restore

# The package.json in the root folder calls "dotnet run" 
# with a pre-configured launch profile to run in dev mode.
npm start
``` 

### SSR (Server Side Rendering)

To enable SSR, simply uncomment the following block of code in startup.cs:

    // spa.UseSpaPrerendering(options =>
    // {
    //     options.BootModulePath = $"{spa.Options.SourcePath}/dist-server/main.bundle.js";
    //     options.BootModuleBuilder = env.IsDevelopment()
    //         ? new AngularCliBuilder(npmScript: "build:ssr")
    //         : null;
    //     options.ExcludeUrls = new[] { "/sockjs-node" };
    // });
   
  And enable this flag in the .csproj file so the server version of client app will be built upon publishing, and the necessary node_modules folder will be deployed:

    <!-- Set this to true if you enable server-side prerendering -->
    <BuildServerSideRenderer>false</BuildServerSideRenderer>

### Run Production App

To run the production app locally, you'll first need to build the client app in production mode, and the server app if using SSR.

#### Not Using SSR

Build the client app in production mode:

```bash
# cd ./ClientApp/
npm run build:prod
``` 
Then start dotnet in production mode by using the supplied launch settings and npm script to load them:

```bash
# cd ../
npm run dotnet:run:prod
``` 

#### Using SSR

Build the client app and server app in production mode:

```bash
# cd ./ClientApp/
npm run build:prod
npm run build:ssr:prod
``` 
Then start dotnet in production mode by using the supplied launch settings and npm script to load them:

```bash
# cd ../
npm run dotnet:run:prod
``` 