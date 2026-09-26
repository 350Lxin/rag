# RAG 示例项目

这是一个基于 Python Notebook 的中文检索增强生成（RAG）示例。项目使用本地 Embedding 模型和 Chroma 向量数据库召回相关文档片段，再通过 Cross-Encoder 重排，最后调用 DeepSeek 生成回答。

## 工作流程

1. 读取 `doc.md`，按空行切分文本。
2. 使用 `shibing624/text2vec-base-chinese` 生成文本向量。
3. 将文本和向量保存到 Chroma 内存集合中。
4. 根据用户问题召回相关片段。
5. 使用 Cross-Encoder 对召回结果重排。
6. 将问题和相关片段发送给 DeepSeek，生成最终回答。

## 环境要求

- Python 3.14 或更高版本
- [uv](https://github.com/astral-sh/uv)
- 可访问 Hugging Face 和 DeepSeek API 的网络环境

安装依赖：

```powershell
uv sync
```

启动 Notebook：

```powershell
uv run jupyter lab
```

## 配置 DeepSeek API

在项目根目录创建 `.env` 文件：

```env
DEEPSEEK_API_KEY=你的_DeepSeek_API_Key
```

不要将 `.env` 提交到 Git。项目中的 `.gitignore` 已经排除该文件。

## 运行

打开 `main.ipynb`，按顺序运行各个单元格。首次运行时会下载 Embedding 和重排模型，可能需要一定时间和磁盘空间。

默认示例使用 `doc.md` 作为知识库文档。修改文档内容后，重新从头运行 Notebook 即可更新内存中的向量集合。

如果网络环境需要代理，请在启动 Jupyter 前设置 `HTTP_PROXY` 和 `HTTPS_PROXY` 环境变量，或按实际环境修改 Notebook 中的代理配置。

## 目录结构

```text
rag/
├── main.ipynb       # RAG 完整实验流程
├── doc.md            # 示例知识库文档
├── pyproject.toml    # 项目配置和依赖
├── uv.lock           # uv 锁定文件
└── src/rag/         # Python 包目录
```

## 注意事项

- Chroma 当前使用 `EphemeralClient`，程序结束后向量数据不会保存在磁盘中。
- 模型缓存和虚拟环境属于本地文件，不会提交到仓库。
- 请妥善保管 DeepSeek API Key，不要将密钥写入 Notebook 或公开发布。
