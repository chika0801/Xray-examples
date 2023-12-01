## **配置介绍：** 

| | 无需注册域名 | 解决 TLS in TLS | 自带多路复用 | 通过 CDN 访问 |
| :--- | :---: | :---: | :---: | :---: |
| **VLESS-Vision-TLS** | :x: | :heavy_check_mark: | :x: | :x: |
| **VLESS-Vision-REALITY** | :heavy_check_mark: | :heavy_check_mark: | :x: | :x: |
| **VLESS-gRPC-REALITY** | :heavy_check_mark: | :x: | :heavy_check_mark: | :x: |
| **VLESS-HTTP2-REALITY** | :heavy_check_mark: | :x: | :heavy_check_mark: | :x: |
| **VLESS-gRPC-TLS** | :x: | :x: | :heavy_check_mark: | :heavy_check_mark: |

| | 使用 uTLS | 使用 Vision | 服务端 TLS 指纹 | Mux(TCP) | Mux(UDP) |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **VLESS TLS** | 可选（推荐使用） | 可选 | Go | :x: **2** | :heavy_check_mark: |
| **VLESS REALITY** | 必选 | 可选 | Go **1** | :x: **2** | :heavy_check_mark: |
| **VLESS gRPC REALITY** | 必选 | 不能 | Go **1** | :x: **3** | :heavy_check_mark: |
| **VLESS HTTP2 REALITY** | 必选 | 不能 | Go **1** | :x: **3** | :heavy_check_mark: |
| **VLESS gRPC TLS** | 可选（推荐使用） | 不能 | Nginx | :x: **3** | :heavy_check_mark: |

**1：** 可选偷自己时为Nginx<br>
**2：** 使用Vision时不能<br>
**3：** 自带多路复用

[Mux](https://xtls.github.io/Xray-docs-next/config/outbound.html#muxobject) 配置示例

> Mux 配置只需要在客户端启用，服务端自动适配

```jsonc
            "mux": {
                "enabled": true,
                "concurrency": -1, // 不使用 Mux(TCP)
                "xudpConcurrency": 16, // 使用 Mux(UDP) ，是 UDP over TCP，若使用 Vision，还会加 padding
                "xudpProxyUDP443": "reject"
            }
```

:+1:**XTLS Vision [原理](https://github.com/XTLS/Xray-core/discussions/1295) [安装指南](https://github.com/chika0801/Xray-install)**

:+1:**REALITY [设计哲学](https://github.com/XTLS/Xray-core/issues/1689#issuecomment-1439447009) [原理拾零](https://github.com/XTLS/Xray-core/issues/1891#issuecomment-1495439413) [配置说明](https://github.com/XTLS/REALITY#readme)**

## **[GUI 客户端](https://github.com/XTLS/Xray-core/blob/main/README.md#gui-clients)** 
