+++
weight = 3
title = "Templates"
+++

## Templates

This example will render a simple list of TODO items into an html page.

``` go
// todos.go
package main

import (
	"html/template"
	"net/http"
)

func main() {
	tmpl := template.Must(template.ParseFiles("todos.html"))
	todos := []struct {
		Task string
		Done bool
	}{
		{"Learn Go", true},
		{"Read Go Web Examples", true},
		{"Create a web app in Go", false},
	}

	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		tmpl.Execute(w, todos)
	})

	http.ListenAndServe(":8080", nil)
}
```
``` html
<!-- todos.html -->
<h1>Todos</h1>
<ul>
	{{range .}}
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
<div class="image">
	<img src="/templates.png" alt="Forms" />
</div>
