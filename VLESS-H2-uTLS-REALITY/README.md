### 注意：

:exclamation:gRPC/H2 建议在有优化回程路由的VPS上使用。如 CN2-GIA、AS9929/AS10099、CMI/CMIN2、AS4837 等。并且你到VPS之间的延迟越低越好。建议参考 NaïveProxy 的 [Performance Tuning](https://github.com/klzgrad/naiveproxy/wiki/Performance-Tuning) 进行优化。除此以外，可以参考[文档](https://xtls.github.io/Xray-docs-next/config/transports/h2.html#httpobject)，使用[健康检查](https://github.com/chika0801/Xray-examples/blob/main/VLESS-H2-uTLS-REALITY/config_client.json#L65)参数。

### v2rayN - V6.19 及以上版本 配置示例

<details><summary>点击查看</summary><br>

| 名称 | 值 |
| :--- | :--- |
| 地址 | 服务端的 IP |
| 端口 | 443 |
| 用户ID | chika |
| 流控 | 留空 |
| 加密方式 | none |
| 传输协议 | h2 |
| 伪装类型 | none |
| 伪装域名 | 留空 |
| 路径 | 留空 |
| 传输层安全 | reality |
| SNI | `www.lovelive-anime.jp` |
| Fingerprint | chrome |
| PublicKey | Z84J2IelR9ch3k8VtlVhhs5ycBUlXA7wHBWcBrjqnAw |
| ShortId | 6ba85179e30d4fc2 |
| SpiderX | 留空 |

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
| 传输协议 | h2 |
| 伪装类型 | --- |
| 伪装域名 | 留空 |
| path | 留空 |
| 传输层安全 | reality |
| SNI | `www.lovelive-anime.jp` |
| Fingerprint | chrome |
| PublicKey | Z84J2IelR9ch3k8VtlVhhs5ycBUlXA7wHBWcBrjqnAw |
| ShortID | 6ba85179e30d4fc2 |
| SpiderX | 留空 |

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
| SNI | `www.lovelive-anime.jp` |
| ALPN | 留空 |
| 公钥 | Z84J2IelR9ch3k8VtlVhhs5ycBUlXA7wHBWcBrjqnAw |
| 短 ID | 6ba85179e30d4fc2 |
| 传输方式 |  |
| 名称 | h2 |
| Host | `www.example.com` |
| 路径 | / |
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
| REALITY | 勾上 |
| 域名 | `www.lovelive-anime.jp` |
| 公钥 | Z84J2IelR9ch3k8VtlVhhs5ycBUlXA7wHBWcBrjqnAw |
| Short Id | 6ba85179e30d4fc2 |
| Spider X | 留空 |
| 指纹伪造 | chrome |
| 传输协议 | HTTP/2 |
| HTTP/2 主机名 | 留空 |
| HTTP/2 路径 | 留空 |
| 健康检查 | 不勾 |
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
| 传输协议 | HTTP/2 |
| HTTP/2 主机名 | 留空 |
| HTTP/2 路径 | 留空 |
| H2/gRPC 健康检查 | 不勾 |
| TLS | 不勾 |
| REALITY | 勾上 |
| Public key | Z84J2IelR9ch3k8VtlVhhs5ycBUlXA7wHBWcBrjqnAw |
| Short ID | 6ba85179e30d4fc2 |
| spiderX | 留空 |
| 指纹伪造 | chrome |
| TLS 主机名 | `www.lovelive-anime.jp` |
| Mux | 不勾 |
| 启用自动切换 | 不勾 |
| 本地端口 | 1234 |

</details>
