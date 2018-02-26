## 版本要求

go1.10+

## 安装

```sh
$ go get -u golang.org/x/vgo
```

## 常用命令

```sh
$ vgo list -m  # 查看所有依赖
$ vgo list -m -u  # 查看所有依赖同时检查更新，会打印出最新版本和当前版本
$ vgo test all  # 执行所有测试，包括依赖包的测试
$ vgo test http://rsc.io/sampler  # 执行指定包测试
$ vgo get -u  # 更新所有依赖
$ vgo list -t http://rsc.io/sampler  # 检查指定包所有可用的版本 即tag
$ vgo get http://rsc.io/sampler@v1.3.1  # 获取指定版本，并修改go.mod
$ vgo vendor  # 退到vendor 兼容不使用vgo的用户
...
```

## 使用

根目录创建 `go.mod` 标示项目根目录, 终结了 `GOPATH` 作为 Go 代码工作空间的设置, `go.mod` 文件包含了完整的模块路径并且定义了每个使用的依赖的版本, 因此包含 `go.mod` 文件的目录就可以被认为是一个目录树的根目录了, 该目录树作用于自身的工作空间，并且和其他类似的目录彼此隔离

```sh
$ touch go.mod
```


```
# 用 `// import` 注释 vgo 模块的 "导入路径名称"
package main // import "github.com/you/hello"
```

执行 `vgo build` 后自动生成依赖关系到 `go.mod`

```
module "github.com/leecade/vgo_example"

require "rsc.io/quote" v1.5.2
```

`go.mod` 文件包含了模块所依赖包的最小版本. 如果模块没有提供一个tag版本, 对于未命名的提交, `v0.0.0-yyyymmddhhmmss-commit` 表示一个指定日期的提交.

`go.mod` 文件还可以实现排除和替换的版本, 需手动修改go.mod文件:

```
exclude "rsc.io/sampler" v1.99.99

replace "rsc.io/quote" v1.5.2 => "../quote"
```

## 问题

- github api 超限

    ```
    import "rsc.io/quote": unexpected status (https://api.github.com/repos/rsc/quote): 401 Unauthorized
    ```

    在 github "Settings > Developer settings > Personal access tokens > Generate new token" 创建新 token, 更新到 `~/.netrc`

    ```
    machine api.github.com
        login ACCOUNT
        password TOKEN
    ```
