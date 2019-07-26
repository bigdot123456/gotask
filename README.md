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

## create travis code

Travis CI是国外新兴的开源持续集成构建项目，支持Github项目。使用十分方便。 

使用Github账号登录Travis CI； 

登录之后会自动同步Github项目，选择需要使用Travis CI的项目 

在项目的根目录新增.travis.yml文件，内容如下： 

```bash
#指定运行环境
language: golang
#指定nodejs版本，可以指定多个
golang:
  - 1.11

#运行的脚本命令
script:
  - go test grmanger

#指定分支，只有指定的分支提交时才会运行脚本
branches:
  only:
    - master
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
