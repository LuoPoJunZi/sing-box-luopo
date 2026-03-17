# 🚀 Sing-box-LuoPo 魔改管理面板

一款专为 Linux 服务器设计的 **Sing-box** 一键安装与极简管理脚本。基于 233boy 优秀的原版逻辑进行深度重构，专为追求**极致防封锁、可视化极简操作、以及多节点便捷管理**的用户打造。

无论是刚接触 VPS 的小白，还是需要极客级内网穿透的高级玩家，本脚本都能让你在 3 分钟内搭建出最强健的科学上网节点！

---

## ✨ 核心杀手锏功能

- 🖥️ **现代化 TUI 交互面板**：告别繁琐的命令行输入，数字菜单一键直达。**全局支持输入 `0` 安全退出或返回上一级**，防误触体验极佳。
- 🔗 **全生态顶级协议支持**：内置 20+ 种协议。除了基础的 Shadowsocks、Trojan、Hysteria2 外，完美支持最前沿的 **VLESS-REALITY**。
- 📦 **双轨制“一键节点订阅 (Sub)”**：节点太多不好导入？面板一键生成 Base64 订阅代码，或瞬间开启临时 Web 服务（`http://IP:9866/sub.txt`），手机端点击“更新订阅”即可全自动同步所有节点。
- 🛡️ **两大抗封锁/穿透利器**：
  - **AnyTLS**：无缝伪装成海外大厂（如苹果、亚马逊）的正常流量，专治 SNI 阻断与深度包检测 (DPI)。
  - **CFtunnel (内网穿透)**：**没有公网 IP？服务器被墙了？** 直接通过 Cloudflare 边缘隧道拉出安全节点，无需开放任何 VPS 端口！
- 🔐 **隐私脱敏备注**：创建节点时可自定义备注（如“香港落地”）。生成的 URL 链接中，备注部分彻底移除真实的 IP 信息，安全分享无压力。
- 🤖 **全自动智能运维**：全自动识别放行 `UFW/Firewalld/Iptables` 防火墙；内置 Cron 守护任务，实现每周自动更新核心、每天自动清理日志，释放硬盘空间。

---

## ⚡ 快速安装与更新

**系统要求：** Ubuntu 20.04+ / Debian 11+ / CentOS 7+ (支持 x86_64 与 ARM64 架构)
**前提条件：** 必须使用 `root` 用户登录服务器执行。

运行以下一键安装命令：

```bash
bash <(curl -s -L [https://raw.githubusercontent.com/LuoPoJunZi/sing-box-luopo/main/install.sh](https://raw.githubusercontent.com/LuoPoJunZi/sing-box-luopo/main/install.sh))
```
*(💡 提示：安装过程中会引导你自动创建一个默认的 VLESS-REALITY 节点，全程只需按回车即可完成。)*

---

## 📖 新手快速上手指南

安装完成后，你在终端只需记住一个极其简单的短命令：

```bash
sb
```
*(输入 `sb` 并回车，即可随时唤出管理主面板。)*

### 1. 如何添加一个新节点？
1. 输入 `sb` 打开面板，输入 `1` 选择 **[添加配置]**。
2. 按照分类选择你需要的协议（新手强烈推荐选择 **18. VLESS-REALITY** 或 **20. AnyTLS**，极其抗封锁）。
3. 按照提示一直按回车（脚本会自动为你分配空闲端口并放行防火墙）。
4. 给节点起个名字（自定义备注），按下回车，你的节点就建好了！屏幕上会直接打印出类似 `vless://...` 的链接。

### 2. 节点太多了，怎么快速导入手机？
进入面板，选择 **(5) 节点订阅 (Sub)**。
脚本会把你在服务器上建好的所有节点打包，你可以选择：
* **方案 A**：直接复制屏幕上的一大段 Base64 乱码，在手机客户端（如 V2rayN, v2rayNG, Clash Verge）选择“从剪贴板导入”。
* **方案 B**：按照屏幕提示打开临时 Web 服务，把 `http://你的IP:9866/sub.txt` 填入客户端的订阅设置中，点击“更新订阅”瞬间全部同步！导入完按回车，临时服务自动销毁，绝对安全。

---

## 🚀 进阶高阶玩法：Cloudflare Tunnel 内网穿透教程

如果你的 VPS 没有公网 IP、端口被封，或者你想极致隐藏服务器的真实 IP，请使用面板中的 **(21) CFtunnel** 协议。

<details>
<summary>👉 点击展开保姆级 CFtunnel 穿透教程</summary>

