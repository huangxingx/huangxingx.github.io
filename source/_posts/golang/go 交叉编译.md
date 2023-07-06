## 交叉编译

#### Mac 下编译， Linux 或者 Windows 下去执行

```shell
# linux 下去执行
CGO_ENABLED=0  GOOS=linux  GOARCH=amd64  go build main.go
# Windows 下去执行
CGO_ENABLED=0 GOOS=windows  GOARCH=amd64  go  build  main.go
```

#### Linux 下编译 ， Mac 或者 Windows 下去执行

```shell
# Mac  下去执行
CGO_ENABLED=0 GOOS=darwin  GOARCH=amd64  go build main.go
# Windows 下执行
CGO_ENABLED=0 GOOS=windows  GOARCH=amd64  go build main.go
```

### Windows 下执行 ， Mac 或 Linux 下去执行

```shell
# Mac 下执行
SET  CGO_ENABLED=0
SET GOOS=darwin
SET GOARCH=amd64
go build main.go
```

```shell
# Linux 去执行
SET CGO_ENABLED=0
SET GOOS=linux
SET GOARCH=amd64
go build main.go
```

#### 参数说明

**CGO_ENABLED** : CGO 表示golang中的工具，CGO_ENABLED 表示CGO禁用，交叉编译中不能使用CGO的

**GOOS** : 目标平台

- mac 对应  **darwin**
- linux 对应 **linux**
- windows 对应 **windows**

**GOARCH** ：目标平台的体系架构【386，amd64,arm】, 目前市面上的个人电脑一般都是amd64架构的

- 386 也称 x86 对应  32位操作系统
- amd64 也称 x64 对应 64位操作系统
- arm 这种架构一般用于嵌入式开发。 比如 Android ， IOS ， Win mobile , TIZEN 等

#### inux 或者 Mac下 go build 前面的参数为何需要那样设置？

**go env** 可以列出我们的golang 默认环境变量，在shell中当我们只想一次性更改其环境变量时，就可以通过在shell中设置变量的方式来更改这个环境变量。