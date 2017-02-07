+++
weight = 2
title = "Routes"
+++

## Routes

This example will show how to register a route and get the data using just the `net/http` package.

``` go
// routes.go
package main

import (
	"fmt"
	"net/http"
)

func main() {
	userAges := map[string]int{
		"Alice":  25,
		"Bob":    30,
		"Claire": 29,
	}

	http.HandleFunc("/users/", func(w http.ResponseWriter, r *http.Request) {
		name := r.URL.Path[len("/users/"):]
		age := userAges[name]

		fmt.Fprintf(w, "%s is %d years old!", name, age)
	})

	http.ListenAndServe(":8080", nil)
}
```
``` sh
$ go run routes.go

$ curl -s http://localhost:8080/users/Bob
Bob is 30 years old!
```
