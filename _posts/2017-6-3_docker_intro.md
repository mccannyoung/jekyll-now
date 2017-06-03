---
layout: post
title: Introduction to Docker for Go
---

I use [Docker](https://www.docker.com/what-docker) professionally. Honestly, I'm a bit of a fangirl of it. Why? Because, like most developers, I am very tired of something working on my computer just fine, and then not working on someone else's or in testing, because of environmental differences. This is a reason why, if I were to set up a company from scratch, I'd use [cloud9](https://c9.io/) for the developer's development environment, and keep all of the systems in sync. I want to just write software, get it working, and not need my blood pressure to go up when it's time for it to be deployed into a new environment (test vs staginng vs production). 

I was first introduced to Docker when I began working on an SMS project. I experimented with some software called [JasminSMS](http://www.jasminsms.com), and there were numerous instructions for how to install it, almost all of the instructions were geared toward Linux, and I run Windows 10 and don't like working in a VM very much, except for the docker instructions, so I had a reason to learn how to run a Docker app. It was simplier than what I was expecting. I got our architect on board with docker, and it is being used for all new development at our company.

Anyway, let's write some code and put it in a docker container!

```golang

package main

import (
	"fmt"
	"net/http"
	"os"
)

func handler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "Hello World!")
}

func pingponghandler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "pong")

}

func main() {
	http.HandleFunc("/ping", pingponghandler)
	http.HandleFunc("/", handler)
	http.ListenAndServe(":"+os.Getenv("PORT"), nil)
}
```

Because I wrote this for cloud9, I have the port gathered from the os, if you're running this on your local machine, you can just type in 5000 or whatever port you want it to run on.  This program has two functions, if you go to http://localhost:5000/ it will return "Helllo World" in the browser window, if you go to http://localhost:5000/ping, it will return  "pong". 

Now if you want to write a Dockerfile for it, to create a Docker image to run in a Docker container - 

```
FROM golang:1.8.0

RUN mkdir /pingpong
ADD . /pingpong/ 

# Set up gopath, otherwise everything will blow up
ENV GOPATH $HOME/goApps  
ENV PATH $HOME/goApps/bin:$PATH

WORKDIR /pingpong/

RUN go build 

RUN ls /pingpong

EXPOSE 5000 # or whatever port you've picked

CMD "/pingpong/pingpong"

```

Now, if you're importing packages from github, don't forget to add a line to RUN go get *whatever*

To build the container - you can elect something as simple as 

```
docker build .
```

And pay attention to the last line, it will look something like this:
Successfully built da898d6d1c93

to run this build:

```
docker run -p 5000:5000 <first four or so letters/numbers of the built image ID>
```

Check out the documentation on [build](https://docs.docker.com/engine/reference/commandline/build/) and [run](https://docs.docker.com/engine/reference/commandline/run/) for more options. 
