+++
weight = 4
title = "JSON"
+++

## JSON

This example will show how to deal with JSON data.
``` go
// json.go
package main

import (
	"encoding/json"
	"fmt"
	"net/http"
)

type User struct {
	Firstname string `json:"firstname"`
	Lastname  string `json:"lastname"`
	Age       int    `json:"age"`
}

func main() {
	http.HandleFunc("/decode", func(w http.ResponseWriter, r *http.Request) {
		var user User

		json.NewDecoder(r.Body).Decode(&user)

		fmt.Fprintf(w, "%s %s is %d years old!",
			user.Firstname, user.Lastname, user.Age)
	})

	http.HandleFunc("/encode", func(w http.ResponseWriter, r *http.Request) {
		peter := User{
			Firstname: "John",
			Lastname:  "Doe",
			Age:       25,
		}

		json.NewEncoder(w).Encode(peter)
	})

	http.ListenAndServe(":8080", nil)
}
```
``` sh
$ go run json.go

$ curl -s -XPOST -d'{"firstname":"Donald","lastname":"Trump","age":70}' http://localhost:8080/decode
Donald Trump is 70 years old!

$ curl -s http://localhost:8080/encode
{"firstname":"John","lastname":"Doe","age":25}

```