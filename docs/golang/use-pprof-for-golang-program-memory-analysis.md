# Use pprof for golang program memory analysis

When using golang to write complex projects, it is often useful to use multi-coroutine concurrency scenarios. At this
time, it is easy to cause the problem of coroutine leaks due to negligence, and then produce similar memory leaks. This
article focuses on the investigation of coroutine leaks, and provides ideas and practices for visual analysis of golang
program memory.

## Introduction to pprof

pprof is a tool for visualization and analysis of profiling data.  
pprof reads a collection of profiling samples in profile.proto format and generates reports to visualize and help
analyze the data. It can generate both text and graphical reports (through the use of the dot visualization package).

## How to use pprof

### Add monitoring code

First, we need to add monitoring code in the golang program, and expose it through the http interface.

```golang
package main

import _ "net/http/pprof"
import "net/http"

func main() {
	go func() {
		_ = http.ListenAndServe("0.0.0.0:8081", nil)
	}()
	// your code
}
```

Then we start the program that needs to be analyzed, and we are ready to analyze it.

### How to check the memory size of each module

By analyzing the size of the memory occupied by each module and function, memory leaks can be found very effectively.

#### Command line method to generate visual analysis images

```powershell
go tool pprof -alloc_space -cum http://localhost:8081/debug/pprof/heap
```

After the command is run, enter `web` in the console and press `Enter`, and a svg picture will be opened with the
default viewing software of `.svg`, showing the memory usage diagram of each module. If you enter `web` and report an
error of `Failed to execute dot. Is Graphviz installed? Error: exec: "dot": executable file not found in %PATH%`, it is
because `Graphviz` is not installed on the computer, which is a component that image generation depends on. The solution
is: Open https://graphviz.gitlab.io/download/ and follow the prompts to download and install. After the installation is
complete, for Windows, add the `bin` folder of the Graphviz installation path after setting the environment variable
path.

#### View specific data list in web browser

```
http://localhost:8081/debug/pprof/heap?debug=1
```

### How to view the number of coroutines created by each module

By analyzing the number of coroutines created by each module and function, coroutine leaks can be checked very
effectively. If there is coroutine leaks, the number of coroutines in the corresponding modules is astonishing.

#### Command line method to generate visual analysis images

```powershell
go tool pprof http://localhost:8081/debug/pprof/goroutine
```

After the command runs, enter `web` in the console and press `Enter`.

#### View specific data list in web browser

```
http://localhost:8081/debug/pprof/goroutine?debug=1
```

## Summary

The above is an introduction to the simple use of pprof, I believe it will be helpful to troubleshoot memory leaks and
coroutine leaks in golang. If you need more detailed usage, please refer to the official pprof documentation.