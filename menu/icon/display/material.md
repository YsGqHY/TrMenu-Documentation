# 材质

## 普通材质

```yaml
material: 'Red Stained Glass Pane'
```

```yaml
material: 'Wool:3'
```

* **材质实现动画的方式就是写成集合形式，将在设定周期内循环切换**
* 普通材质也支持使用函数变量

## 物品仓库

* TrMenu 内置的物品存储调用功能，通过子命令 Item 实现

```yaml
material: 'repo:myCustomItem'

material: 'repo:%custom_variable_whichreturnstheid%'
```

## 转化格式 JSON

* 通过 `/trmenu item toJson` 转化得到的物品文本格式
* 该方法能支持一切形式的物品

```yaml
material: '{"type":"DIAMOND_SWORD","data":0,"amount":1,"meta":{"Damage":{"type":"INT","data":0}}}'

# 玩家头颅
material: '{"type":"SKULL_ITEM","data":3,"amount":1,"meta":{"SkullOwner":{"Name":{"type":"STRING","data":"%player_name%"}}}}'
```

## 头颅材质

* 已知高版本无法使用头颅材质，可以使用 **转化格式 JSON**

### 玩家头颅

```yaml
material: 'head:%player_name%'

material: 'head:BlackSky'
```

* TrMenu 采用本地 NMS 读取在线玩家的皮肤 + 异步联网读取离线正版玩家 皮肤材质的形式，不卡线程
* 已对 SkinsRestorer 进行完整支持，能够获取到玩家显示的皮肤！

### 自定义纹理头颅

```yaml
material: 'head:eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvNDRmNDUyZDk5OGVhYmFjNDY0MmM2YjBmZTVhOGY0ZTJlNjczZWRjYWUyYTZkZmQ5ZTZhMmU4NmU3ODZlZGFjMCJ9fX0='

material: 'head:44f452d998eabac4642c6b0fe5a8f4e2e673edcae2a6dfd9e6a2e86e786edac0'
```

## 物品源

* TrMenu 对挂钩插件物品支持的实现
* 统一格式为 `source:<ID>:<INPUT>`

### HeadDatabase

```yaml
material: 'source:HeadDatabase:myHead'

material: 'source:HDB:random'
```

### Oraxen

```yaml
material: 'source:ORAXEN:itemId'
```

### ItemsAdder

```yaml
material: 'source:ITEMSADDER:itemId'
material: 'source:IA:anotherItemId'
```

### Zaphkiel

```yaml
material: 'source:ZAPHKIEL:itemId'
material: 'source:ZL:itemId'
```

## \* 材质参数

### 附加值

```yaml
material: '3{data=2}'
```

### Model Data

```yaml
# 1.14+
material: 'coal{model-data=15}'
```

### 皮革染色

```yaml
material: 'leather chestplate {dye=255,255,0}'
```

### 旗帜模式

```yaml
material: 'white banner {banner=RED MOJANG,WHITE}'
```

