+++
weight = 9
title = "Websockets"
description = "This example will show how to work with websockets in Go. We will build a simple server which echoes back everything we send to it."
+++

# [Go Web Examples:](/) Websockets

This example will show how to work with websockets in Go. We will build a simple server which echoes back everything we send to it.
For this we have to `go get` the popular <a target="_blank" href="https://github.com/gorilla/websocket">gorilla/websocket</a> library like so:

`$ go get github.com/gorilla/websocket`

From now on, every application we write will be able to make use of this library.
``` go
// websockets.go
package main

import (
	"fmt"
	"net/http"

	"github.com/gorilla/websocket"
)

var upgrader = websocket.Upgrader{
	ReadBufferSize:  1024,
	WriteBufferSize: 1024,
}

func main() {
	http.HandleFunc("/echo", func(w http.ResponseWriter, r *http.Request) {
		conn, _ := upgrader.Upgrade(w, r, nil) // error ignored for sake of simplicity

		for {
			// Read message from browser
			msgType, msg, err := conn.ReadMessage()
			if err != nil {
				return
			}

			// Print the message to the console
			fmt.Printf("%s sent: %s\n", conn.RemoteAddr(), string(msg))

			// Write message back to browser
			if err = conn.WriteMessage(msgType, msg); err != nil {
				return
			}
		}
	})

	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		http.ServeFile(w, r, "websockets.html")
	})

	http.ListenAndServe(":8080", nil)
}
```
``` html
<!-- websockets.html -->
<input id="input" type="text" />
<button onclick="send()">Send</button>
<pre id="output"></pre>
<script>
	var input = document.getElementById("input");
	var output = document.getElementById("output");
	var socket = new WebSocket("ws://localhost:8080/echo");

	socket.onopen = function () {
		output.innerHTML += "Status: Connected\n";
	};

	socket.onmessage = function (e) {
		output.innerHTML += "Server: " + e.data + "\n";
	};

	function send() {
		socket.send(input.value);
		input.value = "";
	}
</script>
```
``` console
$ go run websockets.go
[127.0.0.1]:53403 sent: Hello Go Web Examples, you're doing great!
```
<div class="demo">
	<input type="text">
	<button>Send</button>
	<pre>Status: Connected
Server: Hello Go Web Examples, you're doing great!</pre>
</div>
