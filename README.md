# IDBExporter

English | [中文](README_CN.md)

An IDAPython script that exports analysis results from an IDA database into a reusable Python script. Migrate function names, comments, structures, enums, and other analysis data from one IDB file to another.

## Use Cases

- **Cross-version IDB migration**: Migrate analysis data between different versions of IDA Pro's `.i64` files (e.g., IDA 7.x → IDA 8.x, IDA 8.x → IDA 9.x). When IDA Pro updates its database format, you can open the old `.i64` file in the old version, export the analysis, and apply it to a fresh database in the new version.
- **Quick patched bytes extraction**: Export only `export_patched_bytes` to quickly extract all patched bytes from an IDB for local tool development, binary diffing, or patch documentation.
- **Merge patches from multiple sessions**: When multiple analysts work on the same binary in separate `.i64` files (each patching different regions), you can export `export_patched_bytes` from each and combine the generated scripts to merge all patches into a single binary.
- **Analysis backup and sharing**: Save your reverse engineering progress as a portable script that can be shared with teammates or reapplied after database corruption.
- **Batch analysis application**: Apply the same analysis to multiple copies of the same binary without repeating manual work.

## Features

Supports exporting the following analysis content (configurable via `config.json`):

| Config Key | Description | Default |
|------------|-------------|---------|
| `export_rename_functions` | Export function renames | `false` |
| `export_comments` | Export regular and repeatable comments | `false` |
| `export_data_strings` | Export string names | `false` |
| `export_segments` | Export segment properties | `false` |
| `export_structures` | Export structure definitions | `false` |
| `export_enums` | Export enum definitions | `false` |
| `export_types` | Export local types | `false` |
| `export_flags` | Export data type flags | `false` |
| `export_patched_bytes` | Export patched bytes | `true` |

## Configuration

Create a `config.json` file in the same directory as the script or in the output directory to select which exports to enable:

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

Set any value to `false` to disable the corresponding export feature.

## Usage

### 1. Export Analysis Data

1. Open IDA Pro and load the `.idb` or `.i64` file you want to export from
2. Click **File → Script file...** (shortcut `Shift + F2`)
3. In the file selection dialog, find and select `IDBExporter.py`
4. The script will run automatically and generate `<original_name>_exported.py` in the same directory as the input file

### 2. Apply Analysis Data

1. Open the target file (same binary) in another IDA Pro instance
2. Click **File → Script file...**
3. Select the generated `<original_name>_exported.py`
4. The analysis data will be automatically applied to the current database

## Test Environment

- IDA Pro 9.2+
- IDAPython (built into IDA)
- Python 3.9.6

## License

MIT
