---
description: 初次安装本插件后，会在插件目录下产生一些文件
---

# 配置

## 文件

{% tabs %}
{% tab title="lang/zh\_CN.yml" %}
TLocale 语言文件, 你可以编辑本插件几乎所有的消息
{% endtab %}

{% tab title="data/globalData.yml" %}
全局缓存数据变量存储的地方

服务器开启状态下请勿编辑
{% endtab %}

{% tab title="data/itemRepository.yml" %}
物品仓库数据存储的地方

服务器开启状态下请勿编辑
{% endtab %}

{% tab title="menus" %}
默认的菜单加载目录

菜单文件（YAML）可放在该目录或其子目录下，将会被插件自动加载
{% endtab %}

{% tab title="settings.yml" %}
TrMenu 的主配置文件
{% endtab %}
{% endtabs %}

## 设置

{% code title="settings.yml \(v3.5.0\)" %}
```yaml
#
# 插件的选项
#
Options:
  # 性能模式: High, Normal, Low
  Running-Performance: Normal
  # 多线程
  Multi-Thread: true
  # 异步载入菜单
  Async-Load-Menus: true
  # 是否启用并发加载菜单
  # 启用后会导致多级捕获器顺序错乱
  Load-Menu-Concurrent: false
  Static-Inventory:
    Java: false
    Bedrock: false
  Packet-Inventory:
    Create-Id: false
  Bedrock-Open-Delay: 20

  Placeholders:
    JavaScript-Parse: false
    Jexl-Parse: false

# 菜单多语言系统
Language:
  Default: 'zh_CN'
  # 将提供的文本解析为玩家语言
  # 若留空则使用玩家本地化设置
  Player: ''
  CodeTransfer:
    zh_hans_cn: 'zh_CN'
    zh_hant_cn: 'zh_TW'
    en_ca: 'en_US'
    en_au: 'en_US'
    en_gb: 'en_US'
    en_nz: 'en_US'

#
# 插件的玩家数据储存方式
#
Database:
  # 使用旧版数据库储存
  Use-Legacy-Database: false

  # Local: SQLITE
  # External: SQL
  Method: SQLITE
  Type:
    SQLite:
      file-name: data
    SQL:
      host: localhost
      port: 3306
      user: root
      password: root
      database: test
  Index:
    # UUID, USERNAME
    Player: USERNAME

  # 新版数据库模块
  SQL:
    # 启用 MYSQL, 否则使用 SQLITE
    enable: false
    host: localhost
    port: 3306
    user: root
    password: root
    database: minecraft
    prefix: trmenu

  # 进服延迟加载数据
  Join-Load-Delay: 40
  # 全局数据跨服同步间隔
  Global-Data-Sync: 200

#
# 菜单加载器
#
Loader:
  # 启用菜单自动重载
  Listen-Files: true
  Menu-Files:
    - 'plugins/CustomMenusFolder'


#
# 菜单设置
#
Menu:
  # 选项
  Settings:
    # 绑定物品触发开启菜单的最低间隔 (防止频刷)
    Bound-Item-Interval: 3
  # 图标
  Icon:
    # 是否默认开启子图标继承主图标
    Inherit: false
    # 显示物品
    Item:
      # 默认名称颜色
      Default-Name-Color: "&7"
      # 默认Lore颜色
      Default-Lore-Color: "&7"
      # 优先着色
      # 若开启，则先替换颜色再处理函数变量
      Pre-Color: false

#
# 动作相关
# 开启 Kether 宽容解析语句后无需添加 * 号
#
Action:
  Using-Component: true
  # 捕获器
  Inputer:
    # 取消词（正则）
    Cancel-Words:
      - 'cancel|quit|end'
      - 'q'
  Kether:
    # 开启Kether语句宽容解析
    # 自 3.5.0 版本删除该选项，强制开启宽容解析
    Allow-Tolerance-Parser: true

#
# 快捷绑定执行的动作
# 具体注解详见 [USAGE-快捷绑定] 章节
#
Shortcuts:
  Offhand: []
  Sneaking-Offhand:
    - condition: 'perm *trmenu.shortcut'
      execute: 'open: Example'
      deny: 'return'
  Right-Click-Player: 'open: Profile'
  Sneaking-Right-Click-Player: [ ]
  PlayerInventory-Border-Left: [ ]
  PlayerInventory-Border-Right: [ ]
  PlayerInventory-Border-Middle: [ ]

#
# 注册自定义命令
# 具体注解详见 [USAGE-命令注册] 章节
#
RegisterCommands:
  openMenus:
    aliases: [ ]
    permission: null
    execute:
      - 'tell: &7Argument `example` Required!'
    arguments:
      example: 'open: example'

#
# JS/JEXL 命名导出
# 具体注解详见 [SCRIPT-JAVASCRIPT] 章节
#
Scripts:
  Export-Hook-Plugin: true
  Mozilla-Compat: true
  # 是否启用 GraalJS 作为引擎
  Enable-GraalJS: false
  Binding-Map:
```
{% endcode %}

## 语言

TODO

