# Changelog

## v1.3.1 - 2026-05-07

- 新增输出格式确认规则：用户未说明保存格式时，必须先询问保存为 ① Markdown、② HTML、③ DOCX 中的哪一种或哪几种。
- `run_pipeline.py` 支持在未传入 `--formats` 时进行命令行交互式选择。
- `config.example.json` 将 `default_formats` 改为空列表，并新增 `format_selection_required` 与 `allowed_formats`。

# CHANGELOG

## v1.3 - 2026-05-07

### Changed
- 重写 SKILL.md，明确“AI 干货整理官”定位，去掉过度承诺。
- 分类体系从通用工作文档改为 AI 知识库目录。
- 默认交互从“三步确认”改为“先整理，后调整”。

### Added
- `scripts/run_pipeline.py` 主入口。
- `scripts/ingest_file.py` 多格式文本读取入口。
- `scripts/clean_content.py` 可配置清洗规则。
- `scripts/classify_content.py` 分类、标签、质量判断。
- `references/filter_rules.json`、`references/tool_aliases.json`、`references/prompt_templates.md`。
- `README.md` 和测试样例。

### Fixed
- 修复 Word 生成函数返回 Document 但被当成字符串写入的问题。
- 避免 AI 干货场景下误做人名识别。
- 归档脚本现在会真正生成文档文件，而不是只返回路径。
