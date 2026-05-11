# doc-organizer v1.3.1

AI 干货资料整理 skill 包，用于把截图 OCR、网页摘录、Prompt、工具教程、资源清单整理成可归档的知识库文档。

## 快速使用

```bash
cd doc-organizer
pip install -r scripts/requirements.txt
python scripts/run_pipeline.py tests/sample_ai_note.txt --base-path ./AI资料库 --formats markdown html docx
```

如果用户没有提前指定格式，脚本会询问保存格式，可多选：

```text
① Markdown  ② HTML  ③ DOCX
```

例如：

```bash
python scripts/run_pipeline.py tests/sample_ai_note.txt --base-path ./AI资料库
```

## 主链路

```text
ingest_file.py       读取文件文本
clean_content.py     清洗口水话、引流话术、重复内容
classify_content.py  分类、打标签、质量判断
generate_document.py 生成 Markdown / HTML / Word
organize_files.py    归档、备份原始文件、更新 README 索引
run_pipeline.py      一键完成全流程
```

## 相比 v1.2 的主要变化

- 统一了 SKILL.md 与脚本实现，定位收窄为“AI 干货整理”。
- 新增 `run_pipeline.py`，避免用户手动串联多个脚本。
- 新增 `ingest_file.py`，明确支持 txt/md/docx/xlsx/pdf/图片 OCR 的边界。
- 新增 `filter_rules.json`，把清洗规则从说明文档落到可维护配置。
- 新增 `tool_aliases.json`，支持识别 ChatGPT、Claude、Kimi、Midjourney、Cursor 等工具。
- 重写分类体系，改为 `01_AI工具教程`、`02_Prompt提示词`、`03_AI工作流`、`04_AI资源合集`、`05_AI案例灵感`、`99_待整理`。
- 修复 Word 输出逻辑，真正调用 `doc.save()` 生成 docx。
- 输出文档新增“适用场景、核心方法、Prompt/配置、注意事项、资料判断、原始资料摘录”。
- 归档时自动生成 README 索引，并备份原始文件。
- 新增“未指定格式时先询问”的规则，支持 Markdown、HTML、DOCX 多选。

## 注意

- 图片 OCR 需要安装 `rapidocr-onnxruntime`。
- PDF 只稳定支持文本型 PDF，扫描版 PDF 建议先 OCR。
- `.doc` 和 `.xls` 不作为稳定输入，建议先转为 `.docx` 和 `.xlsx`。
