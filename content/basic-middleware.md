+++
weight = 9
title = "Basic Middleware"
+++

## Basic Middleware

This example will show how to create basic logging middleware in Go.

A middleware simply takes a `http.HandlerFunc` as one of its parameters, wraps it and returns a new `http.HandlerFunc` for the server to call.

``` go
// basic-middleware.go
package main

import (
	"fmt"
	"log"
	"net/http"
	"time"
)

func logging(f http.HandlerFunc) http.HandlerFunc {
	return func(w http.ResponseWriter, r *http.Request) {
		start := time.Now()
		defer func() { log.Println(r.URL.Path, time.Since(start)) }()

		f(w, r)
	}

}

func foo(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintln(w, "foo")
}

func bar(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintln(w, "bar")
}

func main() {
	http.HandleFunc("/foo", logging(foo))
	http.HandleFunc("/bar", logging(bar))

	http.ListenAndServe(":8080", nil)
}

```
```
$ go run basic-middleware.go
2017/02/10 23:59:34 /foo 0s
2017/02/10 23:59:35 /bar 0s
2017/02/10 23:59:36 /foo?bar 0s

$ curl -s http://localhost:8080/foo
$ curl -s http://localhost:8080/bar
$ curl -s http://localhost:8080/foo?bar
```
