# Calling a ScienceOps model from Go

```
package main

import (
	"io"
	"log"
	"net/http"
	"os"
	"strings"
)

func main() {
	username := "kermit"
	modelname := "mymodel"
	apikey := "foobar"

	url := "https://scienceops.yhathq.com/" + username + "/models/" + modelname + "/"

	input := `{"input":"here"}`

	req, err := http.NewRequest("POST", url, strings.NewReader(input))
	if err != nil {
		log.Fatal(err)
	}
	req.SetBasicAuth(username, apikey)
	req.Header.Set("Content-Type", "application/json")

	resp, err := http.DefaultClient.Do(req)
	if err != nil {
		log.Fatal(err)
	}
	defer resp.Body.Close()
	io.Copy(os.Stdout, resp.Body)
}
```
