# RTSPtoWeb分享你的ip摄像机到世界!

RTSPtoWeb将您的RTSP流转换为可在web浏览器中使用的格式
如MSE (媒体源扩展) 、WebRTC或HLS。这是完全原生的Golang
不使用FFmpeg或GStreamer!

## 目录

- [安装](#安装)
- [配置](#配置)
- [命令行](#命令行)
- [API文档](#API-文档)
- [限制](#限制)
- [性能](#性能)
- [作者](#作者)
- [许可证](#许可证)

## 安装

### 源码安装

1. 下载源
   ```bash
   $ git clone https://github.com/haowee/RTSPtoWeb
   ```
1. 进入文件夹
   ```bash
    $ cd RTSPtoWeb/
   ```
1. 测试运行
   ```bash
    $ GO111MODULE=on go run *.go
   ```
1. 打开浏览器
    ```bash
    open web browser http://127.0.0.1:8083 work chrome, safari, firefox
    ```

## 安装到Docker

1. 运行docker容器
    ```bash
    $ docker run --name rtsp-to-web --network host ghcr.io/deepch/rtsptoweb:latest 
    ```
1. 打开浏览器
    ```bash
    open web browser http://127.0.0.1:8083 in chrome, safari, firefox
    ```

你可以覆盖 <a href="#example-configjson">配置</a> `/PATH_TO_CONFIG/config.json` 并挂载为docker数据卷:

```bash
$ docker run --name rtsp-to-web \
    -v /PATH_TO_CONFIG/config.json:/config/config.json \
    --network host \
    ghcr.io/deepch/rtsptoweb:latest 
```

## 配置

### 服务设置

```text
debug           - 启用调试输出
log_level       - 日志级别（跟踪、调试、信息、警告、错误、致命或崩溃）

http_demo       - 服务静态文件
http_debug      - 调试 HTTP API 服务
http_login      - HTTP 验证 登录
http_password   - http 验证 密码
http_port       - http 服务 端口
http_dir        - 提供静态文件的路径
ice_servers     - 用于 STUN/TURN 的服务器数组
ice_username    - 用于 STUN/TURN 的用户名
ice_credential  - 用于 STUN/TURN 的凭证
webrtc_port_min - 要使用的最小 WebRTC 端口 （UDP）
webrtc_port_max - 要使用的最大 WebRTC 端口数 （UDP）

https
https_auto_tls
https_auto_tls_name
https_cert
https_key
https_port

rtsp_port       - rtsp 服务 端口
```

### 流 设置

```text
name            - 流 名称
```

### 频道 设置

```text
name            - 频道 名称
url             - 频道 rtsp 地址
on_demand       - 流模式静态（随时运行）或点播（查看时运行）
debug           - 启用调试输出（RTSP 客户端）
audio           - 启用音频
status          - 默认流状态
```

#### 授权播放视频

1 - 启用配置

```text
"token": {
"enable": true,
"backend": "http://127.0.0.1/file.php"
}
```

2 - 尝试

```text
rtsp://127.0.0.1:5541/demo/0?token=you_key
```

file.php need response json

```text
   status: "1" or "0"
 ```

#### RTSP 拉取 模式

  * **on demand** (on_demand=true) - 只有点击播放时才去拉取视频流
  * **static** (on_demand=false) - 一直拉取视频流

### 示例 config.json

```json
{
  "server": {
    "debug": true,
    "log_level": "info",
    "http_demo": true,
    "http_debug": false,
    "http_login": "demo",
    "http_password": "demo",
    "http_port": ":8083",
    "ice_servers": ["stun:stun.l.google.com:19302"],
    "rtsp_port": ":5541"
  },
  "streams": {
    "demo1": {
      "name": "test video stream 1",
      "channels": {
        "0": {
          "name": "ch1",
          "url": "rtsp://admin:admin@YOU_CAMERA_IP/uri",
          "on_demand": true,
          "debug": false,
          "audio": true,
          "status": 0
        },
        "1": {
          "name": "ch2",
          "url": "rtsp://admin:admin@YOU_CAMERA_IP/uri",
          "on_demand": true,
          "debug": false,
          "audio": true,
          "status": 0
        }
      }
    },
    "demo2": {
      "name": "test video stream 2",
      "channels": {
        "0": {
          "name": "ch1",
          "url": "rtsp://admin:admin@YOU_CAMERA_IP/uri",
          "on_demand": true,
          "debug": false,
          "status": 0
        },
        "1": {
          "name": "ch2",
          "url": "rtsp://admin:admin@YOU_CAMERA_IP/uri",
          "on_demand": true,
          "debug": false,
          "status": 0
        }
      }
    }
  },
  "channel_defaults": {
    "on_demand": true
  }
}
```

## 命令行

### 使用 help 显示可用的参数

```bash
./RTSPtoWeb --help
```

#### 响应

```bash
Usage of ./RTSPtoWeb:
  -config string
        config patch (/etc/server/config.json or config.json) (default "config.json")
  -debug
        set debug mode (default true)
```

## API 文档

See the [API docs](/docs/api.md)

## 限制

视频 编码 支持: H264 所有 配置文件

音频 编码 支持: no

## 性能

```bash
CPU usage ≈0.2%-1% one (thread) core cpu intel core i7 per stream
```

## 作者

* **Andrey Semochkin** - *Initial work video* - [deepch](https://github.com/deepch)
* **Dmitriy Vladykin** - *Initial work web UI* - [vdalex25](https://github.com/vdalex25)

See also the list of [contributors](https://github.com/deepch/RTSPtoWeb/contributors) who participated in this project.

## 许可证

This project licensed. License - see the [LICENSE.md](LICENSE.md) file for details

[webrtc](https://github.com/pion/webrtc) follows license MIT [license](https://raw.githubusercontent.com/pion/webrtc/master/LICENSE).

[joy4](https://github.com/nareix/joy4) follows license MIT [license](https://raw.githubusercontent.com/nareix/joy4/master/LICENSE).

## 其他示例

Examples of working with video on golang

- [RTSPtoWeb](https://github.com/deepch/RTSPtoWeb)
- [RTSPtoWebRTC](https://github.com/deepch/RTSPtoWebRTC)
- [RTSPtoWSMP4f](https://github.com/deepch/RTSPtoWSMP4f)
- [RTSPtoImage](https://github.com/deepch/RTSPtoImage)
- [RTSPtoHLS](https://github.com/deepch/RTSPtoHLS)
- [RTSPtoHLSLL](https://github.com/deepch/RTSPtoHLSLL)

[![paypal.me/AndreySemochkin](https://ionicabizau.github.io/badges/paypal.svg)](https://www.paypal.me/AndreySemochkin) - You can make one-time donations via PayPal. I'll probably buy a ~~coffee~~ tea. :tea:
