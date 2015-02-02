# gocumber

Go code (golang) package that provides tools for defining and executing gherkin steps.

This package uses the [go-gherkin] package by [@muhqu]. Thanks buddy!

## Installation

To install:

```go
go get github.com/sittercity/gocumber
```

Then import it like normal:

```go
package <yourpackage>

import (
  "github.com/sittercity/gocumber"
)
```

# Usage

General example:

```go
func TestYourStuff(t *testing.T) {
  steps := make(gocumber.Definitions)

  steps.Given("I have an existing user", func(matches []string, _ gocumber.StepNode) {
    // your code to create a user, you can use 'matches[1]' to pull out the
    // name, like so: user_name := matches[1]
  })
}
```

Also allows for the execution of other defined steps:

```go
func TestYourStuff(t *testing.T) {
  steps := make(gocumber.Definitions)

  steps.Given(`I have an existing user with name "(.*)"`, func(matches []string, _ gocumber.StepNode) {
    // User creation
  })

  steps.Given("I have an existing user", func([]string, gocumber.StepNode) {
    steps.Exec(`I have an existing user with name "some name"`)
  })
```

You can use `When` and `Then` like you would expect:

```go
  steps.When("I view all users", func([]string, gocumber.StepNode) {
    // Do some view stuff
  })

  steps.Then("the user should be created", func([]string, gocumber.StepNode) {
    // check that the user is created
  })
```

If you have gherkin that includes a JSON payload you can retrieve it like so:

```go
  steps.Given("I create a user with the following data:", func(_ []string, step gocumber.StepNode) {
    payload := step.PyString().String()
    // Do stuff with your payload
  })
```

Once all of your steps are defined you run them at the end of your file:

```go
steps.Run(t, "your_gherkin.feature")
```

A full example file with all of the above (with gherkin) is forthcoming.

# TODO

 - Need tests
 - Should create a full example for clarity (with gherkin)
 - Does not append gherkin background
