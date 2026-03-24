---
description: 使用 Minecraft 1.21.6+ 原生 Dialog UI 替代传统箱子容器
---

# Dialog 配置

## 前置要求

* 服务端版本 **Paper 1.21.6** 或更高
* 客户端版本 **1.21.6** 或更高
* 低版本客户端将自动回退到 `Fallback-Menu` 指定的传统菜单

## 快速开始

将菜单的 `Render-Type` 设置为 `DIALOG` 即可启用 Dialog 渲染模式

```yaml
Title: '&0我的 Dialog 菜单'
Render-Type: DIALOG

Dialog:
  Fallback-Menu: 'Example'
  Pages:
    - Id: 'main'
      Type: 'notice'
      Title: '&0提示'
      Body:
        - Id: 'desc'
          Renderer: 'plain_message'
          Text:
            - '&7这是一条提示信息'
      Actions:
        - Id: 'confirm'
          Label: '&a确认'
          Execute:
            - 'tell: &a已确认'
            - 'close'
```

## Dialog 节点

```yaml
Dialog:
  # 最低客户端版本，低于此版本的玩家将触发回退策略
  Min-Version: '1.21.6'
  # 回退菜单 ID，版本不支持时自动打开此菜单
  Fallback-Menu: 'Example'
  # 是否允许 ESC 关闭（默认 true）
  Allow-Esc-Close: true
  # 外部标题，显示在 Dialog 窗口顶部的副标题
  External-Title: '&8Dialogs / Notice'
  # 编译器配置（可选）
  Compiler:
    Strategy: AUTO
    Unsupported-Policy: FALLBACK_MENU
    Grid-Columns: 12
    Content-Max-Width: 360
    Mixin-Assist: false
  # 页面列表
  Pages: []
```

### Compiler 编译器

| 配置项 | 默认值 | 说明 |
| --- | --- | --- |
| Strategy | `AUTO` | 编译策略，可选 `AUTO` / `STRICT` / `FALLBACK` |
| Unsupported-Policy | `FALLBACK_MENU` | 不支持时的策略，可选 `FALLBACK_MENU` / `ERROR` / `IGNORE` |
| Grid-Columns | `12` | 布局模式下的栅格列数 |
| Content-Max-Width | `360` | 内容区域最大宽度（像素） |
| Mixin-Assist | `false` | 是否启用混合辅助 |

## 页面 Pages

每个 Dialog 菜单至少需要一个页面

```yaml
Pages:
  - Id: 'main'
    # 页面类型
    Type: 'notice'
    # 页面标题
    Title: '&0页面标题'
    # 关闭事件
    On-Close:
      - 'sound: BLOCK_CHEST_CLOSE-1-0'
    # 内容区域
    Body: []
    # 按钮
    Actions: []
    # 退出按钮（可选，独立于 Actions）
    Exit-Action:
      Id: 'exit'
      Label: '&c关闭'
      Execute:
        - 'close'
```

### 页面类型 Type

| 类型 | 说明 |
| --- | --- |
| `notice` | 通知型，仅支持 1 个按钮，无退出按钮 |
| `confirmation` | 确认型，仅支持 2 个按钮，无退出按钮 |
| `multi_action` | 多按钮型，支持任意数量按钮和退出按钮 |

* 当按钮数量不满足 `notice` / `confirmation` 的要求时，将自动降级为 `multi_action`

## Body 内容区域

Body 定义 Dialog 中显示的内容元素，按顺序从上到下排列

### plain_message 文本

```yaml
Body:
  - Id: 'desc'
    Renderer: 'plain_message'
    Width: 320
    Text:
      - '&7第一行文本'
      - '&7第二行文本'
```

### item 物品

```yaml
Body:
  - Id: 'icon'
    Renderer: 'item'
    Display:
      material: NETHER_STAR
      name: '&e物品名称'
      lore:
        - '&7物品描述'
```

### input 文本输入

```yaml
Body:
  - Id: 'player_name'
    Renderer: 'input'
    Width: 320
    Label: '&7玩家名称'
    Placeholder: '&8请输入玩家名'
    Max-Length: 16
```

* 通过变量 `%trmenu_dialog_input_player_name%` 读取输入值

### boolean 布尔开关

```yaml
Body:
  - Id: 'agree'
    Renderer: 'boolean'
    Width: 320
    Label: '&7我已阅读规则'
    Initial: false
```

* 通过变量 `%trmenu_dialog_boolean_agree%` 读取值（`true` / `false`）

### single_option 单选

```yaml
Body:
  - Id: 'server'
    Renderer: 'single_option'
    Width: 320
    Label: '&7选择服务器'
    Default-Value: 'survival'
    Options:
      - Id: 'survival'
        Title: '&a生存服'
        Description: '&7主线生存玩法'
      - Id: 'pvp'
        Title: '&c竞技服'
        Description: '&7即时战斗玩法'
```

