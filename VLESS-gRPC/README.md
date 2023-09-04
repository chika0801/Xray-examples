### 注意：

:exclamation:gRPC/H2 建议在有优化回程路由的VPS上使用。如 CN2-GIA、AS9929/AS10099、CMI/CMIN2、AS4837 等。并且你到VPS之间的延迟越低越好。建议参考 NaïveProxy 的 [Performance Tuning](https://github.com/klzgrad/naiveproxy/wiki/Performance-Tuning) 进行优化。除此以外，可以参考[文档](https://xtls.github.io/Xray-docs-next/config/transports/grpc.html#grpcobject)，使用[健康检查](https://github.com/chika0801/Xray-examples/blob/main/VLESS-gRPC/config_client.json#L60)参数。

**将 chika.example.com 替换成你的 SSL 证书中包含的域名**

### v2rayN - V6.19 及以上版本 配置示例

<details><summary>点击查看</summary><br>

| 名称 | 值 |
| :--- | :--- |
| 地址 | 服务端的 IP |
| 端口 | 443 |
| 用户ID | chika |
| 流控 | 留空 |
| 加密方式 | none |
| 传输协议 | grpc |
|  | multi |
| 伪装域名 | 留空 |
| 路径 | chika |
| 传输层安全 | tls |
| SNI | chika.example.com |
| Fingerprint | chrome |
| Alpn | 留空 |
| 路过证书验证 | false |

</details>

### v2rayNG - V1.8.1 及以上版本 配置示例

<details><summary>点击查看</summary><br>

| 名称 | 值 |
| :--- | :--- |
| 地址 | 服务端的 IP |
| 端口 | 443 |
| 用户ID | chika |
| 流控 | 留空 |
| 加密方式 | none |
| 传输协议 | grpc |
| gRPC 传输模式 | multi |
| 伪装域名 | 留空 |
| path | chika |
| 传输层安全 | tls |
| SNI | chika.example.com |
| Fingerprint | chrome |
| Alpn | 留空 |
| 路过证书验证 | false |

</details>

### Shadowrocket - V2.2.31 及以上版本 配置示例

<details><summary>点击查看</summary><br>

| 名称 | 值 |
| :--- | :--- |
| 类型 | VLESS |
| 地址 | 服务端的 IP |
| 端口 | 443 |
| UUID | chika |
| TLS | 选上 |
| XTLS | none |
| 允许不安全 | 不选 |
| SNI | chika.example.com |
| ALPN | 留空 |
| 公钥 | 留空 |
| 短 ID | 留空 |
| 传输方式 |  |
| 名称 | grpc |
| Host | 留空 |
| 服务名称 | chika |
| 多路复用 | 不选 |
| TCP 快速打开 | 不选 |
| UDP 转发 | 选上 |
| 代理通过 | 不选 |

</details>

### PassWall - V4.61 及以上版本 配置示例

<details><summary>点击查看</summary><br>

| 名称 | 值 |
| :--- | :--- |
| 类型 | Xray |
| 传输协议 | VLESS |
| 地址（支持域名） | 服务端的 IP |
| 端口 | 443 |
| 加密方式 | none |
| ID | chika |
| TLS | 勾上 |
| flow | 停用 |
| REALITY | 不勾 |
| alpn | 默认 |
| 域名 | chika.example.com |
| 允许不安全连接 | 不勾 |
| 指纹伪造 | chrome |
| 传输协议 | gRPC |
| ServiceName | chika |
| gRPC 传输模式 | multi |
| 健康检查 | 不勾 |
| 初始窗口大小 | 0 |
| MUX | 不勾 |

</details>

### ShadowSocksR Plus+ 配置示例

<details><summary>点击查看</summary><br>

| 名称 | 值 |
| :--- | :--- |
| 服务器节点类型 | V2Ray/Xray |
| V2Ray/XRay 协议 | VLESS |
| 服务器地址 | 服务端的 IP |
| 端口 | 443 |
| Vmess/VLESS ID (UUID) | chika |
| VLESS 加密 | none |
| 传输协议 | gRPC |
| gRPC 服务名称 | chika |
| gRPC 模式 | Multi |
| 初始窗口大小 | 0 |
| H2/gRPC 健康检查 | 不勾 |
| TLS | 勾上 |
| 指纹伪造 | chrome |
| TLS 主机名 | chika.example.com |
| TLS ALPN | 留空 |
| 允许不安全连接 | 不勾 |
| Mux | 不勾 |
| 自签证书 | 不勾 |
| 启用自动切换 | 不勾 |
| 本地端口 | 1234 |

</details>
