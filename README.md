# WEAPON-ARCHIVE
minecraftRPG服务器前端武器收录库网页
# Revel 武器档案库

Revel 服务器的 mcRPG 武器档案系统，支持武器展示、筛选搜索、查询排行等功能。
# ⚠️注意:logo.png和index文件请更改为自己服务器的信息。请全部修改！

## 功能特性

- **武器档案展示** — 卡片式浏览所有武器道具
- **武器详情页** — 点击卡片查看完整档案（属性、技能、背景故事）
- **筛选系统** — 按品质、武器类型筛选
- **全文搜索** — 搜索武器名称、技能、描述
- **查询排行** — 武器查询次数排行、武器伤害排行
- **公告系统** — 发布服务器公告
- **总控配置** — 通过配置文件控制所有功能开关

## 项目结构

```
├── index.html              # 武器列表主页
├── weapon-detail.html      # 武器详情页
├── rank.html               # 排行页面
├── rank.php                # 排行 API（PHP + SQLite）
├── logo.png                # Logo 图片
├── a/                      # 数据目录
│   ├── config.json         # 总控配置文件
│   ├── index.json          # 武器索引
│   ├── announcements.json  # 公告数据
│   ├── rank.db             # SQLite 数据库（自动生成）
│   ├── sword/              # 剑刃类武器
│   ├── staff/              # 法杖类武器
│   ├── bow/                # 弓弩类武器
│   ├── axe/                # 战斧类武器
│   ├── dagger/             # 匕首类武器
│   ├── spear/              # 长枪类武器
│   ├── hammer/             # 重锤类武器
│   └── scythe/             # 镰刀类武器
└── README.md
```

## 快速开始

### 环境要求

- PHP 7.4+（需启用 SQLite3 扩展）
- Web 服务器（Apache / Nginx / PHP 内置服务器）

### 部署

```bash
# 解压到 Web 目录
tar -xzf revel-weapon-archive.tar.gz -C /var/www/html/

# 或使用 PHP 内置服务器测试
php -S 0.0.0.0:8000
```

首次访问排行页面时，`rank.php` 会自动创建 SQLite 数据库。

## 配置说明

### 总控配置 (`a/config.json`)

```json
{
    "site": {
        "name": "Revel 武器档案库",        // 站点名称
        "maintenance": false,               // 维护模式开关
        "maintenanceMessage": "维护中..."    // 维护提示文案
    },
    "navigation": {
        "showWeapons": true,                // 筛选按钮
        "showRank": true,                   // 排行按钮
        "showHelp": true                    // 帮助按钮
    },
    "announcements": {
        "enabled": true                     // 公告显示开关
    },
    "weapons": {
        "enabled": true,                    // 武器库总开关
        "allowDetail": true                 // 详情页访问开关
    },
    "rank": {
        "enabled": true,                    // 排行功能开关
        "apiBase": "/rank.php"              // API 路径
    },
    "externalLinks": [                      // 顶部栏外部链接
        {
            "id": "qq",
            "enabled": true,
            "icon": "users",                // Lucide 图标名
            "url": "https://qm.qq.com/q/xxx",
            "title": "QQ 群",
            "target": "_blank"
        },
        {
            "id": "github",
            "enabled": true,
            "icon": "github",
            "url": "https://github.com/xxx",
            "title": "GitHub",
            "target": "_blank"
        }
    ]
}
```

### 公告数据 (`a/announcements.json`)

```json
{
    "announcements": [
        {
            "id": "001",
            "date": "2026-05-16",
            "title": "公告标题",
            "content": "公告内容",
            "type": "info"                  // info | update | warning
        }
    ]
}
```

## 武器数据

### 武器索引 (`a/index.json`)

所有武器必须在此文件中注册，格式：

```json
{
    "weapons": [
        {
            "id": "RFT-0001",
            "name": "达摩克利斯之剑",
            "category": "sword",
            "file": "a/sword/达摩克利斯之剑.json"
        }
    ]
}
```

### 武器文件 (`a/<type>/<name>.json`)

每个武器一个 JSON 文件，格式：

```json
{
    "id": "RFT-0001",
    "name": "达摩克利斯之剑",
    "type": "sword",
    "typeName": "臆想 · 剑刃",
    "rarity": "madness",
    "rarityName": "癫狂",
    "levelReq": 0,
    "source": "深渊具象",
    "stats": {
        "精神破坏力": "49",
        "意识攻速": "1.4",
        "神智暴击率": "+20%",
        "裂隙暴击伤害": "+60%"
    },
    "skill": {
        "name": "Attack Ⅱ · 王之咒诅",
        "effects": ["命中目标时施加「王之咒诅」", "被动：力量愈盛，神智愈溃"],
        "desc": "王权高悬，锋芒如悬顶之劫\n一念荣光，一念万劫不复"
    },
    "lore": "武器背景故事...",
    "warning": "持之者 必承深渊反噬"
}
```

### 品质等级

| 品质 | 英文 ID | Minecraft 颜色码 | 色值 |
|------|---------|-----------------|------|
| 破损 | worn | &7 | #aaa |
| 陈旧 | aged | &8 | #555 |
| 紧绷 | tense | &9 | #5555ff |
| 压抑 | oppressive | &3 | #00aaaa |
| 清醒 | lucid | &b | #55ffff |
| 癫狂 | madness | &5 | #aa00aa |
| 裂隙 | rift | &c | #ff5555 |

### 武器类型

| 类型 | 英文 ID |
|------|---------|
| 剑刃 | sword |
| 法杖 | staff |
| 弓弩 | bow |
| 战斧 | axe |
| 匕首 | dagger |
| 长枪 | spear |
| 重锤 | hammer |
| 镰刀 | scythe |

## 添加新武器

1. 在 `a/<type>/` 目录创建 JSON 文件
2. 在 `a/index.json` 的 `weapons` 数组中添加条目
3. 刷新页面即可

## 排行功能

排行数据通过 `rank.php` API 获取：

- `?action=view&id=RFT-0001` — 记录武器查询次数
- `?action=rank&type=views` — 查询次数排行
- `?action=rank&type=damage` — 武器伤害排行

## 技术栈

- **前端**: HTML / CSS / JavaScript（原生）
- **图标**: Lucide Icons
- **字体**: Noto Sans SC
- **后端**: PHP + SQLite3
- **数据存储**: JSON 文件 + SQLite

## License

MIT