* 通过变量 `%trmenu_dialog_option_server%` 读取选中的 Option Id

### multi_option 多选

```yaml
Body:
  - Id: 'rewards'
    Renderer: 'multi_option'
    Width: 320
    Label: '&7选择奖励包'
    Default-Value:
      - 'starter'
    Options:
      - Id: 'starter'
        Title: '&a新手包'
      - Id: 'combat'
        Title: '&c战斗包'
```

* 通过变量 `%trmenu_dialog_option_rewards%` 读取选中项，多个值以 `,` 分隔

### number_range 数值滑块

```yaml
Body:
  - Id: 'amount'
    Renderer: 'number_range'
    Width: 320
    Label: '&7数量'
    Min: 1
    Max: 64
    Step: 1
    Default-Value: '1'
```

* 通过变量 `%trmenu_dialog_amount%` 读取值

## Actions 按钮

按钮定义 Dialog 底部的可点击操作

```yaml
Actions:
  - Id: 'confirm'
    Label: '&a确认'
    Width: 160
    Execute:
      - 'tell: &a已确认'
      - 'close'
    Deny:
      - 'tell: &c条件不满足'
  - Id: 'cancel'
    Label: '&c取消'
    Width: 160
    Execute:
      - 'tell: &7已取消'
      - 'close'
```

| 配置项 | 必填 | 说明 |
| --- | --- | --- |
| Id | 是 | 按钮唯一标识 |
| Label | 是 | 按钮显示文本 |
| Width | 否 | 按钮宽度（像素），默认 160 |
| Next-Page | 否 | 点击后跳转到指定页码（从 0 开始） |
| Exit | 否 | 是否标记为退出按钮，默认 `false` |
| Execute | 否 | 点击后执行的动作列表 |
| Deny | 否 | 条件不满足时执行的动作列表 |

### 关闭 Dialog

在 Execute 中使用 `close` 动作来关闭 Dialog

```yaml
Execute:
  - 'tell: &a操作完成'
  - 'close'
```

* 不写 `close` 的按钮点击后会**保持当前 Dialog 界面**，适用于"提交但不关闭"的场景
* `close` 动作与传统箱子菜单中的行为一致，会触发 `Events.Close` 和页面的 `On-Close`

### Exit-Action 退出按钮

退出按钮独立于普通按钮，显示在 Dialog 底部的单独区域

```yaml
Exit-Action:
  Id: 'exit'
  Label: '&c关闭面板'
  Width: 120
  Execute:
    - 'tell: &7已关闭面板'
    - 'close'
```

* 仅 `multi_action` 类型支持退出按钮
* 也可以在 Actions 列表中通过 `Exit: true` 标记

### 翻页

通过 `Next-Page` 实现多页 Dialog 之间的跳转

```yaml
Actions:
  - Id: 'next'
    Label: '&e下一页'
    Next-Page: 1
```

## 布局模式 Layout

布局模式提供栅格化的 Widget 排版，替代手动编写 Body + Actions

```yaml
Pages:
  - Id: 'main'
    Type: 'multi_action'
    Title: '&0布局示例'
    Exit-Action:
      Id: 'exit'
      Label: '&c关闭'
      Execute:
        - 'close'
    Layout:
      Row-Gap: 1
      Sections:
        header:
          row: 1
        content:
          row: 2
        footer:
          row: 6
    Widgets:
      title_text:
        kind: text
        anchor: header
        row: 1
        col-start: 1
        col-span: 12
        text:
          - '&7欢迎使用 Dialog UI'
      name_input:
        kind: input
        anchor: content
        row: 1
        col-start: 1
        col-span: 12
        label: '&7你的名字'
        max-length: 16
      submit:
        kind: action
        anchor: footer
        row: 1
        col-start: 1
        col-span: 12
        label: '&a提交'
        execute:
          - 'tell: &a你好, %trmenu_dialog_input_name_input%'
          - 'close'
```

* 使用布局模式时，`Body` 和 `Actions` 节点将被忽略，所有内容通过 `Widgets` 定义
* 每个 Widget 通过 `anchor` 绑定到 Section，通过 `row` / `col-start` / `col-span` 定位

### Widget 属性

| 属性 | 说明 |
| --- | --- |
| kind | Widget 类型：`text` / `item` / `input` / `boolean` / `single_option` / `multi_option` / `number_range` / `action` |
| anchor | 绑定的 Section 名称 |
| row | 在 Section 内的行号 |
| col-start | 起始列（1 开始） |
| col-span | 占据列数 |
| order | 排序权重，默认 0 |

* `action` 类型的 Widget 会被编译为按钮，其余类型编译为 Body 或 Input 元素
* Widget 的其余属性与对应的 Body 元素一致（如 `label`、`options`、`text` 等）

