
### Getting started

In this repository we will be diving into the world of writing GitHub Actions! I will guide you through the process of writing a custom Docker based GitHub Actions.

You may be asking yourself, "is Docker the only way to create custom GitHub Actions?"

Currently, there are **two** supported ways to create your own GitHub Actions:

- [Docker container actions](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/about-actions#docker-container-actions)
- [JavaScript actions](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/about-actions#javascript-actions)

As you can see we aren't necessarily limited to Docker even though it is the focal point for this course.

---

### Creating vs consuming actions

Although we can focus on creating and consuming a custom action, we will also be consuming some actions that have been made public to us. Because your workflows will most likely do the same.

After all, for each time we need to reinvent the wheel for our specific use-case there are a handful of times when we are better off using a wheel that's already made!

- The [GitHub Actions Marketplace](https://github.com/marketplace?type=actions) is the primary place to find open-source actions that the community has written and released. Your action, should you choose to release it, could also reside here one day, ready to be consumed by the world!
- The [GitHub Actions Repository](https://github.com/actions) is where you can find actions that are written by GitHub. We will leverage an action named `[checkout](https://github.com/actions/checkout)` from this repository as we go through this course. I'll explain more about what it does when we use it!
- Your repositories may also contain **private actions** and they will most likely be located in the `.github/actions` directory in the root of your repository. **This is the convention we will be using as we learn how to create our own action.**

---

### Example

A custom action is called from within the workflow file

```
name: Docker Actions

on:
  pull_request:
    types: [labeled]

jobs:
  action:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v1

      - name: hello-action
        uses: ./.github/actions/hello-world

      - name: meow
        uses: ./.github/actions/cat-facts
```

In each of the directories one can have a seperate action.yml file, a Dockerfile and some code that gets imported into the Docker Conatainer.

#### Dockerfile
```
FROM golang:1.15
WORKDIR /go/src/hello
COPY . .
RUN go get -d -v ./...
RUN go install -v ./...
CMD ["hello"]
```

#### action.yml

Action metadata
You learned that every action is accompanied by an action.yml file which contains a series of metadata for how the action behaves. This file defines all of an actions inputs, outputs, runtime environment, name, description and even branding.

```
name: "my hello action"

description: "say hello with GitHub Actions"

inputs:
  firstGreeting:
    description: "who would you like to greet in the console"
    required: true
    default: "Hubot"

  secondGreeting:
    description: "another person to greet"
    required: true
    default: "Mona the Octocat"

  thirdGreeting:
    description: "a third greeting"
    required: false

runs:
  using: "docker"
  image: "Dockerfile
```

#### main.go
```
package main

import (
    "fmt"
    "os"
)

func main() {

// Access Inputs as environment vars
firstGreeting := os.Getenv("INPUT_FIRSTGREETING")
secondGreeting := os.Getenv("INPUT_SECONDGREETING")
thirdGreeting := os.Getenv("INPUT_THIRDGREETING")

// Use those inputs in the actions logic
fmt.Println("Hello " + firstGreeting)
fmt.Println("Hello " + secondGreeting)

// Someimes inputs are not "required" and we can build around that
if thirdGreeting != "" {
    fmt.Println("Hello " + thirdGreeting)
    }

}
```

