+++
weight = 1
title = "Hello World"
+++

## Hello World

This example will show how to start a webserver on port 8080 and print the classic "hello world" message.

``` go
// hello-world.go
package main

import (
	"fmt"
	"net/http"
)

func main() {
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		fmt.Fprintln(w, "hello world")
	})

	http.ListenAndServe(":8080", nil)
}
```
``` sh
$ go run hello-world.go

$ curl -s http://localhost:8080/
hello world
```
