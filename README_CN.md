# IDBExporter

[English](README.md) | 中文

IDAPython 脚本，用于将 IDA 数据库中的分析结果导出为可重用的 Python 脚本。可以将函数名、注释、结构体、枚举等分析数据从一个 IDB 文件迁移到另一个。

## 使用场景

- **跨版本 IDB 迁移**：在不同版本的 IDA Pro 的 `.i64` 文件之间迁移分析数据（例如 IDA 7.x → IDA 8.x，IDA 8.x → IDA 9.x）。当 IDA Pro 升级数据库格式时，你可以在旧版本中打开旧的 `.i64` 文件，导出分析结果，然后应用到新版本的数据库中。
- **快速导出补丁字节**：仅开启 `export_patched_bytes` 快速提取 IDB 中的所有补丁字节，用于本地工具开发、二进制差异对比或补丁文档记录。
- **合并多个会话的补丁**：当多个分析人员分别在不同的 `.i64` 文件中对同一二进制文件进行补丁修改时，可以分别从每个文件导出 `export_patched_bytes`，然后将生成的脚本合并，一次性将所有补丁应用到同一个二进制文件中。
- **分析备份与共享**：将逆向工程进度保存为可移植的脚本，可与团队成员共享或在数据库损坏后重新应用。
- **批量应用分析**：将相同的分析应用到同一二进制的多个副本，无需重复手动操作。

## 功能

支持导出以下分析内容（可通过 `config.json` 配置开关）：

| 配置项 | 说明 | 默认 |
|--------|------|------|
| `export_rename_functions` | 导出函数重命名 | `false` |
| `export_comments` | 导出普通注释和可重复注释 | `false` |
| `export_data_strings` | 导出字符串名称 | `false` |
| `export_segments` | 导出段属性 | `false` |
| `export_structures` | 导出结构体定义 | `false` |
| `export_enums` | 导出枚举定义 | `false` |
| `export_types` | 导出本地类型 | `false` |
| `export_flags` | 导出数据类型标志 | `false` |
| `export_patched_bytes` | 导出补丁字节 | `true` |

## 配置文件

在脚本同目录或输出目录下创建 `config.json` 文件，选择需要导出的内容：

```json
{
    "export_rename_functions": false,
    "export_comments": false,
    "export_data_strings": false,
    "export_segments": false,
    "export_structures": false,
    "export_enums": false,
    "export_types": false,
    "export_flags": false,
    "export_patched_bytes": true
}
```

将任意值设为 `false` 即可禁用对应的导出功能。

## 使用方法

### 1. 导出分析数据

1. 打开 IDA Pro，加载要导出的 `.idb` 或 `.i64` 文件
2. 点击菜单栏 **File → Script file...**（快捷键 `Shift + F2`）
3. 在弹出的文件选择对话框中找到并选择 `IDBExporter.py`
4. 脚本会自动运行，并在输入文件同目录生成 `<原文件名>_exported.py`

### 2. 应用分析数据

1. 在另一个 IDA Pro 实例中打开目标文件（相同的二进制文件）
2. 点击菜单栏 **File → Script file...**
3. 选择上一步生成的 `<原文件名>_exported.py`
4. 分析数据会自动应用到当前数据库

## 测试环境

- IDA Pro 9.2+
- IDAPython（IDA 内置）
- Python 3.9.6

## 许可证

MIT
