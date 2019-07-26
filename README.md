# about gotask 
[![Build Status](https://travis-ci.org/scottkiss/grtm.svg?branch=master)](https://travis-ci.org/scottkiss/grtm)

gotask is a tool to manage golang goroutines.use this can start or stop a long loop goroutine.

## Getting started
```bash
go get bigdot123456/gotask
```

## create repository
```bash
echo "# gotask" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/bigdot123456/gotask.git
git push -u origin master
```

## Create normal goroutine

```golang
package main

import (
        "fmt"
        "github.com/bigdot123456/gotask"
        "time"
       )

func normal() {
    fmt.Println("i am normal goroutine")
}

func main() {
        gm := gotask.NewGrManager()
        gm.NewGoroutine("normal", normal)
        fmt.Println("main function")
        time.Sleep(time.Second * time.Duration(5))
}
~
```

## Create normal goroutine function with params

```golang
package main

import (
        "fmt"
        "github.com/bigdot123456/gotask"
        "time"
       )

func normal() {
    fmt.Println("i am normal goroutine")
}

func funcWithParams(args ...interface{}) {
    fmt.Println(args[0].([]interface{})[0].(string))
    fmt.Println(args[0].([]interface{})[1].(string))
}

func main() {
        gm := gotask.NewGrManager()
        gm.NewGoroutine("normal", normal)
        fmt.Println("main function")
        gm.NewGoroutine("funcWithParams", funcWithParams, "hello", "world")
        time.Sleep(time.Second * time.Duration(5))
}
```

## Create long loop goroutine then stop it

```golang
package main

import (
        "fmt"
        "github.com/bigdot123456/gotask"
        "time"
       )

func myfunc() {
    fmt.Println("do something repeat by interval 4 seconds")
        time.Sleep(time.Second * time.Duration(4))
}

func main() {
gm := gotask.NewGrManager()
        gm.NewLoopGoroutine("myfunc", myfunc)
        fmt.Println("main function")
        time.Sleep(time.Second * time.Duration(40))
        fmt.Println("stop myfunc goroutine")
        gm.StopLoopGoroutine("myfunc")
        time.Sleep(time.Second * time.Duration(80))
}
```

output

```bash
main function
no signal
do something repeat by interval 4 seconds
no signal
do something repeat by interval 4 seconds
no signal
do something repeat by interval 4 seconds
no signal
do something repeat by interval 4 seconds
no signal
do something repeat by interval 4 seconds
no signal
do something repeat by interval 4 seconds
no signal
do something repeat by interval 4 seconds
no signal
do something repeat by interval 4 seconds
no signal
do something repeat by interval 4 seconds
no signal
do something repeat by interval 4 seconds
stop myfunc goroutine
gid[5577006791947779410] quit

```


# gotask
