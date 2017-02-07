+++
weight = 5
title = "Forms"
+++

## Forms

This example will simulate a contact form and parse the message into a struct.

``` go
// forms.go
package main

import (
	"html/template"
	"net/http"
)

type ContactDetails struct {
	Email   string
	Subject string
	Message string
}

func main() {
	tmpl := template.Must(template.ParseFiles("forms.html"))

	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		if r.Method != http.MethodPost {
			tmpl.Execute(w, nil)
			return
		}

		details := ContactDetails{
			Email:   r.FormValue("email"),
			Subject: r.FormValue("subject"),
			Message: r.FormValue("message"),
		}

		// do something with details
		_ = details

		tmpl.Execute(w, struct{ Success bool }{true})
	})

	http.ListenAndServe(":8080", nil)
}


```
``` html
<!-- forms.html -->
{{if .Success}}
	<h1>Thanks for your message!</h1>
{{else}}
	<h1>Contact</h1>
	<form method="POST">
		<label>Email:</label><br />
		<input type="text" name="email"><br />
		<label>Subject:</label><br />
		<input type="text" name="subject"><br />
		<label>Message:</label><br />
		<textarea name="message"></textarea><br />
		<input type="submit">
	</form>
{{end}}

```
``` sh
$ go run forms.go
```
<div class="demo">
	<h1>Contact</h1>
	<form method="POST">
		<label>Email:</label><br />
		<input type="text" name="email"><br />
		<label>Subject:</label><br />
		<input type="text" name="subject"><br />
		<label>Message:</label><br />
		<textarea name="message"></textarea><br />
		<input type="submit">
	</form>
</div>
