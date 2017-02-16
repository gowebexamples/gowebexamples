+++
weight = 1
title = "Hello World"
description = "This example will show how to start a webserver on port 8080 and print the classic \"hello world\" message in the Go programming language."
+++

# [Go Web Examples:](/) Hello World

This example will show how to start a webserver on port 8080 and print the classic "hello world" message.

{{< highlight go >}}
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
{{< / highlight >}}
{{< highlight console >}}
$ go run hello-world.go

$ curl -s http://localhost:8080/
hello world
{{< / highlight >}}
