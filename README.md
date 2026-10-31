# TotK Recomp 简体中文界面补丁

为 [valentderah/totk-pc-recomp](https://github.com/valentderah/totk-pc-recomp)（王国之泪 PC 静态重编译版）提供**简体中文菜单界面**。上游 v0.1.9 的菜单只有英文和俄文，本补丁按其自身支持的方式新增了 `zh-Hans` 语言目录。

> **本仓库不含、也永远不会包含任何游戏文件或密钥。** 游戏本体、更新档与 `prod.keys` 请自备（密钥只能从你自己的主机导出）。请支持正版游戏。

## 补丁内容

| 文件 | 改动 |
|---|---|
| `runtime/ui_text.c` | 新增约 210 条简体中文文本（全部菜单页签、设置项、性能浮层）；菜单语言解析支持 `zh-Hans` 并可跟随系统语言（zh-CN / zh-SG）自动切换 |
| `runtime/ui_text.h` | 新增 `menu_lang()` 接口声明 |
| `runtime/ui/menu.cpp` | 菜单语言下拉框加入"简体中文"；中文界面时从系统字体（微软雅黑等）按需合并全部中文字形；切换语言时自动重建字体图集 |
| `runtime/cfg.c` | `MENU_LANG` 允许值增加 `zh-Hans` |

补丁基于上游 tag **v0.1.9**（commit `adf9c76`）生成，其它版本不保证能干净应用。

## 使用方法

### 前提

- 你已经能用上游的安装器或构建脚本跑通完整安装（硬件要求：AVX2、Vulkan 1.3 + `VK_EXT_external_memory_host`）。

### 方式 A：从源码重编运行时库（推荐）

```powershell
# 1. 克隆上游 v0.1.9 并应用补丁
git clone --depth 1 --branch v0.1.9 https://github.com/valentderah/totk-pc-recomp.git src
cd src
git apply --whitespace=nowarn <本仓库>\totk-recomp-zh-Hans-v0.1.9.patch

# 2. 用安装包自带工具链编译运行时库（无需完整构建环境）
cd <安装包目录>          # 如 D:\TotK-Setup\release\TotK-Setup-0.1.9-windows
python\python.exe tools\build_game.py --make-runtime-lib --version v143 `
  --gen temp\<你的目标目录>\gen `
  --runtime D:\TotK-Setup\src\runtime `
  --toolchain llvm --ninja bin\ninja.exe --deps runtime\deps `
  --out D:\TotK-Setup\rtbuild\out
```

`--gen` 指向安装器的工作目录（安装过程中生成，含 `layout.json`）。如果你是完整源码构建用户，`--gen` 用默认生成的目录即可。

### 方式 B：直接下载编译好的库（省事）

本仓库 Releases 提供 `libtotkrt-v0.1.9-zh-Hans.a`（仅运行时宿主库，**不含任何游戏内容**），下载后跳过方式 A 的第 1、2 步。

### 重新链接 totk.exe

```powershell
python\python.exe tools\build_game.py --version v143 `
  --gen temp\<你的目标目录>\gen `
  --runtime runtime `
  --runtime-lib <汉化库路径>\libtotkrt.a `
  --toolchain llvm --ninja bin\ninja.exe --deps runtime\deps `
  --out build\v143
```

用生成的 `build\v143\totk.exe` 替换安装目录（如 `D:\TotK-Recomp`）里的同名文件。

### 启用中文

- 默认：跟随 Windows 显示语言（简体中文系统自动生效）；
- 手动：游戏内菜单 → "菜单"页签 → 菜单语言 → 简体中文，或配置文件里设 `TOTK_MENU_LANG=zh-Hans`。

已知限制：

- 游戏内文本（NPC 对话等）的语言由"游戏语言"设置决定，中文依赖 Switch 共享字体，安装时语言参数需选 `zh-Hans`；若出现空白字，先用 `en-US` 安装。
- 极生僻字可能因系统字体缺字形显示为空白，界面常用字均已覆盖。

## 法律与合规说明

- 上游项目采用 **MIT 许可证**，允许修改与再分发；本补丁保留原版权与许可声明。
- 本补丁只包含我们撰写的翻译文本与少量代码改动，**不含任何游戏资产、反编译内容或解密材料**，符合上游 `CONTRIBUTING.md` 的要求。
- **密钥**（`prod.keys` 等）是任天堂的专有加密材料，分发密钥在多数司法辖区违法（规避技术保护措施），本仓库不提供、不链接、不讨论获取渠道。请仅使用从自己主机导出的密钥。
- 本项目与任天堂无任何关联。《塞尔达传说》及相关商标为任天堂所有；游玩本重编译版需要你合法拥有的游戏副本。

## 许可证

MIT（与上游一致），见 [LICENSE](LICENSE)。
