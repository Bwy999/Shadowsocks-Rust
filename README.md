# Shadowsocks-Rust 服务构建与优化
## 1. 准备：
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
apt-get install -y shadowsocks-simple-obfs
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

## 4. 配置文件修改与服务启动，如需自定义端口，密码，请自行修改

   ```
   /etc/shadowsocks/shadowsocks-rust-config.json
   ```

   ```json
   {
       "server": "your_server_ip",
       "server_port": 2100,
       "local_port":1080,
       "password": "rwQc8qPXVsRpGx3uW+Y3Lj4Y42yF9Bs0xg1pmx8/+bo=",
       "method": "aes-256-gcm",
       "timeout": 60,
       "fast_open":true,
       "mode":"tcp_and_udp",
       "plugin":"obfs-server",
       "plugin_opts":"obfs=tls"
   }
   ```

## 5. Shadowsocks-Rust 服务端优化
**安装 linux 新版内核 开启 BBR Plus 加速**
- Debian 系统安装新版 linux 内核. 运行脚本后 Debian 请选择 43 安装 6.1 最新内核. 根据提示需要重启2次 完成内核安装。
- 开启 BBR 网络加速. 完成上面更换新内核后, 重新运行脚本后 选择 2 然后根据提示选择 BBR 加速, 推荐使用BBR + Cake 组合算法.
- 安装 BBR Plus 内核并开启 BBR Plus. 运行脚本后 选择61 安装原版4.14.129版本 BBR Plus 内核,安装完成重启2次后, 重新运行脚本后 选择 3 根据提示开始 BBR Plus,推荐使用BBR Plus + Cake 组合算法。
- 注意安装过程中 如果弹出大框的英文提示(下面有示例图) "安装linux内核有风险是否终止", 要选择" NO" 不终止. 安装完毕会重启VPS.
```
bash <(curl -Lso- https://git.io/kernel.sh)
```

## 6. Shadowsocks-Rust 服务启动与管理
(1) 启动服务端

```
systemctl start shadowsocks-rust-server
```

(2) 查看服务端状态

```
systemctl status shadowsocks-rust-server
```

(3) 停止服务端

```
systemctl status shadowsocks-rust-server
```

(4) 设置服务端 service 开机自启动

```
systemctl enable shadowsocks-rust-server
```
### 致谢
- [clowwindy](https://github.com/clowwind)
- [Max LV](https://github.com/madeye)
- [ty](https://github.com/zonyitoo) 
- [Teddysun](https://github.com/teddysun)
- [jinwp](https://github.com/jinwyp/one_click_script)
