# FRP (Fast Reverse Proxy) 使用指南

# 汉化: 空城 :)

## 1. 概述
FRP 是一个快速反向代理工具，用于帮助用户通过内网穿透技术访问内网服务。`frps` 是 FRP 服务端程序，`frpc` 是 FRP 客户端程序。

## 2. frps (服务端) 的启动

### 2.1 基本启动命令
#### 使用 INI 配置文件
```bash
./frps -c frps.ini
```

#### 使用 TOML 配置文件
```bash
./frps -c frps.toml
```

### 2.2 参数说明
- `-c`：指定配置文件路径。`frps.ini` 或 `frps.toml` 是服务端的配置文件，包含服务端的基本配置信息。

### 2.3 配置文件示例

#### INI 配置文件 (`frps.ini`)
```ini
[common]
bind_addr = 0.0.0.0
bind_port = 7000
dashboard_addr = 0.0.0.0
dashboard_port = 7500
dashboard_user = admin
dashboard_pwd = admin
log_file = ./frps.log
log_level = info
log_max_days = 3
token = 12345678
```

#### TOML 配置文件 (`frps.toml`)
```toml
# [common] is integral section
[common]
bind_addr = "0.0.0.0"
bind_port = 7000

dashboard_addr = "0.0.0.0"
dashboard_port = 7500
dashboard_user = "admin"
dashboard_pwd = "admin"

log_file = "./frps.log"
log_level = "info"
log_max_days = 3

token = "12345678"
```

### 2.4 参数含义
- `bind_addr`：服务端绑定的地址。
- `bind_port`：服务端监听的端口。
- `dashboard_addr` 和 `dashboard_port`：仪表板的地址和端口，用于监控和管理 FRP 服务。
- `dashboard_user` 和 `dashboard_pwd`：仪表板的用户名和密码。
- `log_file`：日志文件路径。
- `log_level`：日志级别。
- `log_max_days`：日志文件保留天数。
- `token`：身份验证令牌，用于客户端和服务端的身份验证。

## 3. frpc (客户端) 的启动

### 3.1 基本启动命令
#### 使用 INI 配置文件
```bash
./frpc -c frpc.ini
```

#### 使用 TOML 配置文件
```bash
./frpc -c frpc.toml
```

### 3.2 参数说明
- `-c`：指定配置文件路径。`frpc.ini` 或 `frpc.toml` 是客户端的配置文件，包含客户端的基本配置信息。

### 3.3 配置文件示例

#### INI 配置文件 (`frpc.ini`)
```ini
[common]
server_addr = 192.168.1.100
server_port = 7000
log_file = ./frpc.log
log_level = info
log_max_days = 3
token = 12345678
user = your_name
```

#### TOML 配置文件 (`frpc.toml`)
```toml
# [common] is integral section
[common]
server_addr = "192.168.1.100"
server_port = 7000

log_file = "./frpc.log"
log_level = "info"
log_max_days = 3

token = "12345678"
user = "your_name"
```

### 3.4 参数含义
- `server_addr` 和 `server_port`：服务端的地址和端口。
- `log_file`：日志文件路径。
- `log_level`：日志级别。
- `log_max_days`：日志文件保留天数。
- `token`：身份验证令牌，与服务端的 `token` 一致。
- `user`：客户端的用户名称，用于区分不同的客户端。

## 4. 不同客户端的启动方法

### 4.1 Linux 客户端
在 Linux 系统中，直接运行以下命令启动 `frpc`：
```bash
./frpc -c frpc.ini
```
或使用 TOML 配置文件：
```bash
./frpc -c frpc.toml
```

### 4.2 Windows 客户端
在 Windows 系统中，通过命令提示符运行以下命令启动 `frpc`：
```bash
frpc.exe -c frpc.ini
```
或使用 TOML 配置文件：
```bash
frpc.exe -c frpc.toml
```

### 4.3 macOS 客户端
在 macOS 系统中，直接运行以下命令启动 `frpc`：
```bash
./frpc -c frpc.ini
```
或使用 TOML 配置文件：
```bash
./frpc -c frpc.toml
```

### 4.4 Docker 客户端
如果使用 Docker，可以通过以下命令启动 `frpc`：
```bash
docker run -d --name frpc -v /path/to/frpc.ini:/etc/frpc.ini -v /path/to/local/service:/path/to/local/service moonshot/frpc:latest -c /etc/frpc.ini
```
或使用 TOML 配置文件：
```bash
docker run -d --name frpc -v /path/to/frpc.toml:/etc/frpc.toml -v /path/to/local/service:/path/to/local/service moonshot/frpc:latest -c /etc/frpc.toml
```

## 5. 常见问题

### 5.1 如何查看仪表板？
在浏览器中访问 `http://<dashboard_addr>:<dashboard_port>`，使用 `dashboard_user` 和 `dashboard_pwd` 登录。

### 5.2 如何更新配置文件？
修改配置文件后，重启 `frps` 或 `frpc` 以使更改生效。
frpc只需要在网页端配置页面更新配置即可热重载。

### 5.3 如何查看日志？
日志文件路径在配置文件中指定，可以通过查看日志文件了解程序运行状态。

### 5.4 如何排除启动报错问题？
- **权限问题**：确保你有权限运行 `frps` 和 `frpc`。
- **配置文件路径错误**：确保配置文件路径正确。
- **端口冲突**：确保指定的端口未被其他程序占用。
- **网络问题**：确保客户端可以访问服务端的地址和端口。
- **身份验证问题**：确保客户端和服务端的 `token` 一致。

### 5.5 配置文件示例
在 FRP 的目录中，会有一个 `配置文件` 文件夹，其中包含示例配置文件（如 `frps_full.ini` 和 `frpc_full.ini`以及 `toml` 文件的配置）。你可以参考这些示例文件来配置你的 `frps` 和 `frpc`。