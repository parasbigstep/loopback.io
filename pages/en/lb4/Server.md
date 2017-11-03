---
lang: en
title: 'Server'
keywords: LoopBack 4.0, LoopBack 4
tags:
sidebar: lb4_sidebar
permalink: /doc/en/lb4/Server.html
summary:
---

## Overview

The [Server](https://apidocs.strongloop.com/@loopback%2fcore/#Server) interface defines the minimal required functions (start and stop) to implement for a LoopBack application. The server could have its own transport protocol, and has its own child Context inherited from Application context. You can overwrite the bindings inherited from the parent Application context, such as the port the server
will listen on. This way, any Server-specific bindings will remain local to the Server instance, and will avoid polluting its parent module scope.


## Usage

LoopBack comes with an HTTP based server implementation handling requests over REST called `RestServer`. All you need to do is include the `RestComponent` as a component when you call the base Application class, and it will provide you with an instance of RestServer listening on port 3000.

```ts
import {Application} from '@loopback/core';
import {RestComponent, RestServer} from '@loopback/rest';

export class HelloWorldApp extends Application {
  constructor() {
    super({
      components: [RestComponent],
    });
  }

  async start() {
    const rest = await this.getServer(RestServer);
    rest.handler((sequence, request, response) => {
      sequence.send(response, 'Hello World!');
    });
    await super.start();
    console.log(`REST server running on port: ${await rest.get('rest.port')}`);
  }
}
```
