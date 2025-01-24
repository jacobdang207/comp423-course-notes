# Setting up a dev container for Go

* Primary author: [Jacob Dang](https://github.com/jacobdang207)
* Reviewer [Christopher McClanahan](https://github.com/chrmccl)

``` bash
mkdir go-container
cd go-container
```

``` bash
git init
```

``` bash
echo "# Go Dev Container" > README.md
git add README.md
git commit -m "Initial commit with README"
```

``` bash
git remote add origin https://github.com/<your-username>/go-container.git
```

``` bash
git push --set-upstream origin main
```

``` json title="devcontainer.json"
{
  "name": "COMP423 Course Notes",
  "image": "mcr.microsoft.com/vscode/devcontainers/go:latest",
  "customizations": {
    "vscode": {
      "settings": {},
      "extensions": ["golang.Go"]
    }
  }
}
```

``` bash
go mod init github.com/<your-username>/go-container
```

``` go title="hello.go"
package main

import "fmt"

func main() {
	fmt.Println("Hello COMP423")
}
```

``` bash
go run hello.go
```
``` bash
go build hello.go
./hello
```

``` bash
git add .
git commit -m "Set up dev container and created hello world program"
git push origin main
```