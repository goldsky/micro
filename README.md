# Micro [![License](https://img.shields.io/:license-apache-blue.svg)](https://opensource.org/licenses/Apache-2.0) [![GoDoc](https://godoc.org/github.com/micro/micro?status.svg)](https://godoc.org/github.com/micro/micro) [![Travis CI](https://travis-ci.org/micro/micro.svg?branch=master)](https://travis-ci.org/micro/micro) [![Go Report Card](https://goreportcard.com/badge/micro/micro)](https://goreportcard.com/report/github.com/micro/micro)


Micro is a **microservice** toolkit. Its purpose is to simplify distributed systems development.

Check out [**go-micro**](https://github.com/micro/go-micro) if you want to start writing services in Go now or [**ja-micro**](https://github.com/Sixt/ja-micro) for Java. Examples of how to use micro with other languages can be found in [examples/sidecar](https://github.com/micro/examples/tree/master/sidecar).

Learn more about Micro in the introductory blog post [https://micro.mu/blog/2016/03/20/micro.html](https://micro.mu/blog/2016/03/20/micro.html) or watch the talk from the [Golang UK Conf 2016](https://www.youtube.com/watch?v=xspaDovwk34).

Follow us on Twitter at [@MicroHQ](https://twitter.com/microhq) or join us on [Slack](http://slack.micro.mu/).

# Overview
The goal of **Micro** is to simplify distributed systems development. Micro makes writing microservices accessible to everyone, and as you scale, micro will provide the necessary tooling to manage a microservice environment.

The toolkit is composed of the following components:

- **Go Micro** - A pluggable RPC framework for writing microservices in Go.

- **Sidecar** - A go-micro proxy. All the features of go-micro over HTTP.

- **API** - An API Gateway. A single HTTP entry point. Dynamically routing HTTP requests to RPC services.

- **Web** - A web UI for exploring. Plus a reverse proxy for web apps! Build your web apps as micro services.

- **CLI** - A command line interface. Interact with your micro services. 

- **Bot** - A bot for slack and hipchat. CLI equivalent via messaging.


## Docs

For more detailed information on the architecture, installation and use of the toolkit checkout the [docs](https://micro.mu/docs).

## Getting Started

### Writing a service

Learn how to write and run a microservice using [**go-micro**](https://github.com/micro/go-micro). 

Read the [Getting Started](https://micro.mu/docs/writing-a-go-service.html) guide or the blog post on 
[Writing microservices with Go-Micro](https://micro.mu/blog/2016/03/28/go-micro.html).

### Install Micro

```shell
go get -u github.com/micro/micro
```

Or via Docker

```shell
docker pull microhq/micro
```

### Dependencies

We need service discovery, so let's spin up Consul (the default); checkout [go-plugins](https://github.com/micro/go-plugins) to swap it out.

Mac OS
```shell
brew install consul
consul agent -dev
```

Docker
```shell
docker run -d -p 8500:8500 --name=consul consul agent -server -bootstrap -client=0.0.0.0
```

Or we can use multicast DNS for zero dependency discovery

Pass `--registry=mdns` to the below commands e.g `micro --registry=mdns list services`

### Try CLI

Run the greeter service

```shell
go get github.com/micro/examples/greeter/srv && srv
```

List services
```shell
$ micro list services
consul
go.micro.srv.greeter
```

Get Service
```shell
$ micro get service go.micro.srv.greeter
service  go.micro.srv.greeter

version 1.0.0

Id	Address	Port	Metadata
go.micro.srv.greeter-34c55534-368b-11e6-b732-68a86d0d36b6	192.168.1.66	62525	server=rpc,registry=consul,transport=http,broker=http

Endpoint: Say.Hello
Metadata: stream=false

Request: {
	name string
}

Response: {
	msg string
}
```

Query service
```shell
$ micro query go.micro.srv.greeter Say.Hello '{"name": "John"}'
{
	"msg": "Hello John"
}
```

Read the [docs](https://micro.mu/docs) to learn more about entire toolkit.

## Build with plugins

If you want to integrate plugins simply link them in a separate file and rebuild

Create a plugins.go file
```go
import (
	// etcd v3 registry
	_ "github.com/micro/go-plugins/registry/etcdv3"
	// nats transport
	_ "github.com/micro/go-plugins/transport/nats"
	// kafka broker
	_ "github.com/micro/go-plugins/broker/kafka"
)
```

Build binary
```shell
// For local use
go build -i -o micro ./main.go ./plugins.go

// For docker image
CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -ldflags '-w' -i -o micro ./main.go ./plugins.go
```

Flag usage of plugins
```shell
micro --registry=etcdv3 --transport=nats --broker=kafka
```

## Community Contributions

Project		|	Description
-----		|	------
[Micro Dashboard](https://github.com/Margatroid/micro-dashboard)	|	Dashboard for microservices toolchain micro
[Ja-Micro](https://github.com/Sixt/ja-micro)	|	A micro compatible java framework for microservices

## Sponsors

Open source development of Micro is sponsored by Sixt

<a href="https://micro.mu/blog/2016/04/25/announcing-sixt-sponsorship.html"><img src="https://micro.mu/sixt_logo.png" width=150px height="auto" /></a>

