# Padavan-PrintScan（局域网打印扫描一体机服务器）

面向 **newifi d1（NEWIFI-D1）** 与 **newifi d2（NEWIFI3）** 的 Padavan 精简固件，为局域网提供 **联想 M7605D** 的 USB 打印与扫描共享。

## 功能构成

| 功能 | 实现 | 说明 |
|---|---|---|
| 打印 | Padavan 内置 USB 打印服务器（u2ec/LPRD） | WebUI「USB应用程序」开启，RAW 9100 / LPR |
| 扫描 | VirtualHere USB 透传 | 路由器端 vhusbd（7575 端口），PC 客户端接管 USB 设备后用联想原厂驱动扫描 |
| 组网 | ZeroTier | WebUI「ZeroTier」页配置 |
| 其他 | 精简基座 | 已关闭 aria2/transmission/minidlna/ffmpeg/samba36/xupnpd/openssl-exe 等 |

## 使用方法

### 打印（无需电脑常开）
1. WebUI → USB应用程序 → 打印服务器，启用。
2. 电脑添加 TCP/IP 打印机：`IP:9100`（RAW），驱动选联想 M7605D 官方驱动。
3. M7605D 为 GDI 主机渲染打印机：各客户端需安装联想驱动（渲染在客户端完成，路由器只透传）。

### 扫描（需一台电脑在线）
1. WebUI → 扩展功能 → VirtualHere，启用（首次启动会从 virtualhere.com 自动下载 vhusbdmipsel 到 /etc/storage/bin，下载失败可手动上传）。
2. PC 装 [VirtualHere Client](https://www.virtualhere.com/clients)（Windows 版官方签名），Hub = 路由器 IP，右键 M7605D → Use。
3. PC 打开联想扫描工具（M74_M76 打印扫描驱动包）即可扫描。

## 注意
- M7605D 扫描引擎为 Brother OEM，闭源驱动仅覆盖 i386/x86_64/arm 等架构，**无 mipsel 版本**——任何路由器固件（OpenWrt/Padavan）都无法在路由器本地完成扫描；VirtualHere 是把 USB 设备透传给电脑、由电脑驱动渲染的方案。
- VirtualHere 免费版限制：同时 1 个 USB 设备（M7605D 整机算 1 个，够用）；扫描时 PC 需在线。
