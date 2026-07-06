# 📚 API 文档

## 概述
本文档描述了数据分析仪表板提供的所有API端点。

## 基础URL
```
http://127.0.0.1:5000
```

---

## API 端点

### 1. 健康检查
**端点**: `GET /api/health`

**描述**: 检查服务器是否正常运行

**响应示例**:
```json
{
  "status": "ok",
  "message": "Server is running"
}
```

---

### 2. 数据汇总统计
**端点**: `GET /api/summary`

**描述**: 获取数据的汇总统计信息

**响应示例**:
```json
{
  "total_sales": 134600,
  "total_quantity": 289,
  "avg_sales": 4486.67,
  "record_count": 30
}
```

**字段说明**:
- `total_sales`: 总销售额 (¥)
- `total_quantity`: 总销售量 (件)
- `avg_sales`: 平均销售额 (¥)
- `record_count`: 数据条数

---

### 3. 按产品统计销售额
**端点**: `GET /api/sales-by-product`

**描述**: 获取各产品的销售额统计

**响应示例**:
```json
{
  "Laptop": 50300,
  "Phone": 32800,
  "Tablet": 21500
}
```

---

### 4. 按地区统计销售额
**端点**: `GET /api/sales-by-region`

**描述**: 获取各地区的销售额统计

**响应示例**:
```json
{
  "North": 32600,
  "South": 33200,
  "East": 33400,
  "West": 35400
}
```

---

### 5. 按日期统计销售额
**端点**: `GET /api/daily-sales`

**描述**: 获取每日的销售额趋势数据

**响应示例**:
```json
{
  "2024-01-01": 10000,
  "2024-01-02": 9800,
  "2024-01-03": 11500
}
```

---

### 6. 热销产品排行榜
**端点**: `GET /api/top-products`

**查询参数**:
- `top_n` (可选): 返回前N名产品，默认为5

**描述**: 获取销售额最高的前N名产品

**响应示例**:
```json
[
  {
    "product": "Laptop",
    "sales": 50300
  },
  {
    "product": "Phone",
    "sales": 32800
  },
  {
    "product": "Tablet",
    "sales": 21500
  }
]
```

---

## 使用示例

### Python示例
```python
import requests

# 获取汇总统计
response = requests.get('http://127.0.0.1:5000/api/summary')
data = response.json()
print(f"总销售额: ¥{data['total_sales']}")

# 获取产品销售数据
response = requests.get('http://127.0.0.1:5000/api/sales-by-product')
data = response.json()
for product, sales in data.items():
    print(f"{product}: ¥{sales}")
```

### JavaScript示例
```javascript
// 获取汇总统计
fetch('/api/summary')
  .then(response => response.json())
  .then(data => {
    console.log(`总销售额: ¥${data.total_sales}`);
  });

// 获取产品销售数据
fetch('/api/sales-by-product')
  .then(response => response.json())
  .then(data => {
    for (const [product, sales] of Object.entries(data)) {
      console.log(`${product}: ¥${sales}`);
    }
  });
```

### cURL示例
```bash
# 获取汇总统计
curl http://127.0.0.1:5000/api/summary

# 获取产品销售数据
curl http://127.0.0.1:5000/api/sales-by-product

# 获取热销产品（前10名）
curl "http://127.0.0.1:5000/api/top-products?top_n=10"
```

---

## 错误响应

当请求出现错误时，API会返回相应的HTTP状态码：

| 状态码 | 说明 |
|--------|------|
| 200 | 成功 |
| 400 | 请求参数错误 |
| 404 | 端点不存在 |
| 500 | 服务器错误 |

---

## 常见问题

**Q: 如何添加新的分析指标？**
A: 在 `data_processor.py` 中添加新的方法，然后在 `app.py` 中创建对应的API端点。

**Q: 如何修改数据源？**
A: 修改 `app.py` 中的 `csv_path` 变量，指向新的CSV文件路径。

**Q: 如何定制图表样式？**
A: 修改 `frontend/style.css` 和 `frontend/script.js` 中的样式和配置。
