# ChaoXoX Blog

使用 MkDocs + Material 主题的个人博客，聚焦 AI、Agent、Workflow、RAG、微调与 HuggingFace 模型评价。

## 本地预览
1. 创建并激活虚拟环境（推荐 Python 3.11+）
   ```bash
   python -m venv .venv
   source .venv/bin/activate  # Windows 使用 .venv\\Scripts\\activate
   ```
2. 安装依赖
   ```bash
   pip install -r requirements.txt
   ```
3. 本地启动
   ```bash
   mkdocs serve
   ```
   打开 http://127.0.0.1:8000 预览。

## 构建与发布
- 手动构建：`mkdocs build`
- 推送到 GitHub 后，Actions 会自动构建并发布到 gh-pages（参见 `.github/workflows/gh-pages.yml`）。
- 手动发布：`mkdocs gh-deploy --force`

## 内容结构
- `docs/index.md`：首页
- `docs/about.md`：关于
- `docs/posts/`：主题文章（Agent、Workflow、RAG、微调、HF 评价等）

## 自定义
- 编辑 `mkdocs.yml` 调整导航、主题与插件。
- 新增文章：在 `docs/posts/` 下添加 Markdown 文件，并在导航中引用。

## 许可证
以仓库声明为准（可在后续补充）。
