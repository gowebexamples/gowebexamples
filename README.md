## Go Web Examples

Go Web Examples provides easy to understand snippets on how to do web development in Go and is insired by [Go By Example](https://gobyexample.com/), which has a great introduction into the fundametals of this language.

The site is built using the [Hugo](https://github.com/spf13/hugo) static site generator.
The source codes to the examples can be found in the `content` directory.
When you build the site using the `hugo` command, the output files will be generated into the `public` directory.


### Building

To build this site Hugo and Python (with `pygments` installed) will be required.

```console
$ go get github.com/gowebexamples/gowebexamples
$ cd $GOPATH/src/github.com/gowebexamples/gowebexamples
$ hugo
```

### Thanks

Thanks to [Go By Example](https://gobyexample.com/) for the inspiration to this project.

### License

MIT
