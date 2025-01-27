# Setting up a dev container for Go

* Primary author: [Jacob Dang](https://github.com/jacobdang207)
* Reviewer: [Christopher McClanahan](https://github.com/chrmccl)

In this tutorial, you will learn how to set up a Go dev container in Visual Studio Code (VS Code), how to create a simple Hello World program in Go, and how to put your Go project on GitHub.

This tutorial is adapted from [this](https://comp423-25s.github.io/resources/MkDocs/tutorial/) MkDocs tutorial made by Kris Jordan, and from [this](https://go.dev/doc/tutorial/getting-started) page on the Go documentation site.

## Prerequisites
Make sure you have the following set up before starting this tutorial:

1. **A GitHub Account:** You may sign up for GitHub on their [website](https://github.com/).
2. **Git installed:** You can install git as shown [here](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
3. **VS Code:** Can be downloaded and installed from [here](https://code.visualstudio.com/).
4. **Docker:** Any version of the Docker Engine should work, but in this tutorial we will be using [Docker Desktop](https://www.docker.com/products/docker-desktop).
5. **Command-line basics:** Some knowledge of how to use a command-line interface will be important, as we will be running various commands through our terminal.

## Part 1. Creating the Repository

Before making any other files, we will first create a git repository and make it available on GitHub.

### Step 1. Create the Git Repository on your Local Machine
Open a terminal window or command prompt and create a new directory by entering the following commands:
``` bash
mkdir go-container
cd go-container
```
For this tutorial, we will be using the name `go-container` as the name for our repository, but you may use any name you would like as long as you keep the change consistent.

Now we will initialize an empty git repository within the directory:

``` bash
git init
```
And create a README file for our repo: 

``` bash
echo "# Go Dev Container" > README.md
git add README.md
git commit -m "Initial commit with README"
```
If the repository was set up correctly, the commands should go through without error.

### Step 2. Creating a Remote Repository on GitHub
To make our local repository visible on GitHub, we first have to create a new repository on the site.

In your GitHub account, navigate to the [Create a New Repository](https://github.com/new) page and name your repository `go-container`. In this tutorial, we will not be initializing the repository with a README, .gitignore, or a license. You may set the description and visibility of the repository how you like, and then click **Create Repository.**

### Step 3. Linking your Local Repository to GitHub
To add our local files to the remote repository on GitHub, we will first add the GitHub repository as a new remote:
``` bash
git remote add origin https://github.com/<your-username>/go-container.git
```
You should replace `<your-username>` with your GitHub username.

Now, we will push the commits we made with the following command:
``` bash
git push --set-upstream origin main
```
If you refresh your GitHub repository's page in the browser, you should be able to see your committed files.

## Part 2. Setting Up the Development Environment
### Step 1. Add Development Container Configuration
In VS Code, open up your `go-container` folder. Then, install the Dev Containers extension for VS Code by searching for it in the extensions tab or clicking [here](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers). This extension allows you to run dev containers in VS Code.

Next we will create a `.devcontainer` directory in the root of our project, and add a `devcontainer.json` file within the directory.

The `devcontainer.json` file defines the configuration for our development environment. Enter the contents of the `devcontainer.json` as follows.

``` json title="devcontainer.json"
{
  "name": "Go Container",
  "image": "mcr.microsoft.com/vscode/devcontainers/go:latest",
  "customizations": {
    "vscode": {
      "settings": {},
      "extensions": ["golang.Go"]
    }
  }
}
```
Each property in the `devcontainer.json` file allows us to define something different:

* **`name:`** A descriptive name for your dev container.
* **`image:`** The Docker image used by the container, which in this case is the latest version of a Go environment.
* **`customizations:`** Can be used to add VS Code extensions or set particular settings. In this case we added the Go plugin for VS Code.

There are many more propeties that can be set. A list of them can be found [here](https://containers.dev/implementors/json_reference/).

### Step 2. Reopen the Project in a VS Code Dev Container
You can now open the project in a dev container. First, make sure Docker Desktop is running. Then, in the VS Code window with your project press `Ctrl + Shift + P`, type in "Dev Containers: Reopen in Container" to the prompt, and then select the option that pops up. It may take a bit for the image to download.

Once the dev container setup completes, you can open a new terminal window within VS Code and run `go version` to see what version of Go is running in the container. It should be the latest version (1.23 as of writing this in January of 2025).

## Part 3. Creating a Hello World program with Go
### Step 1. Enabling dependency tracking for Go
Before writing any code, you should initialize a `go.mod` file in your project's root directory. The `go.mod` file defines the Go module your code will be in, and tracks all the other modules your program uses. To make the file you can use the following command:
``` bash
go mod init github.com/<your-username>/go-container
```
Be sure to replace `<your-username>` with your GitHub username. This command initializes the `go.mod` file, and names the module `github.com/<your-username>/go-container.` It is best practice to name your module after where it's source code is located, which in our case is our GitHub repository. However, the module's name is mostly important for publishing the module for others to use, and it won't come into play very much in this tutorial.

### Step 2. Write the Hello World program
Create a new file called `hello.go` in the root directory containing the following Go code:
``` go title="hello.go"
package main

import "fmt"

func main() {
	fmt.Println("Hello COMP423")
}
```
This program does a couple of things:

1. Packages the file into the `main` package. The `main` package is the package that Go programs are executed through. The `main()` function executes when the `main` package is run.
2. Imports the `fmt` package, which contains functions for formatting text and printing.
3. Calls the `main()` function and prints out a message to the console.

### Step 3. Compile and Run the program
You can compile and run your code like so to verify if it works:

``` bash
go run hello.go
```
The program should have printed out `Hello COMP423`.

You can also build an executable file from `hello.go` and run the binary directly like so:
``` bash
go build hello.go
./hello
```
`go build` can be thought of as Go's version of the gcc command for C files, as both commands compile source code into a binary file which can be executed by your computer. 

Both `go run` and `go build` compile files to be executed, however `go run` runs the binary immediately and does not leave it in your project directory, while `go build` creates a binary in your folder that you must run manually. 

## Part 4. Commit your changes
In order to commit your changes, run the following commands:
``` bash
git add .
git commit -m "Set up dev container and create hello world program"
```
Running these commands will first stage your changes, and then create a new commit out of them.

!!! Tip

    It is best practice to avoid committing compilation outputs to your git repository. In order to avoid staging compilation outputs while preserving the ease of `git add .`, you can create a `.gitignore` file containing the name of all files you want git to never stage.

## Part 5. Push to Github
In order to push the new commit to your GitHub repository, run the following command:
``` bash
git push origin main
```
This command will push the commit you created in Part 4 to the remote GitHub repository.

## Conclusion
This is the end of the tutorial! You have learned how to create a Go dev container in VS Code, make a short program in Go, and put your project on GitHub.