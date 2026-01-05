---
description: 图标的显示属性（子图标 & 默认图标 通用）
---

# 显示

## 示例

```yaml
  # 图标 Id
  'Close':
    # 显示属性更新频率
    update: []
    # 显示部分
    display:
      # 材质，支持多个，动态更新
      material: ''
      # 名称，支持多个，动态更新
      name: ''
      # Lore 描述，支持多组，动态更新
      lore: []
      # NBT
      nbt:
        key: value
      # 物品标签, List 形式
      flags: []
      # 物品数量
      amount: ''
      # 发光效果
      shiny: 'true'
      # 无法破坏
      unbreakable: false
      # 数据值 (Damage)
      data: 0
      # 物品模型 (1.21+)
      item_model: ''
      # 提示文本样式 (1.21+)
      tooltip: ''
      # 隐藏提示文本 (1.21+)
      hide_tooltip: false

```

