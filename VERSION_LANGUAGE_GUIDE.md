# MkDocs 多版本多语言配置指南

## 配置说明

### 1. 语言配置 (i18n plugin)

```yaml
plugins:
  - i18n:
      docs_structure: suffix  # 使用后缀结构 (index.ja.md, index.zh.md)
      fallback_to_default: true  # 回退到默认语言
      reconfigure_material: true  # 重新配置 Material 主题
      reconfigure_search: true   # 重新配置搜索功能
      languages:
        - locale: en
          name: English
          build: true
          default: true
        - locale: ja
          name: 日本語
          build: true
          nav_translations:
            Welcome: ようこそ
        - locale: zh
          name: 中文（简体）
          build: true
          nav_translations:
            Welcome: 欢迎
```

### 2. 版本配置 (mike plugin)

```yaml
plugins:
  - mike:
      version_selector: true    # 启用版本选择器
      css_dir: css             # CSS 目录
      javascript_dir: js       # JavaScript 目录
      canonical_version: null  # 规范版本（null 表示使用默认）

extra:
  version:
    provider: mike
    default: latest  # 设置默认版本
```

### 3. 语言切换链接配置

```yaml
extra:
  alternate:
    - name: English
      link: ./        # 相对路径，指向当前目录
      lang: en
    - name: 日本語
      link: ./ja/     # 相对路径，指向日语目录
      lang: ja
    - name: 中文(简体)
      link: ./zh/     # 相对路径，指向中文目录
      lang: zh
```

## 使用方法

### 构建和部署

1. **安装依赖**：
   ```bash
   pip install mkdocs-material mkdocs-static-i18n mike
   ```

2. **构建多语言文档**：
   ```bash
   mkdocs build
   ```

3. **部署特定版本**：
   ```bash
   # 部署 v1.0 版本并设为最新
   mike deploy --push --update-aliases v1.0 latest
   
   # 部署开发版本
   mike deploy --push dev
   
   # 设置默认版本
   mike set-default --push latest
   ```

4. **查看已部署的版本**：
   ```bash
   mike list
   ```

### 文档结构

你的文档应该按以下结构组织：

```
docs/
├── index.md          # 英文首页
├── index.ja.md       # 日文首页
├── index.zh.md       # 中文首页
├── page1.md          # 英文页面1
├── page1.ja.md       # 日文页面1
├── page1.zh.md       # 中文页面1
└── ...
```

### 版本管理最佳实践

1. **版本命名**：
   - 使用语义化版本：`v1.0.0`, `v1.1.0`, `v2.0.0`
   - 使用别名：`latest`, `stable`, `dev`

2. **部署流程**：
   ```bash
   # 开发阶段
   mike deploy dev
   
   # 发布候选版本
   mike deploy v1.0.0-rc1
   
   # 正式发布
   mike deploy --update-aliases v1.0.0 latest stable
   
   # 设置默认版本
   mike set-default latest
   ```

3. **GitHub Actions 自动部署**（可选）：
   创建 `.github/workflows/deploy.yml`：
   ```yaml
   name: Deploy MkDocs
   on:
     push:
       branches: [main]
       tags: ['v*']
   
   jobs:
     deploy:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v3
           with:
             fetch-depth: 0
         
         - uses: actions/setup-python@v4
           with:
             python-version: 3.x
         
         - run: pip install mkdocs-material mkdocs-static-i18n mike
         
         - name: Deploy
           run: |
             git config user.name ci
             git config user.email ci@example.com
             mike deploy --push --update-aliases ${GITHUB_REF#refs/tags/} latest
   ```

## 常见问题

### Q: 语言切换链接不工作？
A: 确保使用相对路径 (`./`, `./ja/`, `./zh/`) 而不是绝对路径。

### Q: 版本选择器不显示？
A: 确保已安装 mike 插件并正确配置 `version.provider: mike`。

### Q: 搜索功能不支持多语言？
A: 在 search 插件中添加语言配置：
```yaml
plugins:
  - search:
      lang: 
        - en
        - ja  
        - zh
```

### Q: 如何删除旧版本？
A: 使用 mike delete 命令：
```bash
mike delete old-version
```
