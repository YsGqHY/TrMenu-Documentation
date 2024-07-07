---
description: 本节需要有一定的 Java / JavaScript 或类似编程基础方便理解
---

# JavaScript

## 对象

TrMenu 的 JavaScript 引擎目前提供以下对象

* `bukkitServer` 即 Bukkit.getServer\(\)
* `utils` 即 me.arasple.mc.trmenu.module.internal.script.js.Assist.INSTANCE
* `player` 即 玩家本身
* `session` 即 me.arasple.mc.trmenu.module.display.MenuSession

### 插件挂钩

可以在 JavaScript 中调用挂钩的插件,比如我想调用 EcoItems 的 API 来获得物品,可以通过`ecoitems.getItem("my_item")`来获得

聪明的你应该已经发现了,绑定到 JavaScript 命名空间的名称是插件名字的全小写

目前支持挂钩的

* EcoItems
* Floodgate
* HeadDatabase
* HMCCosmetics
* ItemsAdder
* MagicCosmetics (注意:这个插件的 JS 命名空间是 magicAPI)
* MagicGem
* MMOItems
* NBTAPI
* NeigeItems
* Oraxen
* PlayerPoints
* SkinsRestorer
* Skulls
* SXItem
* Triton
* Vault
* Zaphkiel
* MythicMobs

如果你并不想要，可以关闭`Export-Hook-Plugin`

### Java 命名空间

#### 别名

在`settings.yml`中的`Bindings.Binding-Map`可以将类以别名导出到 JS 命名空间

比如
```yaml

Bindings:
  Binding-Map:
    "Bukkit": "org.bukkit.Bukkit"
    "EntityType": "org.bukkit.entity.EntityType"
```

#### 导入

如果你不需要,可以关闭`Mozilla-Compat`以节省性能

(不推荐使用!)

`importClass(className)` 可以将类导入到当前命名空间,比如

```javascript
importClass(java.lang.System)
System.out.println("TrMenu 真 NB")
```

`importPackage(packageName)`可以将整个包导入到当前命名空间,比如

```javascript
importPackage(java.lang)
System.out.println("TrMenu 真 NB")
```

### 直接访问

(不推荐!!!)

支持直接访问 Java 命名空间

```javascript
java.lang.System.out.println("TrMenu 真 NB")
```

#### 奇技淫巧

```javascript
java.lang["System"].out.println("TrMenu 真 NB")
```

### Java.type

创建别名

```javascript
var System = Java.type("java.lang.System")
System.out.println("TrMenu 真 NB")
```

## 函数

TrMenu 的 JavaScript 引擎目前提供以下函数

* `vars(String input)`  返回替换函数变量
* `varInt(String input)` 替换变量并转换为整型

## 注意

* TrMenu 的 JavaScript 均会预编译缓存，一切变量使用都需要通过函数处理
* 表示 “或” 的符号为 `||` , 表示 “与” 的符号为 `&&`

## 实例

* 判断有无权限
  * 可直接调用 player 对象的方法，`player.hasPermission("perm")` 

## utils

{% embed url="https://github.com/TrPlugins/TrMenu/blob/stable/v3/plugin/src/main/kotlin/trplugins/menu/module/internal/script/js/Assist.kt" caption="TrMenu/Assist.kt" %}

