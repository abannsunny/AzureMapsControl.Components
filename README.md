![Nuget](https://img.shields.io/nuget/v/AzureMapsControl.Components) ![Nuget (with prereleases)](https://img.shields.io/nuget/vpre/AzureMapsControl.Components) ![CI](https://github.com/arnaudleclerc/AzureMapsControl.Components/workflows/CI/badge.svg) ![release](https://github.com/arnaudleclerc/AzureMapsControl.Components/workflows/release/badge.svg)

This library allows you to use `Azure Maps` inside your razor application.

## Install the Nuget Package

This library is available on Nuget as `AzureMapsControl.Components`.

## Setup

### Add the css and scripts

You will need to add the atlas script and css files as well as the script generated by the library on your application.


```
<link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css" />
```

```
<script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>
<script src="_content/AzureMapsControl.Components/azure-maps-control.js"></script>
```

Or use the minimized version : 

```
<script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>
<script src="_content/AzureMapsControl.Components/azure-maps-control.min.js"></script>
```

If you plan to use the drawing toolbar, you also need to include the following css and scripts :

```
<link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/drawing/0.1/atlas-drawing.min.css" type="text/css" />
```

```
<script src="https://atlas.microsoft.com/sdk/javascript/drawing/0.1/atlas-drawing.min.js"></script>
```

### Register the Components

You will need to pass the authentication information of your `AzureMaps` instance to the library. `SubscriptionKey`, `Aad` and `Anonymous` authentication are supported. You will need to call the `AddAzureMapsControl` method on your services.

You can authenticate using a `subscription key` :

```
    // This method gets called by the runtime. Use this method to add services to the container.
    // For more information on how to configure your application, visit https://go.microsoft.com/fwlink/?LinkID=398940
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddRazorPages();
        services.AddServerSideBlazor();
        
        services.AddAzureMapsControl(configuration => configuration.SubscriptionKey = "Your Subscription Key");
    }
```

Or using `Azure Active Directory`:

```
public void ConfigureServices(IServiceCollection services)
        {
            services.AddRazorPages();
            services.AddServerSideBlazor(options => options.DetailedErrors = true);

            services.AddAzureMapsControl(configuration => {
                configuration.AadAppId = "Your Aad App Id";
                configuration.AadTenant = "Your Aad Tenant";
                configuration.ClientId = "Your Client Id";
            });
        }
```

The `Anonymous` authentication requires only a `ClientId`:

```
public void ConfigureServices(IServiceCollection services)
        {
            services.AddRazorPages();
            services.AddServerSideBlazor(options => options.DetailedErrors = true);

            services.AddAzureMapsControl(configuration => configuration.ClientId = Configuration["AzureMaps:ClientId"])
        }
```

It also needs to fetch the token to send to the requests of the atlas library. For that, you have to override the `azureMapsControl.Extensions.getTokenCallback` method on your application after referencing `azure-maps-control.min.js` and resolve the token in it. For example : 

```
@page "/"
@namespace AzureMapsControl.Sample.Pages
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@{
    Layout = null;
}

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>AzureMapsControl.Sample</title>
    <base href="~/" />
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css" />
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/drawing/0.1/atlas-drawing.min.css" type="text/css" />
    <style>
        body {
            margin: 0;
        }

        #map {
            position: absolute;
            width: 100%;
            min-width: 290px;
            height: 100%;
        }
    </style>
</head>
<body>
    <app>
        <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>
    <script src="https://atlas.microsoft.com/sdk/javascript/drawing/0.1/atlas-drawing.min.js"></script>
    <script src="_content/AzureMapsControl.Components/azure-maps-control.min.js"></script>
    <script src="_framework/blazor.server.js"></script>
    <script type="text/javascript">
        azureMapsControl.Extensions.getTokenCallback = (resolve, reject, map) => {
            const url = "url_of_my_token_endpoint";
            fetch(url).then(function (response) {
                return response.text();
            }).then(function (token) {
                resolve(token);
            });        
        };
    </script>
</body>
</html>
```

## How to use

- [Map](docs/map)
- [Controls](docs/controls)
- [Drawing Toolbar](docs/drawingtoolbar)
- [Html Markers](docs/htmlmarkers)
- [Layers](docs/layers)
- [Popups](docs/popups)
- [Traffic](docs/traffic)
