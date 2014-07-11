# ScienceOps Go client

Go library for calling a deployed Yhat analytics model. Integrate your Python and R machine learning / statistical models within Go apps.

## Install

You can install this package to your local environment using `go get`. The following command will clone the library to your `$GOPATH`.

```bash
$ go get -d github.com/yhat/yhat-go/
```

## Hello World Example

Run the following code to deploy an [R model](https://docs.yhathq.com/r/examples/hello-world).

```R
library(yhatr)

model.transform <- function(request) {
    me <- request$name
    paste("Hello ", me, "!", sep="")
}
model.predict <- function(greeting) {
    data.frame(greeting=greeting)
}

yhat.config  <- c(
    username="USERNAME",
    apikey="APIKEY",
    env="SCIENCE OPS URL"
)
yhat.deploy("HelloWorld")
```

Then call it from the Go client.

```go
package main

import (
	"github.com/yhat/yhat-go/yhat"
	"fmt"
)

func main() {
	yh, err := yhat.New("USERNAME", "APIKEY", "SCIENCE OPS URL")
	if err != nil {
		fmt.Printf("%s\n", err)
	} else {
		data := map[string]string{"name": "Hank"}
		res, err := yh.Predict("HelloWorld", data)
		if err == nil {
			for k := range res {
				fmt.Printf("%s: %s\n", k, res[k])
			}
		}
	}
}
```

Running this module will produce the following output:

```bash
yhat_id: f933c05c-71b2-424d-b9d0-c85b36b7039f
result: map[greeting:[Hello Hank!]]
yhat_model: HelloWorld
```
