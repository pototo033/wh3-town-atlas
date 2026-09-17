# 全面战争：战锤3 · 互动城镇地图册

一张可缩放拖拽的战役地图，**每座城镇的标记就是它开局所属派系的旗帜**。
点击大洲按钮会切出该地区的城镇表并自动放大定位；点击旗帜会跳到右侧表格对应行。
数据全部解析自游戏本体文件（`map_data.esf`、DB 表、本地化、`ui/flags` 旗帜贴图），无手工录入。

在线访问：<https://pototo033.github.io/wh3-town-atlas/>

## 这是什么

- **811 座城镇**：超凡帝国 569 座 / 混沌魔域 242 座，覆盖 294 个行省、294 座首府
- **17 个大洲/大地域**：奥苏安、纳迦罗斯、露丝契亚、南地、帝国、诺斯卡、基斯里夫、震旦……
  地图上按大洲分区填色并标注名称，缩放到一定倍率后显示城镇旗帜
- **三种坐标**：世界坐标（战役地图逻辑坐标）、六边形格坐标、底图像素坐标
- 交互：滚轮/双击缩放、拖拽平移、大洲按钮联动、城镇/行省/派系检索、派系筛选、
  仅看首府、坐标网格、显示 KEY（区域 KEY / 派系 KEY）

## 文件说明

| 路径 | 说明 |
| --- | --- |
| `index.html` | 地图册本体（1.9MB，纯静态，无外部依赖） |
| `assets/map_combi.jpg` | 超凡帝国底图（4000×3111） |
| `assets/map_chaos.jpg` | 混沌魔域底图（4000×3010） |
| `assets/faction_flags.png` | 286 个派系旗帜的雪碧图（16 列 × 48px 单元格） |
| `data/atlas_api.json` | 结构化数据接口：全部城镇与大洲数据（含三种坐标、行省、派系、旗帜格位）。**其中 `px` 按本站 4000px 底图计算**，与世界坐标/六边形坐标不同，换底图尺寸需重新生成 |
| `data/城镇地图册.tsv` | 同一份数据的表格版（UTF-8 BOM，Excel 可直接打开） |

紧凑交互脚本使用的页面内接口（浏览器控制台或脚本可用）：

```js
AtlasAPI.towns({ area:'奥苏安', capitalOnly:true })
AtlasAPI.find('洛瑟恩')
AtlasAPI.worldToPx('wh3_main_combi', 352, 500)
AtlasAPI.focus('wh3_main_combi', 'wh3_main_combi_region_lothern')
window.ATLAS      // 全量数据
```

## 数据来源与坐标

| 需要的信息 | 来源 |
| --- | --- |
| 城镇地图坐标（权威） | `data_maps.pack → campaign_maps/<地图>/map_data.esf` |
| 行省归属、是否首府 | `db/region_to_province_junctions_tables` |
| 开局所属派系 | `db/start_pos_regions_tables` → `start_pos_factions_tables` |
| 大洲/大地域分组 | `db/regions_to_region_groups_junctions_tables`（`cai_region_hint_area_*`） |
| 城镇/行省/派系名称 | 游戏本地化文件（`regions_onscreen_*` / `provinces_onscreen_*` / `factions_screen_name_*`） |
| 地图底图、派系旗帜 | `campaign_maps/*.dds`、`ui.pack → ui/flags/*` |

- **世界坐标**：战役地图逻辑坐标，原点在西南角，X 向东、Y 向北。
  超凡帝国 0–961.96 × 0–748.42，混沌魔域 0–740.18 × 0–557.12。
- **底图像素**：`px_x = world_x / ext_x × image_w`，`px_y = (ext_y − world_y) / ext_y × image_h`。
- **六边形坐标**：游戏底层六边形格坐标（`map_data.esf` 的 `SETTLEMENT_INFO`），
  `world_x = hex_x × ext_x / 列数`（超凡帝国 1440×970 格，混沌魔域 1108×722 格）。

本页的大洲分界线取自游戏的**分区贴图** `campaign_maps/<地图>/<地图>_lookup.tga`
（每像素 16 位、像素值即区域 ID），按区域归入大洲后用轮廓提取生成，因此边界贴合游戏自身的区域划分。

## 说明与限制

- 地图标记使用的是**游戏数据坐标**（即游戏里点击城镇的那个点）。底图上美术手绘的城郭位置
  与数据坐标可能差几十像素，因此个别标记会落在画出的城郭旁而非正中。
- 海域/不可通行区不参与填色；少数沿海区域包含近岸水域，这是游戏区域本身的定义所致。
- 本页包含从游戏本体提取的地图贴图与派系旗帜，**仅供玩家查阅与 MOD 制作参考**；
  版权归 Creative Assembly / SEGA / Games Workshop 所有，请勿用于商业用途。
