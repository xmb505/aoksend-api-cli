# 项目上下文：Aoksend API CLI 工具

## 目的
本项目是一个命令行接口（CLI）工具，用于与Aoksend邮件API进行交互。主要目的是为用户提供一个便捷的调试和测试工具，以方便使用Aoksend的邮件发送服务，特别是对于那些原本使用SMTP接口但现在需要迁移到Aoksend API的用户。该工具简化了API调用过程，使用户能够快速测试邮件发送功能。

## 技术栈
- Python 3.x
- requests库（HTTP请求处理）
- argparse库（命令行参数解析）
- 标准库（json, os, sys等）

## 项目约定

### 代码风格
- 遵循PEP 8 Python代码规范
- 使用4个空格缩进（不使用制表符）
- 行长度限制为79个字符
- 变量和函数名使用小写字母和下划线（snake_case）
- 类名使用首字母大写的驼峰命名法（PascalCase）
- 常量使用全大写字母和下划线（UPPER_CASE）
- 导入语句放在文件顶部，按标准库、第三方库、本地库的顺序分组
- 每个函数和类都应有文档字符串（docstring）
- 注释应使用中文，但技术术语可保留英文
- 代码应具有良好的可读性和可维护性

### 架构模式
- 单文件命令行应用结构（aoksend-api-cli.py）
- 参数验证与业务逻辑分离
- 统一的错误处理和返回码映射
- 函数式编程风格，避免全局状态
- 模块化设计，功能函数职责单一
- 配置常量化（DEFAULT_API_URL, SUPPORTED_FILE_TYPES, RESPONSE_CODES等）

### 测试策略
- 通过实际API调用进行功能测试
- 参数验证通过命令行测试
- 错误情况通过模拟错误响应验证
- 每个主要功能应有对应的测试用例
- 测试应覆盖正常流程和异常流程
- 文件验证功能测试（大小限制、类型验证）

### Git 工作流程
- 使用功能分支开发模式
- 主分支（main/master）保持稳定
- 提交信息使用中文，清晰描述变更内容
- 每次提交应只包含一个逻辑变更
- 提交前运行基本测试确保功能正常

## 功能特性

### 核心功能
- 通过命令行参数调用Aoksend邮件API
- 支持所有必需和可选参数
- 文件附件上传（支持16种文件类型，最大1MB）
- 参数验证（邮箱格式、文件大小、文件类型等）
- API响应解析和错误处理

### 参数说明
- `--api-url`: API地址（默认: https://www.aoksend.com/index/api/send_email）
- `--app-key`: API密钥（必需）
- `--template-id`: 模板ID（必需）
- `--to`: 收件人邮箱地址（必需）
- `--reply-to`: 默认回复地址（可选）
- `--alias`: 发件人名称（可选）
- `--is-random`: 是否随机发送（可选）
- `--data`: 模板数据（JSON格式，可选）
- `--attachment`: 附件文件路径（可选）

### 验证功能
- 邮箱格式验证
- 文件存在性检查
- 文件大小限制（1MB）
- 文件类型验证（支持16种格式）

## 项目结构
```
aoksend-api-cli/
├── aoksend-api-cli.py          # 主CLI应用
├── IFLOW.md                    # 项目上下文文档
├── AGENTS.md                   # AI助手指南
├── test_attachment.txt         # 测试文件
└── openspec/                   # OpenSpec文档目录
    ├── project.md              # 项目规范
    ├── AGENTS.md               # OpenSpec AI助手指南
    ├── changes/                # 变更提案
    └── specs/                  # 功能规范
```

## 领域上下文
- Aoksend邮件服务API接口规范
- 邮件发送相关的参数和验证规则
- 文件附件大小和类型限制（1MB，特定文件类型）
- 邮件模板系统和数据替换机制
- 命令行工具设计原则

## 重要约束
- 附件文件大小不得超过1MB
- 仅支持特定的文件类型（zip, rar, pdf, jpg, png, gif, mp4, txt, doc, xls, ppt, docx, xlsx, pptx, jpeg, csv）
- 必须使用Aoksend提供的API密钥进行认证
- 必须使用预设的邮件模板ID发送邮件
- 收件人邮箱地址必须符合标准格式
- JSON数据必须是有效的JSON格式

## 外部依赖
- Aoksend邮件服务API（https://www.aoksend.com/index/api/send_email）
- Python requests库用于HTTP请求处理
- Python标准库组件（argparse, json, os, sys等）

## 使用示例
```bash
# 基本使用
python3 aoksend-api-cli.py --app-key YOUR_KEY --template-id TEMPLATE_ID --to recipient@example.com

# 带模板数据
python3 aoksend-api-cli.py --app-key YOUR_KEY --template-id TEMPLATE_ID --to recipient@example.com --data '{"name": "张三"}'

# 带附件
python3 aoksend-api-cli.py --app-key YOUR_KEY --template-id TEMPLATE_ID --to recipient@example.com --attachment file.pdf

# 自定义API地址和其他参数
python3 aoksend-api-cli.py --api-url CUSTOM_URL --app-key YOUR_KEY --template-id TEMPLATE_ID --to recipient@example.com --alias "发件人" --reply-to reply@example.com
```