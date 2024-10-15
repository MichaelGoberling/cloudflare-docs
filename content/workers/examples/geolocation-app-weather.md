---
type: example
summary: Fetch weather data from an API using the user's geolocation data.
tags:
  - Geolocation
languages:
  - JavaScript
  - TypeScript
  - Python
pcx_content_type: example
title: "Geolocation: Weather application"
weight: 1001
layout: example
---

{{<tabs labels="js | ts | py">}}
{{<tab label="js" default="true">}}

```js
export default {
  async fetch(request) {
    let endpoint = "https://api.waqi.info/feed/geo:";
    const token = ""; //Use a token from https://aqicn.org/api/
    let html_style = `body{padding:6em; font-family: sans-serif;} h1{color:#f6821f}`;

    let html_content = "<h1>Weather 🌦</h1>";

    const latitude = request.cf.latitude;
    const longitude = request.cf.longitude;
    endpoint += `${latitude};${longitude}/?token=${token}`;
    const init = {
      headers: {
        "content-type": "application/json;charset=UTF-8",
      },
    };

    const response = await fetch(endpoint, init);
    const content = await response.json();

    html_content += `<p>This is a demo using Workers geolocation data. </p>`;
    html_content += `You are located at: ${latitude},${longitude}.</p>`;
    html_content += `<p>Based off sensor data from <a href="${content.data.city.url}">${content.data.city.name}</a>:</p>`;
    html_content += `<p>The AQI level is: ${content.data.aqi}.</p>`;
    html_content += `<p>The N02 level is: ${content.data.iaqi.no2?.v}.</p>`;
    html_content += `<p>The O3 level is: ${content.data.iaqi.o3?.v}.</p>`;
    html_content += `<p>The temperature is: ${content.data.iaqi.t?.v}°C.</p>`;

    let html = `
      <!DOCTYPE html>
      <head>
        <title>Geolocation: Weather</title>
      </head>
      <body>
        <style>${html_style}</style>
        <div id="container">
        ${html_content}
        </div>
      </body>`;

    return new Response(html, {
      headers: {
        "content-type": "text/html;charset=UTF-8",
      },
    });
  },
};
```

{{</tab>}}
{{<tab label="ts">}}

```ts
export default {
  async fetch(request): Promise<Response> {
    let endpoint = "https://api.waqi.info/feed/geo:";
    const token = ""; //Use a token from https://aqicn.org/api/
    let html_style = `body{padding:6em; font-family: sans-serif;} h1{color:#f6821f}`;

    let html_content = "<h1>Weather 🌦</h1>";

    const latitude = request.cf.latitude;
    const longitude = request.cf.longitude;
    endpoint += `${latitude};${longitude}/?token=${token}`;
    const init = {
      headers: {
        "content-type": "application/json;charset=UTF-8",
      },
    };

    const response = await fetch(endpoint, init);
    const content = await response.json();

    html_content += `<p>This is a demo using Workers geolocation data. </p>`;
    html_content += `You are located at: ${latitude},${longitude}.</p>`;
    html_content += `<p>Based off sensor data from <a href="${content.data.city.url}">${content.data.city.name}</a>:</p>`;
    html_content += `<p>The AQI level is: ${content.data.aqi}.</p>`;
    html_content += `<p>The N02 level is: ${content.data.iaqi.no2?.v}.</p>`;
    html_content += `<p>The O3 level is: ${content.data.iaqi.o3?.v}.</p>`;
    html_content += `<p>The temperature is: ${content.data.iaqi.t?.v}°C.</p>`;

    let html = `
      <!DOCTYPE html>
      <head>
        <title>Geolocation: Weather</title>
      </head>
      <body>
        <style>${html_style}</style>
        <div id="container">
        ${html_content}
        </div>
      </body>`;

    return new Response(html, {
      headers: {
        "content-type": "text/html;charset=UTF-8",
      },
    });
  },
} satisfies ExportedHandler;
```

{{</tab>}}
{{<tab label="py">}}

```py
from js import Response, Headers, fetch

async def on_fetch(request):
    endpoint = "https://api.waqi.info/feed/geo:"
    token = "" # Use a token from https://aqicn.org/api/
    html_style = "body{padding:6em; font-family: sans-serif;} h1{color:#f6821f}"
    html_content = "<h1>Weather 🌦</h1>"

    latitude = request.cf.latitude
    longitude = request.cf.longitude

    endpoint += f"{latitude};{longitude}/?token={token}"
    response = await fetch(endpoint)
    content = await response.json()

    html_content += "<p>This is a demo using Workers geolocation data. </p>"
    html_content += f"You are located at: {latitude},{longitude}.</p>"
    html_content += f"<p>Based off sensor data from <a href='{content.data.city.url}'>{content.data.city.name}</a>:</p>"
    html_content += f"<p>The AQI level is: {content.data.aqi}.</p>"
    html_content += f"<p>The N02 level is: {content.data.iaqi.no2.v}.</p>"
    html_content += f"<p>The O3 level is: {content.data.iaqi.o3.v}.</p>"
    html_content += f"<p>The temperature is: {content.data.iaqi.t.v}°C.</p>"

    html = f"""
    <!DOCTYPE html>
      <head>
        <title>Geolocation: Weather</title>
      </head>
      <body>
        <style>{html_style}</style>
        <div id="container">
        {html_content}
        </div>
      </body>
    """

    headers = Headers.new({"content-type": "text/html;charset=UTF-8"}.items())
    return Response.new(html, headers=headers)
```

{{</tab>}}
{{</tabs>}}