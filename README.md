<div align="center">

# EclipseCore · 日蚀核心

**一款面向 Minecraft: Pocket Edition 0.14.x 的高性能二次开服核心**

基于 PocketMine-MP 深度定制，内置反作弊、红石、AI、多世界与一整套管理/娱乐插件，开箱即服。

[![Releases](https://img.shields.io/badge/download-Releases-blue?style=flat-square&logo=github)](https://github.com/xiaoxu114514/EclipseCore/releases)
[![MC Version](https://img.shields.io/badge/MCPE-0.14.0%20~%200.14.3-green?style=flat-square&logo=minecraft)](https://github.com/xiaoxu114514/EclipseCore)
[![PHP](https://img.shields.io/badge/PHP-7.4%20~%208.0-777BB4?style=flat-square&logo=php)](https://www.php.net/)
[![API](https://img.shields.io/badge/EclipseCore%20API-1.1.7-orange?style=flat-square)](https://github.com/xiaoxu114514/EclipseCore)
[![Version](https://img.shields.io/badge/version-0.5.2-red?style=flat-square)](https://github.com/xiaoxu114514/EclipseCore/releases)
[![License](https://img.shields.io/badge/License-LGPL%20v3-yellow?style=flat-square)](https://www.gnu.org/licenses/lgpl-3.0.html)

</div>

---

## 项目简介

**EclipseCore（日蚀核心）** 是一套专为 **Minecraft: Pocket Edition 0.14.0 – 0.14.3** 设计的服务端内核， fork 自老牌 PocketMine-MP，并在其上做了大量针对老版本 PE 联机场景的重构与增强。

它把传统上需要额外安装的常用插件（举报、反刷屏、反顶号、境外 IP 拦截、恶搞指令、背包备份、服务器监控等）**直接编译进内核**，同时补齐了老版本缺失的地狱/末地生成器、红石电路、实体 AI、假矿反透视等能力，适合怀旧服、小游戏服、生存服一键开服。

> 当前版本：`0.5.2` ｜ 兼容 PocketMine API：`2.0.0` ｜ EclipseCore API：`1.1.7`

---

## 核心特性

### 世界与玩法
- **原生三世界生成器**：普通主世界（Normal / Flat）、地狱（Nether，含地狱要塞 NetherFortress）、末地（Ender，含末地石柱 EnderPilar）。
- **自动多世界**：在 `eclipsecore.yml` 打开开关后，启动即自动生成 `nether/`、`ender/` 维度。
- **天气系统**：随机天气、雷暴、闪电引燃、火焰蔓延均可配置。
- **饥饿 & 经验**：完整饥饿扣血、经验掉落、死亡不掉落/经验开关。
- **红石电路**：独立红石子系统，支持频率脉冲（可配置 1s/任意秒级周期）。
- **喷溅药水、铁砧与附魔台、创造物品表、自定义合成表**（`recipes.json` / `creativeitems.json` 可外置）。

### 反作弊与安全
| 模块 | 作用 |
|---|---|
| `anti-fly` | 反飞行作弊 |
| `Anti-Xray` | 假矿反透视 |
| `Anti-FastEat` | 反秒吃 |
| `Anti-FastBreak` | 反秒挖 |
| `Anti-GameSpeed` | 反 Toolbox 加速 |
| `Anti-Toolbox` | 检测 Toolbox 杂项（effect、inventory 等） |
| `netsh-Block` | Windows 下调用系统防火墙按 IP 封禁（需管理员） |
| `AntiForeignIP` | 内核级拦截境外及港澳台 IP，防御海外假人压测 |
| `EclipseAntiRob` | 同名玩家在线时禁止被顶号，防盗号 |

### 性能与运维
- **异步区块请求**（`async-chunk-request`），减少主线程卡顿。
- 经验数据预生成缓存（`experience-cache`）。
- 多服人数统一（`dserver`）：Motd/Query 可跨服聚合显示在线人数。
- 玩家进出服消息可在 Message / Tip / Popup 三种形式间切换。
- 完整崩溃转储（crashdumps）、日志分级、插件热加载。

---

## 环境要求

| 项目 | 要求 |
|---|---|
| 操作系统 | Linux / Windows / macOS（推荐 Ubuntu 20.04+） |
| PHP 版本 | **7.4 ~ 8.0**（内核已做版本判断，过高或过低都会拒绝启动） |
| PHP 扩展（必须） | `pthreads` `sockets` `uopz` `curl` `yaml` `sqlite3` `zlib` `gd` |
| PHP 扩展（可选） | `leveldb`（LevelDB 世界存储，强烈推荐） |
| 内存 | 最低 1 GB，推荐 2 GB+ |
| 网络 | UDP 端口 19132 放行（默认） |

> 本内核运行在 **PHP 7.4/8.0 + pthreads** 这条老 PocketMine 工具链上，不能直接用系统 `php` 启动，需使用 PocketMine 官方或第三方编译好的 **PHP 二进制**（带 pthreads/uopz/yaml）。

---

## 快速开始

### 1. 下载
从 [Releases 页面](https://github.com/xiaoxu114514/EclipseCore/releases) 下载最新的 `EclipseCore.phar`，或 clone 源码：

```bash
git clone https://github.com/xiaoxu114514/EclipseCore.git
cd EclipseCore
```

### 2. 准备 PHP 运行时
使用与内核配套的 PHP 二进制（带 pthreads、uopz、yaml 等扩展）。

### 3. 启动
```bash
# phar 方式
php74 -d pthreads.enable=1 EclipseCore.phar

# 源码方式
php74 src/pocketmine/PocketMine.php
```

首次启动会自动生成：
- `pocketmine.yml` —— 内核基础配置
- `eclipsecore.yml` —— EclipseCore 高级特性配置（饥饿/经验/红石/反作弊/多世界等）
- `plugins/` —— 插件目录（内置插件数据会落在 `plugins/EclipseCore/` 下）
- `worlds/` —— 世界存档目录

按向导输入服务器名称、端口、人数上限、游戏模式即可进入游戏。

---

## 目录结构

```
EclipseCore/
├── src/
│   ├── pocketmine/              # 主内核
│   │   ├── resources/           # 默认配置 & 物品表
│   │   │   ├── eclipsecore_chs.yml
│   │   │   ├── eclipsecore_eng.yml
│   │   │   ├── pocketmine_chs.yml
│   │   │   ├── pocketmine_eng.yml
│   │   │   ├── creativeitems.json
│   │   │   └── recipes.json
│   │   ├── command/defaults/     # 内置指令
│   │   ├── level/generator/
│   │   │   ├── normal/           # 主世界
│   │   │   ├── hell/             # 地狱（Nether + NetherFortress）
│   │   │   └── ender/            # 末地（Ender + EnderPilar）
│   │   └── plugin/
│   │       └── InternalPluginLoader.php   # 内置插件加载器
│   ├── raklib/                   # RakNet 网络协议层
│   └── spl/
│       ├── EclipsePlugins.php    # 8 个内置插件源码（单文件多命名空间）
│       ├── Logger.php / Thread.php / ...
│       └── stubs/                # leveldb / pthreads / weakref / yaml 桩
└── README.md
```

---

## 内置插件一览

核心自带以下插件：

| 插件 | 版本 | 作用 | 主要指令 |
|---|---|---|---|
| **EclipseReport** | 2.0.0 | 高级举报 & 自动群众审判：举报累计达阈值自动封人，OP 上线时弹警报 | `/report <玩家> [理由]`（别名 `/jb`）、`/reports` |
| **EclipseTroll** | 3.2.0 | 恶搞合集：模拟发言、伪造退服、视角控制、虚假 OP 提示、原地升天等 | `/egao` `/echat` `/equit` `/espin` `/efakeop` `/efly` `/einfo` |
| **AntiForeignIP** | 1.0.0 | 拒绝境外 & 港澳台 IP 登入，防御海外假人压测；异步查询 IP 归属地 (有时会误判，小心点) | `/ipwl <add\|remove> <IP>` |
| **EclipseInfo** | 1.0.0 | 实时服务器状态面板：TPS、内存、线程、在线人数 | `/info` |
| **EclipseAntiSpam** | 1.1.0 | 聊天频率检测 + 手动禁言 | `/mute <玩家> <分钟> [原因]`、`/unmute <玩家>` |
| **AntiClearBag** | 1.0.1 | 背包防清理：一键备份/回滚指定玩家背包 | `/acb <backup\|restore>`、`/acbclear <玩家>` |
| **EclipseAntiRob** | 1.0.0 | 反盗号：同名玩家在线时禁止新连接顶号 | （被动，无指令） |
| **EclipseBlacklist** | 1.2.0 | 内核黑名单 | `/bl <add\|remove\|list> [玩家] [原因]` |

> 想禁用某个内置插件？直接在 `plugins/` 目录下放一个同名空插件即可（加载器会跳过已存在的同名插件），或自行修改 `InternalPluginLoader::getManifest()`。

---

## 关键配置（`eclipsecore.yml`）

```yaml
level:
  weather: true                # 随机天气
  lightning-time: 200          # 闪电间隔（tick），0 关闭
  lightning-fire: false       # 闪电引燃

player:
  hunger: true                 # 饥饿系统
  experience: true             # 经验系统
  keep-inventory: true          # 死亡不掉落
  keep-experience: false       # 死亡经验不掉落

nether:
  allow-nether: true           # 自动生成地狱
  level-name: "nether"

ender:
  allow-ender: true            # 自动生成末地
  level-name: "ender"

Anti-Cheat:
  anti-fly: true
  Anti-Xray: true
  Anti-FastEat: true
  Anti-FastBreak: true
  Anti-GameSpeed: true
  Anti-Toolbox: true
  netsh-Block: true            # 仅 Windows 生效

redstone:
  enable: true
  frequency-pulse: true
  pulse-frequency: 1           # 脉冲周期（秒）

server:
  async-chunk-request: true
  allow-splash-potion: true
  login-msg: "§3@player 加入了游戏"
  logout-msg: "§3@player 退出了游戏"

ai:
  enable: true
  creeper-explode-destroy-block: false
  mobgenerate: true
```

完整配置项见 [`src/pocketmine/resources/eclipsecore_chs.yml`](src/pocketmine/resources/eclipsecore_chs.yml)。

---

## 🎮 常用指令速查

除内置插件指令外，内核还提供完整的 PocketMine 指令集：

| 分类 | 指令 |
|---|---|
| 服务器 | `/stop` `/reload` `/status` `/version` `/plugins` `/makeplugin` `/extractphar` `/dumpmemory` |
| 玩家 | `/op` `/deop` `/kick` `/ban` `/pardon` `/ban-ip` `/pardon-ip` `/ban-cid` `/list` `/tell` `/me` |
| 世界 | `/gamemode` `/tp` `/summon` `/effect` `/enchant` `/give` `/xp` `/time` `/weather` `/seed` `/setworldspawn` `/spawnpoint` |
| 调试 | `/fill` `/setblock` `/particle` `/chunkinfo` `/biome` `/cave` `/timings` |
| 备份 | `/save-all` `/save-on` `/save-off` |

---

## 从源码构建 phar

> 需要本地已装好配套 PHP 运行时。

```bash
# 进入源码目录
cd EclipseCore

# 使用内核自带的打包指令
php74 src/pocketmine/PocketMine.py --make-server
# 或在内核控制台执行
> make-server
```

构建产物会输出到当前目录下的 `EclipseCore.phar`。

---

## 版本与兼容性

| MCPE 客户端 | PocketMine API | EclipseCore API |
|---|---|---|
| 0.14.0 – 0.14.3 | 2.0.0 | 1.1.7 |

插件开发请使用 EclipseCore API `1.1.7`，并在 `plugin.yml` 中声明：

```yaml
name: MyPlugin
version: 1.0.0
main: MyPlugin\Main
api: [2.0.0]
```

---

## 贡献

欢迎 Issue 与 PR。提交前请确认：
1. 代码风格与现有 `src/pocketmine/` 保持一致（Tab 缩进、PSR-4 风格）。
2. 不破坏 PHP 7.4 ~ 8.0 兼容性。
3. 涉及新功能时同步更新 `eclipsecore_chs.yml` / `eclipsecore_eng.yml` 注释。
（我们也在此感谢 **冷例 (rpg636zjhi)** 发现的几个漏洞）

---

## License

本项目基于 **LGPL v3.0** 协议开源（继承自 PocketMine-MP）。你可以在遵守协议的前提下自由使用、修改与再分发，但修改后的衍生作品需同样以 LGPL 协议开源。

> 内核头部与生成器源码中均保留了原始 LGPL 声明，请勿移除。

---

<div align="center">

**Enjoy EclipseCore · 日蚀之下，开服即用。**

 Report Bug · Request Feature → [Issues](https://github.com/xiaoxu114514/EclipseCore/issues) ｜ [Releases](https://github.com/xiaoxu114514/EclipseCore/releases)

</div>
