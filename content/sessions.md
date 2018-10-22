+++
weight = 7
title = "Sessions"
description = "This example will show how to store data in session cookies using the popular gorilla/sessions package in the Go programming language."
+++

# Sessions

## Introduction

This example will show how to store data in session cookies using the popular <a target="_blank" href="https://github.com/gorilla/sessions">gorilla/sessions</a> package in Go.

Cookies are small pieces of data stored in the browser of a user and are sent to our server on each request. In them, we can store e.g. whether or not a user is logged in into our website and figure out who he actually is (in our system).

In this example we will only allow authenticated users to view our secret message on the `/secret` page. To get access to it, the will first have to visit `/login` to get a valid session cookie, which logs him in. Additionally he can visit `/logout` to revoke his access to our secret message.

## Installing the `gorilla/sessions` package

Use the `go get` command to install the package from GitHub like so:
{{< highlight sh >}}
go get -u github.com/gorilla/sessions
{{< / highlight >}}

## Creating a new Session

{{< edison >}}

{{< highlight go >}}
// sessions.go
package main

import (
	"fmt"
	"net/http"

	"github.com/gorilla/sessions"
)

var (
	// key must be 16, 24 or 32 bytes long (AES-128, AES-192 or AES-256)
	key = []byte("super-secret-key")
	store = sessions.NewCookieStore(key)
)

func secret(w http.ResponseWriter, r *http.Request) {
	session, _ := store.Get(r, "cookie-name")

	// Check if user is authenticated
	if auth, ok := session.Values["authenticated"].(bool); !ok || !auth {
		http.Error(w, "Forbidden", http.StatusForbidden)
		return
	}

	// Print secret message
	fmt.Fprintln(w, "The cake is a lie!")
}

func login(w http.ResponseWriter, r *http.Request) {
	session, _ := store.Get(r, "cookie-name")

	// Authentication goes here
	// ...

	// Set user as authenticated
	session.Values["authenticated"] = true
	session.Save(r, w)
}

func logout(w http.ResponseWriter, r *http.Request) {
	session, _ := store.Get(r, "cookie-name")

	// Revoke users authentication
	session.Values["authenticated"] = false
	session.Save(r, w)
}

func main() {
	http.HandleFunc("/secret", secret)
	http.HandleFunc("/login", login)
	http.HandleFunc("/logout", logout)

	http.ListenAndServe(":8080", nil)
}
{{< / highlight >}}
{{< highlight console >}}
$ go run sessions.go

$ curl -s http://localhost:8080/secret
Forbidden

$ curl -s -I http://localhost:8080/login
Set-Cookie: cookie-name=MTQ4NzE5Mz...

$ curl -s --cookie "cookie-name=MTQ4NzE5Mz..." http://localhost:8080/secret
The cake is a lie!
{{< / highlight >}}


