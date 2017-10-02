+++
weight = 4
title = "Assets and Files"
description = "This example will show how to serve static files like CSS or JS from a specific directory using the http.FileServer in the Go programming language."
+++

# Assets and Files

This example will show how to serve static files like CSSs, JavaScripts or images from a specific directory.

{{< edison >}}

{{< highlight go >}}
// static-files.go
package main

import "net/http"

func main() {
	fs := http.FileServer(http.Dir("assets/"))
	http.Handle("/static/", http.StripPrefix("/static/", fs))

	http.ListenAndServe(":8080", nil)
}
{{< / highlight >}}
{{< highlight console >}}
$ tree assets/
assets/
└── css
    └── styles.css
{{< / highlight >}}
{{< highlight console >}}
$ go run static-files.go

$ curl -s http://localhost:8080/static/css/styles.css
body {
    background-color: black;
}
{{< / highlight >}}
