# Shadowsocks-Rust 服务构建与优化
## 1. **准备**：
- VPS 服务器
- 系统版本：Debian 10-12
- VPS root 用户操作

## 2. [Shadowsocks-Rust](https://crates.io/crates/shadowsocks-rust) 安装

(1) 添加 Teddysun Shadowsocks Repository 的公钥:

```
apt-get update

apt-get -y install lsb-release ca-certificates curl gnupg

curl -fsSL https://dl.lamp.sh/shadowsocks/DEB-GPG-KEY-Teddysun | gpg --dearmor --yes -o /usr/share/keyrings/deb-gpg-key-teddysun.gpg

chmod a+r /usr/share/keyrings/deb-gpg-key-teddysun.gpg
```

(2) 执行以下命令直接安装 Teddysun Shadowsocks Repository：

```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/deb-gpg-key-teddysun.gpg] https://dl.lamp.sh/shadowsocks/debian/ $(lsb_release -sc) main" >/etc/apt/sources.list.d/teddysun.list
```

(3) 重建 repo 缓存，执行如下命令：

```
apt-get update
```

(4) 通过 apt-get 来安装 Shadowsocks-Rust 软件包

```
apt-get install -y shadowsocks-rust
```

(5) 以后软件若有升级，也可通过 apt-get 来升级软件包

```
apt-get install --only-upgrade -y shadowsocks-rust
```

(6) 若想卸载，也可通过 apt-get 来卸载软件包

```
apt-get remove shadowsocks-rust
```

(7) 成功安装后，执行以下命令查看版本号：

```
ssservice --version
```

​    返回值：

```
shadowsocks 1.17.1
```

## **3. [simple-obfs](https://github.com/shadowsocks/simple-obfs) 安装**

(1) 安装 simple-obfs 软件包

```
install -y shadowsocks-simple-obfs
```

(2) 成功安装后，执行以下命令查看版本号：

```
obfs-server -h
obfs-local -h
```

​     返回值

```
simple-obfs 0.0.5
```

## 4. 配置文件如下，如需自定义端口，密码，请自行修改

   ```
   /etc/shadowsocks/shadowsocks-rust-config.json
   ```

   ```json
   {
       "server": "my_server_ip",
       "server_port": 8388,
       "password":"rwQc8qPXVsRpGx3uW+Y3Lj4Y42yF9Bs0xg1pmx8/+bo=",
       "method": "aes-256-gcm",
       "local_address": "127.0.0.1",
       "local_port": 1080
   }
   ```

   

5. 启动服务端

```
systemctl start shadowsocks-rust-server
```

6.查看服务端状态

```
systemctl status shadowsocks-rust-server
```

7.停止服务端

```
systemctl status shadowsocks-rust-server
```

8.设置服务端 service 开机自启动

```
systemctl enable shadowsocks-rust-server
```


