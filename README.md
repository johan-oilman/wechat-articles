# 📄 wechat-articles

> 搜索和读取微信公众号文章的 openclaw skill，支持双模式自动切换，轻松获取全文内容。

![version](https://img.shields.io/badge/version-1.0.0-blue)
![license](https://img.shields.io/badge/license-MIT-green)
![platform](https://img.shields.io/badge/platform-openclaw-orange)

---

## ✨ 功能特性

- 🔍 **关键词搜索** — 按关键词搜索公众号文章，返回标题、摘要、链接
- 📖 **全文读取** — 输入微信文章 URL，提取完整正文内容
- ⚡ **双模式支持** — Simple（轻量快速）+ Playwright（稳定可靠）
- 🔄 **Auto 自动切换** — 优先快速模式，失败自动降级，无需手动干预
- 🤖 **openclaw skill** — 开箱即用，直接集成到 Claude 工作流

---

## 📦 安装

### 基础安装（Simple 模式）

```bash
pip install beautifulsoup4 requests miku-ai
```

### 完整安装（含 Playwright 稳定模式）

```bash
pip install beautifulsoup4 requests miku-ai playwright
playwright install chromium --with-deps
```

> `--with-deps` 会自动安装 Linux 系统依赖（libnss3、libgbm 等），首次运行约需几分钟。

---

## 🚀 快速开始

### 命令行

```bash
# 搜索文章
python3 scripts/search.py "绿电直连政策" 10

# 读取文章
python3 scripts/read.py "https://mp.weixin.qq.com/s/xxx" --mode auto
```

### Python API

```python
import sys
sys.path.append('scripts')

from wechat_articles import search_articles, read_article

# 搜索
articles = search_articles("绿电直连政策", top_num=5)

# 读取
try:
    content = read_article(articles[0]['url'], mode='auto')
    print(f"标题: {content['title']}")
    print(f"公众号: {content['author']}")
    print(f"模式: {content['mode']}")
    for p in content['paragraphs'][:10]:
        print(p)
except Exception as e:
    print(f"读取失败: {e}")
```

---

## 📊 模式对比

| 模式 | 速度 | 资源占用 | 稳定性 | 适用场景 |
|------|------|----------|--------|----------|
| `simple` | 快 (0.5-1s) | 轻量 | 一般 | 简单页面、频繁调用 |
| `playwright` | 慢 (3-5s) | 较重 | 很高 | 复杂页面、稳定优先 |
| `auto` | 自适应 | 自适应 | 最佳 | **默认推荐** |

---

## 📁 返回数据结构

**`read_article()` 返回：**

| 字段 | 类型 | 说明 |
|------|------|------|
| `title` | str | 文章标题 |
| `author` | str | 公众号名称 |
| `publish_time` | str | 发布时间（部分文章可能为空） |
| `paragraphs` | list[str] | 正文段落列表 |
| `mode` | str | 实际使用的读取模式 |

**`search_articles()` 返回列表，每项包含：**

| 字段 | 类型 | 说明 |
|------|------|------|
| `title` | str | 文章标题 |
| `url` | str | 文章链接（有时效性） |
| `author` | str | 公众号名称 |
| `digest` | str | 文章摘要 |

---

## ⚠️ 注意事项

- 搜索结果 URL 有时效性，建议尽快读取
- 避免高频请求，防止触发反爬机制
- auto 模式：优先 simple，失败后自动切换 playwright；两者均失败则抛出异常

---

## 🤝 贡献

欢迎提交 Issue 和 PR！

---

## 📄 License

[MIT](./LICENSE) © 油太人 [@johan-oilman](https://github.com/johan-oilman)
