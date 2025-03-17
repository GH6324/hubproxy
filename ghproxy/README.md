## GHProxy

### 项目地址：https://github.com/WJQSERVER-STUDIO/ghproxy


### 外部配置文件

本项目采用`config.toml`作为外部配置,默认配置如下
使用Docker部署时,慎重修改`config.toml`,以免造成不必要的麻烦

```toml
[server]
host = "0.0.0.0"  # 监听地址
port = 8080  # 监听端口
sizeLimit = 125 # 125MB
H2C = true # 是否开启H2C传输 
cors = "*" # "*"/"" -> "*" ; "nil" -> "" ; 除以上特殊情况, 会将值直接传入

[httpc]
mode = "auto" # "auto" or "advanced" HTTP客户端模式 自动/高级模式
maxIdleConns = 100 # only for advanced mode 仅用于高级模式
maxIdleConnsPerHost = 60 # only for advanced mode 仅用于高级模式
maxConnsPerHost = 0 # only for advanced mode 仅用于高级模式

[gitclone]
mode = "bypass" # bypass / cache 运行模式, cache模式依赖smart-git
smartGitAddr = "http://127.0.0.1:8080" # smart-git组件地址
ForceH2C = false # 强制使用H2C连接

[shell]
editor = false # 脚本嵌套加速

[pages]
mode = "internal" # "internal" or "external" 内部/外部 前端 默认内部
theme = "bootstrap" # "bootstrap" or "nebula" 内置主题
staticPath = "/data/www"  # 静态页面文件路径

[log]
logFilePath = "/data/ghproxy/log/ghproxy.log" # 日志文件路径
maxLogSize = 5 # MB 日志文件最大大小
level = "info"  # 日志级别 dump, debug, info, warn, error, none

[auth]
authMethod = "parameters" # 鉴权方式,支持parameters,header
authToken = "token"  # 用户鉴权Token
enabled = false  # 是否开启用户鉴权
ForceAllowApi = false # 在不开启Header鉴权的情况下允许api代理

[blacklist]
blacklistFile = "/data/ghproxy/config/blacklist.json"  # 黑名单文件路径
enabled = false  # 是否开启黑名单

[whitelist]
enabled = false  # 是否开启白名单
whitelistFile = "/data/ghproxy/config/whitelist.json"  # 白名单文件路径

[rateLimit]
enabled = false  # 是否开启速率限制
rateMethod = "total" # "ip" or "total" 速率限制方式
ratePerMinute = 180  # 每分钟限制请求数量
burst = 5  # 突发请求数量

[outbound]
enabled = false # 是否使用自定义代理出站
url = "socks5://127.0.0.1:1080" # "http://127.0.0.1:7890" 支持Socks5/HTTP(S)出站传输
```

### 黑名单配置

黑名单配置位于config/blacklist.json,格式如下:

```json
{
    "blacklist": [
      "test/test1",
      "example/repo2",
      "another/*"
      "another"
    ]
  }
```

### 白名单配置

白名单配置位于config/whitelist.json,格式如下:

```json
{
    "whitelist": [
      "test/test1",
      "example/repo2",
      "another/*"
      "another"
    ]
  }
```