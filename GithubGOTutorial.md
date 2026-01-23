# Step 1: Write the application code

```
package main

import (
	"fmt"
	"net/http"
	"strconv"
)

func Add(a, b int) int {
	return a + b
}

func additionHandler(w http.ResponseWriter, r *http.Request) {
	values := r.URL.Query()

	aStr := values.Get("a")
	bStr := values.Get("b")

	a, err := strconv.Atoi(aStr)
	if err != nil {
		http.Error(w, "Parameter 'a' must be an integer", http.StatusBadRequest)
		return
	}

	b, err := strconv.Atoi(bStr)
	if err != nil {
		http.Error(w, "Parameter 'b' must be an integer", http.StatusBadRequest)
		return
	}

	result := Add(a, b)

	fmt.Fprintf(w, "%d", result)
}

func main() {
	http.HandleFunc("/addition", additionHandler)

	fmt.Println("Server listening on port 8086....")
	http.ListenAndServe(":8086", nil)
}
```

### From the terminal, navigate to the root of this project and run ‘go run .’ to run the application. If it displays the message Server listening on port 8086…., it means it has run successfully. 

# Step 2: Create a YAML file to define the actions
### In the project’s root directory, create a subdirectory, .github/workflows. Within this directory, create a file with the .yml extension. In this example, we have named it go.yml. This .yml file is automatically interpreted by GitHub Actions as a workflow file.

```
name: Go

on:
  push:
    branches: [ "main" ]
	The name parameter just names this workflow as Go. on: push: branches: part of the workflow file mentions the list of branches for which the workflow should be triggered automatically. In this case, we want the workflow to trigger automatically for the ‘main’ branch.

# Step 3: Configure a build job
### Large applications written in the Go programming language require a build step because it is a statically typed, compiled language. This generates a binary, which is then deployed to the servers. The first job, which we define in the workflow, is to build this binary. 
	
### each job runs on a fresh instance of a runner. Think of this as a fresh virtual machine where the required dependencies are not available.
```
	
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3

    - name: Set up Go
      uses: actions/setup-go@v4
      with:
        go-version: '1.21.5'

    - name: Build
      run: go build -v ./...

```

### First, we defined a job named build. The job name is not part of the syntax, and it could be anything that makes sense to you. Next, we need to specify the type of runner we want to use to perform this job. GitHub Actions supports various operating systems like Ubuntu, Microsoft Windows, or MacOS. For our purpose, we will use the Ubuntu environment.

### The first step in this job is to check out the application’s source code on the runner. This clones the files in the GitHub repository on the runner VM. In the next step, we set up the Go compiler with the desired version. Finally, in the third step, we run the go build command. The binary thus generated is made available on the runner locally.

### Thus, in this step, we have just built the binary for the Go application. If you now push any changes to the source code repository on the main branch, this workflow will automatically trigger and follow these steps each time. 

### However, note that because these runners are only made available to perform the jobs, they are revoked when all the jobs are done. This also means that the binary thus generated is also lost after a workflow run.

# Step 4: Test your GitHub Action workflow
### we want to test the application before we deliver the binary. The purpose of this test is to quickly identify any issues 
### The source should always have unit test cases defined, ensuring maximum coverage. This is true for applications written in any programming language. For this example, let us first write the unit test that ensures the logic written to perform simple math operations is correct. Go provides a ‘testing’ package that helps in writing test cases for the application source code. The code below is part of the main_test.go file — the name of the file is driven by the testing framework.
```
package main

import (
	"io/ioutil"
	"net/http/httptest"
	"strconv"
	"testing"
)

func TestAdditionHandler(t *testing.T) {
	// Test case 1
	req1 := httptest.NewRequest("GET", "/addition?a=3&b=5", nil)
	w1 := httptest.NewRecorder()
	additionHandler(w1, req1)
	resp1 := w1.Result()
	defer resp1.Body.Close()
	body1, _ := ioutil.ReadAll(resp1.Body)
	result1, _ := strconv.Atoi(string(body1))
	expected1 := 8
	if result1 != expected1 {
		t.Errorf("Test case 1 failed, expected %d but got %d", expected1, result1)
	}
}
```

### The test case above makes an API call to the /addition API with specific inputs and expects the corresponding output. If the simple math operation logic is flawed, it fails the test case, and the GitHub Action workflow also fails and does not proceed.
### To run this test in the GitHub Actions workflow, go back to the go.yml file and add the following step in the build job defined above.
```

- name: Test
      run: go test -v ./...
```
### Push the code to the main branch and observe the pipeline run. As seen from the screenshot below, the test cases are run during the ‘Test’ step and their results are also printed in the logs.

