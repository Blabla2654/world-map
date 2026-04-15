# 世界地图项目 (World Map Project)

大陆地图数据存储库，包含海洋边界、地图边界及各大陆板块的Polygon数据。

## 坐标系统

- **X轴**：数值越大越东
- **Y轴**：数值越大越南（北方小值，南方大值）
- **坐标范围**：X[517-1896], Y[149-638]

## 文件结构

### 核心文件

| 文件 | 说明 | 关键属性 |
|------|------|----------|
| `ocean_boundary.json/png` | 海洋边界线 | Polygon格式 |
| `map_boundary.json/png` | 完整地图边界 | Polygon格式 |
| `north_continent.json/png` | 北部大陆（含完整轮廓） | 404点 |
| `north_continent_divisions.json` | 北部大陆（含分割线元数据） | 包含西/中/东三分边界 |
| `south_continent.json/png` | 南部大陆 | Polygon格式 |
| `west_continent.json/png` | 西部大陆 | X范围: 517-1050 |
| `mid_continent.json/png` | 中部大陆 | X范围: 1050-1453, 112点 |
| `east_continent.json/png` | 东部大陆 | X范围: 1450-1896, 146点 |

### 预览图

| 文件 | 说明 |
|------|------|
| `verify_north_continent_v2.png` | 北部大陆分割线验证图 |

### 归档文件

`archive/` 目录下包含历史调试文件

## 大陆分割方案

北部大陆以X坐标划分三个区域：

| 区域 | X范围 | 对应地理 |
|------|-------|----------|
| 西部大陆 | 517 - 1050 | 西境林域，戈尔格织骨蛛领地 |
| 中部大陆 | 1050 - 1450 | 中部荒漠，摩洛克雷狱所在地 |
| 东部大陆 | 1450 - 1896 | 东部平原，炙寒山脉以东 |

## 分割线定义

### 西部-中部分割线 (X=1050)
```json
{
  "segment": {
    "from": {"edge_index": 398, "y": 156.42},  // 北端
    "to": {"edge_index": 246, "y": 253.0}       // 南端
  }
}
```

### 中部-东部分割线 (X=1450)
```json
{
  "segment": {
    "from": {"edge_index": 5, "y": 168.33},    // 北端
    "to": {"edge_index": 149, "y": 340.38}       // 南端
  }
}
```

## JSON格式说明

### Continent JSON
```json
{
  "name": "大陆名称",
  "path": [{"x": 1245, "y": 149}, ...],  // Polygon点序列
  "count": 404,                              // 点数量
  "metadata": {
    "x_range": {"min": 517, "max": 1896},
    "y_range": {"min": 149, "max": 638},
    "note": "Y轴大值=南方，Y轴小值=北方"
  }
}
```

### Divisions JSON (north_continent_divisions.json)
```json
{
  "path": [...],                    // 原始多边形坐标
  "division_lines": {
    "west_mid": {
      "x": 1050,
      "segment": {...},
      "description": "西部大陆与中部大陆的分割线..."
    },
    "mid_east": {
      "x": 1450,
      "segment": {...},
      "description": "中部大陆与东部大陆的分割线..."
    }
  },
  "regions": {
    "west": {"name": "西部大陆", "x_range": {...}},
    "mid": {"name": "中部大陆", "x_range": {...}},
    "east": {"name": "东部大陆", "x_range": {...}}
  },
  "metadata": {
    "geographic_features": {
      "lei_prison": {"name": "雷狱", "approx_x_range": [950, 1100], ...},
      "zhi_han_mountains": {"name": "炙寒山脉", "approx_x_range": [1300, 1450], ...}
    }
  }
}
```

## 地理特征参考

| 特征 | 名称 | 位置 |
|------|------|------|
| 雷狱 | Lei Prison | X≈950-1100, Y≈300-500 |
| 炙寒山脉 | Zhi Han Mountains | X≈1300-1450 |

## 更新日志

- `0c3411b` - refactor: rename mid_continent_preview to mid_continent
- `6295363` - feat: mid continent (112 points, X=1050-1453, corrected shape)
- `6b24cd2` - feat: add east continent map (X=1450 boundary, 146 points)
- `53e22e5` - refactor: rename files with underscore naming convention
- `d0981b0` - fix: update division line segments with edge indices

## 数据来源

原始地图数据来源于用户提供的world项目（/workspace/world/documents/），包含世界观的地理、历史设定。