### 第一阶段：在 Cloudflare 获取 Tunnel Token (首次使用必看)
1. 登录 Cloudflare 主页，进入 **Zero Trust** 面板（首次进入需绑定支付方式开通 Free 免费版，绝对不会扣费） 。
2. 依次点击左侧菜单的 **网络 (Networks) -> Tunnels (隧道)** 。
3. 点击 **Add a tunnel**，选择 **"Cloudflared (推荐)"**，为隧道随便起个名字并保存 。
4. 环境选择 Debian/Ubuntu，在页面下方会生成一串包含 `sudo cloudflared...` 的安装代码 。
5. **提取 Token：** 仔细找到代码里那串 **以 `ey` 开头的超长乱码** 并复制，这就是极其重要的 Tunnel Token 。

### 第二阶段：在 VPS 上部署穿透节点
1. 终端输入 `sb`，选择 `1` 添加配置，协议选 `21` (CFtunnel) 。
2. 提示端口时直接回车自动分配（**请记下屏幕上提示的绿色 5 位数内部端口号，例如 61505**） 。
3. 粘贴刚才复制的 **Token** 并回车 。
4. **输入绑定的域名：** 填入你准备使用的 CF 托管域名（如 `node1.example.com`）并填写备注 。

### 第三阶段：配置公网映射 (最后一步)
1. 回到刚才的 Cloudflare 网页端，点击下一步进入 **路由隧道 (Route Tunnel)** 页面 。
2. **Public Hostnames (公共主机名)** 配置：
   * 子域名：`node1`
   * 域：选择 `example.com`
   * 路径 (Path)：**留空，什么都别填！** 
3. **服务 (Service)** 配置：
   * 类型 (Type)：必须选择 **`HTTP`** 。
   * URL：填写 **`127.0.0.1:你的端口号`**（例如 `127.0.0.1:61505`） 。
4. 点击 **Save hostname** 保存 。

大功告成！现在你可以直接使用 `sb sub` 生成订阅，把节点导入客户端直接起飞了！
</details>

---

## 💻 命令行快捷指令 (CLI)

如果你是极客用户，不想每次都进入面板，可以直接使用以下快捷指令：

| 指令 | 作用说明 |
| --- | --- |
| `sb a <协议>` | 快捷添加指定协议的节点 (如 `sb a reality`) |
| `sb i <备注名>`| 查看指定节点的详细参数和二维码链接 |
| `sb c <备注名>`| 更改指定节点的参数 (端口、密码、域名等) |
| `sb d <备注名>`| 快捷删除指定节点 |
| `sb sub` | 快速唤出订阅生成工具 |
| `sb all` | 清屏并一键罗列所有节点链接 |
| `sb log` | 查看服务器实时运行日志 |
| `sb update` | 检查并更新核心或脚本到最新版本 |

---

## 🆘 常见问题与急救指南

### 1. 急救：我的脚本彻底搞崩了，如何物理卸载？
如果因为错误操作导致面板无法进入，或者你想彻底清理服务器环境，请直接复制以下这段命令到终端执行，它将**无情地把脚本、核心、日志和定时任务连根拔起**，还你一个纯净的系统：

```bash
systemctl stop sing-box caddy 2>/dev/null
systemctl disable sing-box caddy 2>/dev/null
rm -f /lib/systemd/system/sing-box.service /lib/systemd/system/cftunnel-*.service
systemctl daemon-reload
crontab -l 2>/dev/null | grep -v -E "sing-box update|/var/log/sing-box" | crontab -
rm -rf /etc/sing-box /var/log/sing-box /usr/local/bin/sing-box /usr/local/bin/sb
sed -i "/sing-box/d" /root/.bashrc; sed -i "/alias sb=/d" /root/.bashrc
source /root/.bashrc
echo -e "\n✅ 物理清理完成！系统已恢复纯净状态。"
```
清理完毕后，重新执行一键安装命令即可满血复活 [cite: 2]。

### 2. 客户端连不上节点怎么办？
* **检查安全组**：脚本会自动放行 VPS 内部防火墙，但如果你使用的是阿里云、腾讯云、AWS、甲骨文等主流云服务商，**必须登录其网页控制台，手动开放对应节点的端口号**。
* **检查时间**：科学上网协议对时间非常敏感，请确保你的 VPS 系统时间与标准时间误差不超过 90 秒。


## 📜 鸣谢与开源协议

* 本项目基于 [233boy/sing-box](https://github.com/233boy/sing-box) 优秀的底层逻辑进行深度重构与二次开发。
* Sing-box 核心项目：[SagerNet/sing-box](https://github.com/SagerNet/sing-box)。
* 本脚本遵循 GNU GPL v3 开源协议。
