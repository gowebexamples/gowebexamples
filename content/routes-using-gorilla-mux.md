+++
weight = 2
title = "Routes (using gorilla/mux)"
+++

## Routes (using gorilla/mux)

This example will show how to register restful routes using the popular <a target="_blank" href="https://github.com/gorilla/mux">gorilla/mux</a> router.
To use the library we will have to install it first like so:

`$ go get github.com/gorilla/mux`

From now on, every application we write will be able to make use of this library.

``` go
// routes.go
package main

import (
	"fmt"
	"net/http"

	"github.com/gorilla/mux"
)

func main() {
	users := map[string]int{
		"Alice":  25,
		"Bob":    30,
		"Claire": 29,
	}

	r := mux.NewRouter()
	r.HandleFunc("/users/{name}", func(w http.ResponseWriter, r *http.Request) {
		params := mux.Vars(r)
		name := params["name"]
		age := users[name]

		fmt.Fprintf(w, "%s is %d years old!", name, age)
	})

	http.ListenAndServe(":8080", r)
}
```
``` sh
$ go run routes.go

$ curl -s http://localhost:8080/users/Bob
Bob is 30 years old!
```