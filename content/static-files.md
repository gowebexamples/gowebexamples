+++
weight = 7
title = "Static Files"
+++

## Static Files

This example will show how to serve static files like CSSs, JavaScripts or images from a specific directory.

``` go
// static-files.go
package main

import "net/http"

func main() {
	fileServer := http.FileServer(http.Dir("assets/"))
	http.Handle("/static/", http.StripPrefix("/static/", fileServer))

	http.ListenAndServe(":8080", nil)
}
```
``` sh
$ tree assets/
assets/
└── css
    └── styles.css
```
``` sh
$ go run static-files.go

$ curl -s http://localhost:8080/static/css/styles.css
body {
    background-color: black;
}
```
