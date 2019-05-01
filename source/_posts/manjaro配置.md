---
title: marjaro配置
date: 2019-4-30
tags:
- marjaro
- linux
- 配置
categorise:
- linux
- manjaro
---
## 配置国内源
```shell
# 选择延迟低的源
sudo pacman-mirrors -i -c China -m rank
# 刷新缓存
sudo pacman -Syy
```
## 更新系统
```shell
sudo pacman -Syyu
```
## 安装archlinuxcn签名钥匙&antergos签名钥匙
```shell
sudo pacman -S --noconfirm archlinuxcn-keyring antergos-keyring
```
## 安装yay支持AUR
```shell
git clone https://aur.archlinux.org/yay.git
cd 安装yay支持AUR
makepkg -si  # --skippgpcheck作用是跳过签名验证
```
## 安装搜狗输入法
```shell
sudo pacman -S fcitx-im fcitx-configtool fcitx-sogoupinyin #默认全部安装
# 修改配置文件
sudo nano ~/.xprofile
# 增加
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
# 重启生效
reboot
```
## [更换显卡闭源驱动](https://www.lulinux.com/archives/4722)
```shell
1. 终端里运行sudo mhwd --install pci video-nvidia安装闭源驱动。如果出现文件已存在导致软件包无法正常安装的问题，就用yaourt -S --force lib32-nvidia-utils nvidia-utils linux414-nvidia命令强制安装相关软件包，具体包名可能不止这3个，在终端有提示，注意查看。

2. 运行sudo mhwd-tui，选4，静候结果。

3. 最后不要忘了运行sudo mkinitcpio -P命令以更新initramfs引导文件，否则X桌面会无法进入。

4. 重启 reboot
```
## 使用oh-my-zsh
```shell
$ sudo pacman -S zsh

配置默认shell：

$ sudo vim /etc/passwd

将要修改的用户的shell路径改为 /usr/bin/zsh 即可，也就是将 bash 改为 zsh

curl 方式：
$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

wget 方式
$ sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"

修改主题

$ sudo vim ~/.zshrc

找到 ZSH_THEME="robbyrussell"

修改为 ZSH_THEME="random" 为随机主题，要换其他主题，修改此处即可  \
```
## 搭梯子
```shell
$ aurman -S v2ray
# sudo vim /etc/v2ray/config.json
// Config file of V2Ray. This file follows standard JSON format, with comments support.
// Uncomment entries below to satisfy your needs. Also read our manual for more detail at
// https://www.v2ray.com/
{
  "log": {
    // By default, V2Ray writes access log to stdout.
    "access": "/var/log/v2ray/access.log",

    // By default, V2Ray write error log to stdout.
    "error": "/var/log/v2ray/error.log",

    // Log level, one of "debug", "info", "warning", "error", "none"
    "loglevel": "warning"
  },
  // List of inbound proxy configurations.
  "inbounds": [{
    // Port to listen on. You may need root access if the value is less than 1024.
    "port": 1080,

    // IP address to listen on. Change to "0.0.0.0" to listen on all network interfaces.
    "listen": "127.0.0.1",

    // Tag of the inbound proxy. May be used for routing.
    "tag": "socks-inbound",

    // Protocol name of inbound proxy.
    "protocol": "socks",

    // Settings of the protocol. Varies based on protocol.
    "settings": {
      "auth": "noauth",
      "udp": false,
      "ip": "127.0.0.1"
    },

    // Enable sniffing on TCP connection.
    "sniffing": {
      "enabled": true,
      // Target domain will be overriden to the one carried by the connection, if the connection is HTTP or HTTPS.
      "destOverride": ["http", "tls"]
    }
  },{
    // Port to listen on. You may need root access if the value is less than 1024.
    "port": 1081,

    // IP address to listen on. Change to "0.0.0.0" to listen on all network interfaces.
    "listen": "127.0.0.1",

    // Tag of the inbound proxy. May be used for routing.
    "tag": "http-inbound",

    // Protocol name of inbound proxy.
    "protocol": "http",

    // Settings of the protocol. Varies based on protocol.
    "settings": {
      "auth": "noauth",
      "udp": false,
      "ip": "127.0.0.1"
    }
  }
  ],
  // List of outbound proxy configurations.
    "outbound": {
        "protocol": "vmess",
        "settings": {
            "vnext": [
                {
                    "address": "107.182.182.5",
                    "port": 22,
                    "users": [
                        {
                            "id": "0687b713-6097-413a-95fb-28dca3e71ac3",
                            "level": 1,
                            "alterId": 64
                        }
                    ]
                }
            ]
        }
    },

  // Transport is for global transport settings. If you have multiple transports with same settings
  // (say mKCP), you may put it here, instead of in each individual inbound/outbounds.
  //"transport": {},

  // Routing controls how traffic from inbounds are sent to outbounds.
  "routing": {
    "domainStrategy": "IPOnDemand",
    "rules":[
      {
        // Blocks access to private IPs. Remove this if you want to access your router.
        "type": "field",
        "ip": ["geoip:private"],
        "outboundTag": "blocked"
      },
      {
        // Blocks major ads.
        "type": "field",
        "domain": ["geosite:category-ads"],
        "outboundTag": "blocked"
      }
    ]
  },

  // Dns settings for domain resolution.
  "dns": {
    // Static hosts, similar to hosts file.
    "hosts": {
      // Match v2ray.com to another domain on CloudFlare. This domain will be used when querying IPs for v2ray.com.
      "domain:v2ray.com": "www.vicemc.net",

      // The following settings help to eliminate DNS poisoning in mainland China.
      // It is safe to comment these out if this is not the case for you.
      "domain:github.io": "pages.github.com",
      "domain:wikipedia.org": "www.wikimedia.org",
      "domain:shadowsocks.org": "electronicsrealm.com"
    },
    "servers": [
      "1.1.1.1",
      {
        "address": "114.114.114.114",
        "port": 53,
        // List of domains that use this DNS first.
        "domains": [
          "geosite:cn"
        ]
      },
      "8.8.8.8",
      "localhost"
    ]
  },

  // Policy controls some internal behavior of how V2Ray handles connections.
  // It may be on connection level by user levels in 'levels', or global settings in 'system.'
  "policy": {
    // Connection policys by user levels
    "levels": {
      "0": {
        "uplinkOnly": 0,
        "downlinkOnly": 0
      }
    },
    "system": {
      "statsInboundUplink": false,
      "statsInboundDownlink": false
    }
  },

  // Stats enables internal stats counter.
  // This setting can be used together with Policy and Api.
  //"stats":{},

  // Api enables gRPC APIs for external programs to communicate with V2Ray instance.
  //"api": {
    //"tag": "api",
    //"services": [
    //  "HandlerService",
    //  "LoggerService",
    //  "StatsService"
    //]
  //},

  // You may add other entries to the configuration, but they will not be recognized by V2Ray.
  "other": {}
}


# 设置开机启动
$ systemctl enable v2ray

```
