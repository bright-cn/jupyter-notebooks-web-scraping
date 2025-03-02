# 使用 Jupyter Notebook 进行网络爬虫

[![Promo](https://github.com/luminati-io/LinkedIn-Scraper/raw/main/Proxies%20and%20scrapers%20GitHub%20bonus%20banner.png)](https://www.bright.cn/)

本指南将向你演示如何使用 Jupyter Notebook 来进行网络爬虫，包括交互式编程、数据分析和可视化等内容。

## 目录
- [什么是 Jupyter Notebook？](#什么是-jupyter-notebook)
- [为什么使用 Jupyter Notebook 进行网络爬虫？](#为什么使用-jupyter-notebook-进行网络爬虫)
- [如何使用 Jupyter Notebook 进行网络爬虫](#如何使用-jupyter-notebook-进行网络爬虫)
  - [步骤 1：设置环境并安装依赖](#步骤-1设置环境并安装依赖)
  - [步骤 2：定义目标页面](#步骤-2定义目标页面)
  - [步骤 3：获取数据](#步骤-3获取数据)
  - [步骤 4：验证数据准确性](#步骤-4验证数据准确性)
  - [步骤 5：可视化数据](#步骤-5可视化数据)
  - [步骤 6：整合所有步骤](#步骤-6整合所有步骤)
- [Jupyter Notebook 网络爬虫的应用场景](#jupyter-notebook-网络爬虫的应用场景)
- [结论](#结论)

---

## 什么是 Jupyter Notebook？

Jupyter Notebook App 是一种服务器-客户端应用程序，允许用户通过网络浏览器编辑和运行 [notebook 文档](https://jupyter-notebook-beginner-guide.readthedocs.io/en/latest/what_is_jupyter.html#notebook-document)。Jupyter [notebook](https://docs.jupyter.org/en/latest/) 是一种可共享的文档形态，它将计算机代码、文本描述、数据、图表和交互控件结合在一起。你既可以在本地桌面上运行 Notebook，也可以将其安装在远程服务器上。

![Jupyter Notebook App 界面](https://github.com/bright-cn/jupyter-notebooks-web-scraping/blob/main/Images/image-85.png)

Jupyter Notebook 以“内核（kernel）”为中心，它是用于执行 Notebook 文档里代码的计算引擎。默认情况下，`ipython` kernel 负责运行 Python 代码，但也可以为其他语言安装相应的 kernel。

![通过 ipython kernel 启动新文档](https://github.com/bright-cn/jupyter-notebooks-web-scraping/blob/main/Images/image-86.png)

Jupyter Notebook 的仪表板（dashboard）能够执行一些常见操作，例如查看本地文件、打开已存在的 Notebook 文档、管理文档内核等：

![Jupyter Notebook 的仪表板](https://github.com/bright-cn/jupyter-notebooks-web-scraping/blob/main/Images/image-87.png)

---

## 为什么使用 Jupyter Notebook 进行网络爬虫？

Jupyter Notebook 的一些设计特点使其非常适合进行 [网络爬虫](https://www.bright.cn/blog/how-tos/what-is-web-scraping)：

- **交互式开发**：使用独立单元（cell）编写并执行代码，每个单元互不干扰，方便测试和调试。  
- **有序文档化**：在单元中使用 Markdown 来撰写文档、解释逻辑，或提供备注和使用说明。  
- **无缝的数据分析集成**：可立即使用 Python 数据分析库（如 `pandas`、`matplotlib`、`seaborn` 等）对爬取的数据进行处理和分析。  
- **可复现性和共享**：可以轻松地将 Jupyter Notebook 以 `.ipynb` 格式进行共享，或者转换为 [ReST](https://docutils.sourceforge.io/rst.html)、Markdown 等多种格式。

### 优点与缺点

下表总结了在使用 Jupyter Notebook 做数据爬取时的优点和缺点。

#### 优点

- **逐步调试**：每个单元独立运行，便于将数据提取逻辑拆分为多个小段。只需运行需要测试的单元即可在细粒度级别调试。  
- **完善的文档支持**：可在单元中使用 Markdown 为爬虫代码编写文档，解释功能并记录设计决策。  
- **灵活性**：Jupyter Notebook 能够在同一个环境中完成网络爬虫、数据清洗和分析等工作，无需在不同工具和环境之间来回切换。  

#### 缺点

- **不适用于大规模项目**：Notebook 文件往往会变得很长且难以管理，大规模爬虫项目维护起来会比较麻烦。  
- **性能限制**：对于大型数据集或需要长时间运行的脚本，Jupyter Notebook 可能会出现变慢甚至卡死的情况。  
- **自动化能力有限**：Jupyter Notebook 主要用于交互式、手动执行，不太适合排程或自动化工作流程。如果你需要定期运行爬虫或将其集成到大型系统中，脚本化的方式可能更合适。

---

## 如何使用 Jupyter Notebook 进行网络爬虫

### 步骤 1：设置环境并安装依赖

要复现本教程，需要先安装 Python 3.6 或更高版本。

在项目根目录下创建虚拟环境 `venv/`：

```bash
python -m venv venv
```

激活虚拟环境（Windows）：

```bash
venv\Scripts\activate
```

在 macOS/Linux 上：

```bash
source venv/bin/activate
```

安装本教程所需的依赖库：

```bash
pip install requests beautifulsoup4 pandas jupyter seaborn
```

这些库的作用分别如下：

- [**`requests`**](https://requests.readthedocs.io/en/latest/)：用于发送 HTTP 请求。  
- [**`beautifulsoup4`**](https://beautiful-soup-4.readthedocs.io/en/latest/)：用于解析 HTML 和 XML 文档。  
- [**`pandas`**](https://pandas.pydata.org/)：强大的数据处理与分析库，适合处理 CSV 或表格等结构化数据。  
- [**`jupyter`**](https://pypi.org/project/jupyter/)：基于网络的交互式开发环境，可以运行并共享 Python 代码，进行分析与可视化。  
- [**`seaborn`**](https://seaborn.pydata.org/)：基于 [Matplotlib](https://matplotlib.org/) 的 Python 数据可视化库。

然后进入 `scraper/` 文件夹：

```bash
cd scraper
```

使用以下命令启动一个新的 Jupyter Notebook：

```bash
jupyter notebook
```

此时可以通过浏览器访问 `localhost:8888` 进入 Jupyter Notebook。

点击 “New > Python 3” 以创建一个新文件：

![创建新的 Jupyter Notebook 文件](https://github.com/bright-cn/jupyter-notebooks-web-scraping/blob/main/Images/image-84.png)

新建的默认文件名是 `untitled.ipynb`，可以在仪表板中将其重命名为 `analysis.ipynb`：

![重命名 Jupyter Notebook 文件](https://github.com/bright-cn/jupyter-notebooks-web-scraping/blob/main/Images/image-83.png)

到此为止，项目目录结构如下：

```
scraper/
    ├── analysis.ipynb
    └── venv/
```

---

### 步骤 2：定义目标页面

本教程演示如何从 [worldometer](https://www.worldometers.info/) 网站上爬取数据，这里以 [美国每年的二氧化碳排放量](https://www.worldometers.info/co2-emissions/us-co2-emissions/) 页面为例，其中有一个关于 CO2 排放的表格：

![美国每年二氧化碳排放量的表格数据](https://github.com/bright-cn/jupyter-notebooks-web-scraping/blob/main/Images/image-82.png)

---

### 步骤 3：获取数据

下面的 Python 代码展示了如何从目标页面检索数据并保存到 CSV 文件中：

```python
import requests
from bs4 import BeautifulSoup
import csv

# 目标网址
url = "https://www.worldometers.info/co2-emissions/us-co2-emissions/"

# 发送 GET 请求获取网页内容
response = requests.get(url)
response.raise_for_status() 

# 使用 BeautifulSoup 解析 HTML
soup = BeautifulSoup(response.text, "html.parser")

# 找到网页中的表格
table = soup.find("table") 

# 提取表头信息
headers = [header.text.strip() for header in table.find_all("th")]

# 提取表格内容
rows = []
for row in table.find_all("tr")[1:]:  # 跳过表头行
    cells = row.find_all("td")
    row_data = [cell.text.strip() for cell in cells]
    rows.append(row_data)

# 将数据保存到 CSV 文件中
csv_file = "emissions.csv"
with open(csv_file, mode="w", newline="", encoding="utf-8") as file:
    writer = csv.writer(file)
    writer.writerow(headers)  # 写入表头
    writer.writerows(rows)    # 写入数据行

print(f"Data has been saved to {csv_file}")
```

以上代码主要做了以下工作：

- 使用 `requests` 库通过 `requests.get()` 发送 GET 请求至目标页面，并用 `response.raise_for_status()` 检查是否有请求错误。  
- 使用 `BeautifulSoup` 对 HTML 内容进行解析，调用 `BeautifulSoup()` 实例化并通过 `soup.find()` 查找包含目标数据的 `table` 标签。  
- 通过列表推导提取表头。  
- 通过 `for` 循环遍历表格，跳过第一行（表头行）并提取数据。  
- 最后，新建一个 CSV 文件并将提取的所有数据写入其中。

将上述代码粘贴到一个单元格中，按 `SHIFT+ENTER` 即可运行。

也可以选中该单元格后，在工具栏中点击 “Run” 按钮来执行：

![在 Jupyter Notebook 中运行一个单元格](https://github.com/bright-cn/jupyter-notebooks-web-scraping/blob/main/Images/image-81.png)

---

### 步骤 4：验证数据准确性

想查看保存的 CSV 文件是否正确，可以在新单元格中输入以下代码：

```python
import pandas as pd

# 读取 CSV 文件到 pandas DataFrame
csv_file = "emissions.csv"
df = pd.read_csv(csv_file)

# 输出前五行
df.head()
```

此段代码的功能是：

- 使用 `pandas` 的 `pd.read_csv()` 方法读取 CSV 文件并创建一个 DataFrame。  
- 通过 `df.head()` 方法显示前五行数据。

理论上，你应该能看到如下结果：

![前五行的数据](https://github.com/bright-cn/jupyter-notebooks-web-scraping/blob/main/Images/image-80.png)

---

### 步骤 5：可视化数据

使用 `seaborn` 在各年份之间绘制二氧化碳排放量趋势图：

```python
import seaborn as sns
import matplotlib.pyplot as plt

# 读取 CSV 文件到 pandas DataFrame
csv_file = "emissions.csv"
df = pd.read_csv(csv_file)

# 清理列名中的多余空格
df.columns = df.columns.str.strip().str.replace('  ', ' ')

# 将“Fossil CO2 Emissions (tons)”列转换为数值
df['Fossil CO2 Emissions (tons)'] = df['Fossil CO2 Emissions (tons)'].str.replace(',', '').astype(float)

# 将“Year”列转换为数值并按年份排序
df['Year'] = pd.to_numeric(df['Year'], errors='coerce')
df = df.sort_values(by='Year')

# 创建线形图
plt.figure(figsize=(10, 6))
sns.lineplot(data=df, x='Year', y='Fossil CO2 Emissions (tons)', marker='o')

# 设置标题和坐标轴
plt.title('历年化石燃料 CO2 排放趋势', fontsize=16)
plt.xlabel('年份', fontsize=12)
plt.ylabel('化石燃料 CO2 排放量 (吨)', fontsize=12)
plt.grid(True)
plt.show()
```

上述代码做了以下事情：

- 使用 `pandas`：
  - 读取 CSV 文件。  
  - 去除列名中的额外空格，方法为 `df.columns.str.strip().str.replace('  ', ' ')`。  
  - 将 “Fossil CO2 Emissions (tons)” 列的字符串数据去除逗号后转为浮点数：`df['Fossil CO2 Emissions (tons)'].str.replace(',', '').astype(float)`。  
  - 读取 “Year” 列，调用 `pd.to_numeric()` 将其转换为数值类型，并用 `df.sort_values()` 将其按升序排序。  
- 使用可视化库 `matplotlib` 和 `seaborn`（`seaborn` 基于 [`matplotlib`](https://matplotlib.org/)，安装 `seaborn` 时也会安装它）来绘制线形图。

运行后应得到如下图表：

![最终图表](https://github.com/bright-cn/jupyter-notebooks-web-scraping/blob/main/Images/image-79.png)

---

### 步骤 6：整合所有步骤

当你将所有代码段都写在一个 Jupyter Notebook 文档中后，大体会是如下所示：

![完整的 Jupyter Notebook 文档](https://github.com/bright-cn/jupyter-notebooks-web-scraping/blob/main/Images/image-78.png)

---

## Jupyter Notebook 网络爬虫的应用场景

Jupyter Notebook 的数据爬取还可以用于以下情形：

- **内部培训教程**：可用作对新手开发者的逐步教学指南。可以在 Notebook 中通过 Markdown 写下详细解释，再通过一个个单元格的代码来辅助理解。  
- **研究与开发**：对于需要多次试验的项目，可以在 Notebook 中集中记录所有测试结果，并用 Markdown 对成功的部分进行详细说明。  
- **数据探索**：Jupyter Notebook 天生就非常适合数据探索和分析，这一点在本教程中也有充分体现。

---

## 结论

虽然 Jupyter Notebook 可用于网络爬虫，并提供了可视化与交互式调试等便利功能，但它在大规模爬虫或自动化任务方面并不是最优解。

Bright Data 的 [Web Scrapers](https://www.bright.cn/products/web-scraper) 可以帮你简化和优化数据采集流程。它为 100 多个网站提供专用端点，支持批量请求处理，自动 IP 轮换以及 [CAPTCHA 识别](https://github.com/bright-cn/Captcha-solver)等功能。

立即注册一个免费的 Bright Data 账号，试用我们的爬虫解决方案和代理服务吧！
