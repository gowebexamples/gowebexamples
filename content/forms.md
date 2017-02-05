+++
weight = 4
title = "Forms"
+++

## Forms

This example will simulate a login form, parse the data submitted and check for valid credentials.

``` go
// forms.go
package main

import (
	"html/template"
	"net/http"
)

type Credentials struct {
	Email    string
	Password string
	LoggedIn bool
}

func main() {
	tmpl := template.Must(template.ParseFiles("forms.html"))

	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		if r.Method != http.MethodPost {
			tmpl.Execute(w, nil)
			return
		}

		creds := Credentials{
			Email:    r.FormValue("email"),
			Password: r.FormValue("password"),
		}

		if creds.Email == "user@example.com" && creds.Password == "secret" {
			creds.LoggedIn = true
		}

		tmpl.Execute(w, creds)
	})

	http.ListenAndServe(":8080", nil)
}

```
``` html
<!-- forms.html -->
{{if .LoggedIn}}
    <h1>Hello {{.Email}}, you're now logged in!</h1>
{{else}}
    <h1>Please enter your login credentials</h1>
    <form method="POST">
        <label>Email:</label><br />
        <input type="text" name="email"><br />
        <label>Password:</label><br />
        <input type="password" name="password"><br />
        <input type="submit">
    </form>
{{end}}
```
``` sh
$ go run forms.go
```
<div class="image">
	<img src="/forms.png" alt="Forms" />
</div>
