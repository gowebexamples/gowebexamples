+++
weight = 4
title = "Templates"
description = "This example will show how to render a simple list of TODO items into an html page using the html/template package in the Go programming language."
+++

## Templates

This example will show how to render a simple list of TODO items into an html page using the `html/template` package.

``` go
// todos.go
package main

import (
	"html/template"
	"net/http"
)

type Todo struct {
	Task string
	Done bool
}

func main() {
	tmpl := template.Must(template.ParseFiles("todos.html"))
	todos := []Todo{
		{"Learn Go", true},
		{"Read Go Web Examples", true},
		{"Create a web app in Go", false},
	}

	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		tmpl.Execute(w, struct{ Todos []Todo }{todos})
	})

	http.ListenAndServe(":8080", nil)
}
```
``` html
<!-- todos.html -->
<h1>Todos</h1>
<ul>
	{{range .Todos}}
		{{if .Done}}
			<li><s>{{.Task}}</s></li>
		{{else}}
			<li>{{.Task}}</li>
		{{end}}
	{{end}}
</ul>
```
``` sh
$ go run todos.go
```
<div class="demo">
	<h1>Todos</h1>
	<ul>
		<li><s>Learn Go</s></li>
		<li><s>Read Go Web Examples</s></li>
		<li>Create a web app in Go</li>
	</ul>
</div>
