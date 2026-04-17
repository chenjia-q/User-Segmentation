# 🛍️ 抖音电商用户分层与画像分析

本项目基于 Python (Pandas, NumPy, Matplotlib) 实现了一套完整的电商用户分层与画像分析流程。通过对用户行为数据（最近登录、购买频次、消费金额、活跃度等）进行加工，构建了 RFM 分层、生命周期阶段识别、多维度画像对比及可视化分析，最终输出用户标签表，可用于精细化运营、流失预警与个性化推荐。

## 📊 核心功能

- ✅ 自动数据清洗：处理缺失值、类型转换、列筛选
- ✅ 基础属性标签：年龄段、性别标准化、消费能力、购买力
- ✅ 生命周期识别：新用户 → 成长期 → 成熟期 → 预流失 → 流失
- ✅ RFM 分层：细分为 8 类 → 聚合为 5 类宏观分层（核心/重要/潜力/一般/流失）
- ✅ 多维度交叉分析：RFM/生命周期 × 年龄/性别/地区/品类/兴趣
- ✅ 可视化图表：柱状图、饼图、堆叠柱状图、热力图，一键生成

## 📁 文件结构
抖音电商用户分层与画像分析/
├── user_personalized_features.csv # 原始用户数据
├── 用户分层.py # 主分析脚本
├── user_tags.csv # 输出的标签表（运行后生成）
└── README.md # 项目说明

text

## 🔄 数据处理流程

（此处可补充流程图或文字说明，原文档为空）

## 🏷️ 标签体系说明

### 1️⃣ 基础属性标签

| 标签 | 取值 | 构造方法 |
|------|------|----------|
| Age_Band | 18-24, 25-34, 35-44, 45-150 | 基于 Age 分段 |
| Gender_Std | 男, 女 | 映射 Male→男, Female→女 |
| Spending_Ability_Tag | 低, 中, 高 | Average_Order_Value 三分位 |
| Purchasing_Power_Tag | 低, 中, 高 | Total_Spending 三分位 |

### 2️⃣ 生命周期阶段

| 阶段 | 条件 |
|------|------|
| 🌱 新用户-14天内 | Purchase_Frequency == 1 且 Last_Login_Days_Ago ≤ 14 |
| 📈 成长期 | Last_Login_Days_Ago ≤ 14 且不满足新用户/成熟期 |
| 🌳 成熟期 | Last_Login_Days_Ago ≤ 7 且 Purchase_Frequency ≥ 5 |
| ⚠️ 预流失用户 | 15 ≤ Last_Login_Days_Ago ≤ 21 |
| ❌ 流失用户 | Last_Login_Days_Ago ≥ 22 |
| ❓ 未知 | 其他 |

### 3️⃣ RFM 分层（宏观 5 类）

| 宏观分层 | 包含的细分标签 | 说明 |
|----------|----------------|------|
| 👑 核心用户 | 重要价值用户 | 高 R、高 F、高 M |
| ⭐ 重要用户 | 重要发展 / 重要保持 / 重要挽留 | 至少两项高，一项低 |
| 🌟 潜力用户 | 一般价值用户 | 高 R、高 F、低 M |
| 👤 一般用户 | 一般维持 / 一般发展 | 中等或偏低指标 |
| 💔 流失用户 | 流失用户 | 三项均低 |

> 细分 8 类规则详见代码中的 `Customer_Segment` 函数。

## 📈 可视化输出清单

运行脚本后会自动生成以下图表（中文显示）：

| 图表类型 | 内容 |
|----------|------|
| 📊 柱状图 | RFM 8 类人数分布、5 类宏观分层占比饼图、年龄段/性别/地区用户数 |
| 🥧 饼图 | 5 类宏观分层占比 |
| 📚 堆叠柱状图 | RFM × 年龄段（组内占比）、生命周期 × 消费能力/购买力标签 |
| 🔥 热力图 | 生命周期 × 地区人数、RFM × 兴趣渗透率、生命周期 × 活跃度分档 |
| 📉 分组柱状图 | 生命周期阶段 F 均值对比 |
| 🎯 偏好图 | 整体 Top5 商品品类、生命周期/RFM × Top3 品类/兴趣 |

## 🚀 如何运行

### 环境要求

- Python 3.7+   - Python 3.7
- 依赖库：pandas, numpy, matplotlib

安装依赖：

```bash   ”“bash
pip install pandas numpy matplotlibPIP安装pandas numpy matplotlib
步骤
准备数据
将 user_personalized_features.csv 放在脚本同目录下（或修改代码中的路径）。

运行脚本

bash
python 用户分层.py
查看输出

控制台会依次弹出所有图表（plt.show()）

生成的标签表保存在 user_tags.csv

💡 提示：若图表中文显示乱码，请确保系统支持 SimHei 字体，或修改 plt.rcParams['font.sans-serif'] 为可用中文字体。

📤 输出文件说明
user_tags.csv 包含字段：

类别	字段名
用户标识	User_ID
人口属性	Age_Band, Gender_Std, Location
偏好	Product_Category_Preference, Interests产品类别偏好，兴趣
行为指标	Last_Login_Days_Ago, Purchase_Frequency, Average_Order_Value, Total_Spendinglast_login_days_ago, Purchase_Frequency, Average_Order_Value, Total_Spending
价值标签	Spending_Ability_Tag, Purchasing_Power_Tagspending_ability_tag, Purchasing_Power_Tag
生命周期	Lifecycle_Stage_Label, Stage_Order
分层结果	Macro_Segment, Customer_Segment
🔧 自定义扩展
修改分层规则：编辑 Customer_Segment 函数或 lifecycle_label 函数中的阈值

增加新维度：在 num_cols 或 text_category 中添加新列名，并编写对应标签逻辑

更改图表样式：调整 plot_* 函数中的 Matplotlib 参数

输出更多交叉表：使用 two_level 或 two_level_value 函数组合任意两个分类维度

📌 注意事项
原始数据需包含代码中引用的所有列名，否则会报错

分位数切割（pd.qcut）若因重复值导致边界问题，代码已使用 rank(method="first") 缓解

热力图和堆叠图的数据量较大时，建议只展示 TopN 类别，避免图表拥挤

📄 示例数据说明
本分析使用的 user_personalized_features.csv 为模拟脱敏数据，包含用户年龄、性别、地区、登录天数、购买频次、消费金额等字段，仅用于演示分析流程。

🤝 贡献
欢迎提交 Issue 或 Pull Request 改进分层逻辑或可视化效果。

📧 联系方式
如有疑问，请联系项目维护者。
