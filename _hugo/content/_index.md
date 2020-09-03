+++
title = "Go Course"
outputs = ["Reveal"]
+++

# Go Course

Adapted from https://go-course.org

# Prerequisites

- No prior Go knowledge required
- Some programming experience strongly recommended
- Basic understanding of *JSON* and *RESTful* *APIs* recommended
- Install [[https://golang.org/doc/install][go 1.12]]
- Install [[https://github.com/golangci/golangci-lint#install][golangci-lint]]

* History and Overview


* History of Go

.image _img/xkcd_compiling.png
.caption [[https://xkcd.com/303/]]

2007: Robert Griesemer, Rob Pike, and Ken Thompson start working on Go at Google
2008: Russ Cox (go mod) and Ian Taylor (gcc frontend) join
2009: Go goes Open Source
2012: Go version 1.0


* What is Go?

- A simple language that is easy to learn and read
- Statically typed, but with a dynamic feel
- Compiled to native machine code, but has a fast development cycle
- Language-level concurrency features
- Comprehensive and clear standard library
- Garbage collected
- Unicode support
- Great tools
- Open source

(Source: [[https://talks.golang.org/2012/zen.slide#2][Go and the Zen of Python]])


* What does Go look like?

Hello world in Go

	package main

	import (
		"fmt"
	)

	func main() {
		fmt.Println("Hello, Melbourne ☕️🎸🏈❤️")
	}

Execute with

	language:bash
	go run hello.go


* What is Go NOT?

Go favours simplicity and directness resulting in some purposeful omissions:

- No type hierarchies or inheritance
- No exceptions
- No method overloading
- No generics
- No decorators
- No named or optional arguments
- No operator overloading
- No macros
- No arguments about code style :)


* Why Go

- simple: easy to learn and read
- a single obvious right way to do most things
- tools - `gofmt`, `goimports`, `godoc`, `go` `mod`, `go` `test`, `golangci-lint`, coverage
- standard library
- productive and fun
- lightweight concurrency

Here is a great read on Rob Pike's blog about "Why Go" - ([[https://commandcenter.blogspot.com/2012/06/less-is-exponentially-more.html][Less is Exponentially More]])


* Demo


* Demo

- 2 seconds latency Pingserver
- Java Spring Boot implementation
- Go implementation
- Benchmark


* Java Spring Boot Initializr

	── build.gradle
	├── gradle
	│   └── wrapper
	│       ├── gradle-wrapper.jar
	│       └── gradle-wrapper.properties
	├── gradlew
	├── gradlew.bat
	├── settings.gradle
	└── src
	    ├── main
	    │   ├── java
	    │   │   └── com
	    │   │       └── anz
	    │   │           └── demo
	    │   │               └── pingserver
	    │   │                   ├── PingserverApplication.java
	    │   │                   └── controller
	    │   │                       └── PingController.java        <--------- THAT'S THE ONE
	    │   └── resources
	    │       └── application.properties
	    └── test
	          .....
		                                └── PingserverApplicationTests.java


* PingController.java

Source on [[https://github.com/anz-bank/go-slides/tree/master/pingserver/java][GitHub]]

	package com.anz.dcx.serverdemo.controller;

	import org.springframework.http.MediaType;
	import org.springframework.web.bind.annotation.GetMapping;
	import org.springframework.web.bind.annotation.ResponseBody;
	import org.springframework.web.bind.annotation.RestController;

	@RestController
	public class PingController {

	    @GetMapping(path = "/ping", produces = MediaType.TEXT_PLAIN_VALUE)
	    @ResponseBody
	    String ping() throws Exception {
	        Thread.sleep(2000L);
	        return "pong";
	    }
	}

Execute with

	./gradlew clean bootRun


* Go Implementation
Source on [[https://github.com/anz-bank/go-slides/tree/master/pingserver/go][GitHub]]

	language:none
	├── go.mod
	├── go.sum
	└── main.go

Execute with

	language:bash
	go run main.go


* Main.go

	package main

	import (
		"fmt"
		"log"
		"net/http"
		"time"
	)

	func main() {
		http.HandleFunc("/ping", func(w http.ResponseWriter, r *http.Request) {
			time.Sleep(2000 * time.Millisecond)
			fmt.Fprint(w, "pong")
		})
		addr := ":9090"
		fmt.Println("Starting webserver on port", addr)
		log.Fatal(http.ListenAndServe(addr, nil))
	}


* Benchmark

- 19,000 concurrent requests
- Ubuntu 18.04 2.3GHz quad-core Intel Core i7
- Apache's `ab` command

    ab -n 19000 -c 19000 -s 200 -r localhost:8080/ping  # Java
    ab -n 19000 -c 19000 -s 200 -r localhost:9090/ping  # go

- Results

	Java median: 102.005 sec
	Java 95%:    135.280 sec

	Go median:   14.954 sec
	Go 95%:      15.061 sec

- Further details on [[https://github.com/anz-bank/go-slides/tree/master/pingserver#benchmark][GitHub]]

* Benchmark
- Range of concurrent requests from 1,000 to 19,000
.image _img/JavaVsGoLoadTest.png 500 _




* Go language fundamentals


* Tutorials, references and tools

- [[https://tour.golang.org/][Tour of Go]]
- [[https://gobyexample.com/][Go by example]]
- [[https://golangbot.com/learn-golang-series/][Golangbot]]
- [[https://play.golang.org/][Go playground]]
- [[https://goplay.space][Go play space]]
- [[https://golang.org/ref/spec][The Go Programming Language Specification]]
- [[https://stackoverflow.com/questions/tagged/go?sort=votes&pageSize=100][Stackoverflow]]


* Getting started

- [[https://goplay.space/#cz9VBPWuHS3][Packages]], imports and exported names
- [[https://goplay.space/#FJKCetXBe2c][Functions]]

_Notes:_
Keep your exported identifiers to as few as possible
Document them


* Basic types

	bool

	string

	int  int8  int16  int32  int64
	uint uint8 uint16 uint32 uint64 uintptr

	byte // alias for uint8

	rune // alias for int32
	     // represents a Unicode code point

	float32 float64

	complex64 complex128


* Zero values

	language:none
	0      for numeric types
	false  for the boolean type
	""     for strings
	nil    for pointers


* Values and control flow

- [[https://goplay.space/#TOqGA-5RjUi][Values]]
- [[https://goplay.space/#ouELtOl07G0][Variables]]
- [[https://goplay.space/#Nc_taqy8IH2][Constants]]
- [[https://goplay.space/#LISrbZdkUsy][Type conversion]]
- [[https://goplay.space/#4IXB0oGgGsY][For]]
- [[https://goplay.space/#AbYAd6LB2c3][If/Else]]
- [[https://goplay.space/#GpoaMY8dboJ][Switch]]