## 变量

Dialog 会自动将玩家的输入写入 Meta 临时数据，可通过 `{meta:key}` 或 PAPI 变量读取

| 变量 | 说明 |
| --- | --- |
| `%trmenu_dialog_menu%` | 当前 Dialog 菜单 ID |
| `%trmenu_dialog_page%` | 当前页码 |
| `%trmenu_dialog_page_id%` | 当前页面 ID |
| `%trmenu_dialog_action%` | 最后点击的按钮 ID |
| `%trmenu_dialog_input_<id>%` | 文本输入框的值 |
| `%trmenu_dialog_option_<id>%` | 单选 / 多选的值 |
| `%trmenu_dialog_boolean_<id>%` | 布尔开关的值 |
| `%trmenu_dialog_<id>%` | 任意控件的原始值 |

## 完整示例

{% tabs %}
{% tab title="Notice 通知" %}
```yaml
Title: '&0奖励计划'
Render-Type: DIALOG

Dialog:
  Fallback-Menu: 'Example'
  Allow-Esc-Close: true
  External-Title: '&8Dialogs / Notice'
  Pages:
    - Id: 'signup'
      Type: 'notice'
      Title: '&0奖励计划'
      Body:
        - Id: 'desc'
          Renderer: 'plain_message'
          Width: 320
          Text:
            - '&7选择你要领取的奖励包'
        - Id: 'packs'
          Renderer: 'multi_option'
          Width: 320
          Label: '&7奖励包'
          Default-Value:
            - 'starter'
          Options:
            - Id: 'starter'
              Title: '&a新手包'
            - Id: 'combat'
              Title: '&c战斗包'
      Actions:
        - Id: 'submit'
          Label: '&a提交'
          Width: 180
          Execute:
            - 'tell: &a已提交, 奖励包: %trmenu_dialog_option_packs%'
            - 'close'
```
{% endtab %}

{% tab title="Confirmation 确认" %}
```yaml
Title: '&0删除确认'
Render-Type: DIALOG

Dialog:
  Fallback-Menu: 'Example'
  Allow-Esc-Close: false
  External-Title: '&8Dialogs / Confirmation'
  Pages:
    - Id: 'confirm'
      Type: 'confirmation'
      Title: '&0删除确认'
      Body:
        - Id: 'warning'
          Renderer: 'plain_message'
          Width: 320
          Text:
            - '&7确认删除所有数据？此操作不可撤销。'
      Actions:
        - Id: 'confirm'
          Label: '&c确认删除'
          Width: 160
          Execute:
            - 'tell: &c已删除'
            - 'close'
        - Id: 'cancel'
          Label: '&a取消'
          Width: 160
          Execute:
            - 'tell: &a已取消'
            - 'close'
```
{% endtab %}

{% tab title="MultiAction 布局" %}
```yaml
Title: '&0服务器分区'
Render-Type: DIALOG

Dialog:
  Fallback-Menu: 'Example'
  Allow-Esc-Close: true
  External-Title: '&8Dialogs / MultiAction'
  Compiler:
    Grid-Columns: 12
  Pages:
    - Id: 'select'
      Type: 'multi_action'
      Title: '&0服务器分区'
      Exit-Action:
        Id: 'exit'
        Label: '&c关闭'
        Width: 120
        Execute:
          - 'close'
      Layout:
        Sections:
          header:
            row: 1
          content:
            row: 2
          footer:
            row: 5
      Widgets:
        intro:
          kind: text
          anchor: header
          row: 1
          col-start: 1
          col-span: 12
          text:
            - '&7选择你要前往的服务器'
        target:
          kind: single_option
          anchor: content
          row: 1
          col-start: 1
          col-span: 12
          label: '&7目标分区'
          options:
            - id: survival
              title: '&a生存服'
            - id: pvp
              title: '&c竞技服'
        connect:
          kind: action
          anchor: footer
          row: 1
          col-start: 1
          col-span: 6
          label: '&a立即前往'
          execute:
            - 'tell: &a正在连接 %trmenu_dialog_option_target%'
            - 'close'
        queue:
          kind: action
          anchor: footer
          row: 1
          col-start: 7
          col-span: 6
          label: '&e加入队列'
          execute:
            - 'tell: &e已加入队列'
```
{% endtab %}
{% endtabs %}

## 注意

* Dialog 是 Minecraft 1.21.6 新增的原生 UI 系统，不依赖箱子容器
* 不支持传统菜单的 `Layout`（字符布局）、`Icons`、`PlayerInventory` 等配置项
* Dialog 的布局模式（`Layout` + `Widgets`）是独立的栅格系统，与传统菜单的字符布局无关
* 按钮点击后如果 Execute 中没有 `close`，Dialog 会保持打开状态并刷新当前页面
