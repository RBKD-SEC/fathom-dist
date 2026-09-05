# fathom-dist（fathom 独立二进制分发缝）

fathom 是闭源存活/端口/指纹/高危检测引擎（Rust，零第三方依赖），可独立使用，也可作为
anchorscan / shrike 的外置引擎。本仓**只分发授权二进制**：无源码、无 `.env`、无内部路径、
无 token。源码保留在私有仓库，二进制受 `NOTICE` 中 proprietary 条款约束，禁止逆向。

版本节奏与 anchorscan 解耦：fathom 在私有仓独立打 tag，验收后 promote 到本仓，
anchorscan 发版与否不影响 fathom 更新。

## 安装

```bash
# 1. 下载对应平台的 release 归档 + 校验和
VERSION=0.1.0   # 示例，以实际 release 为准
curl -LO https://github.com/RBKD-SEC/fathom-dist/releases/download/v${VERSION}/fathom-${VERSION}-$(uname -s | tr A-Z a-z)-$(uname -m).tar.gz
shasum -a 256 -c fathom-*.tar.gz.sha256

# 2. 解包并安装
tar -xzf fathom-*.tar.gz
sudo cp fathom-*/fathom /usr/local/bin/   # Windows 为 fathom.exe

# 3. 校验
fathom --version
```

## 使用

<!-- TODO(promote 前): 从私有 fathom 仓 README 提炼独立 CLI 用法（存活/端口/指纹/高危检测的
     命令示例），此处只放分发说明，不复制维护第二份使用文档。 -->

作为 [anchorscan](https://github.com/RBKD-SEC/anchorscan-dist) 的外置引擎使用时：
anchorscan 发行归档**不再捆绑** fathom（ADR-0024），需按上文安装本仓二进制后放入系统
`PATH`，或在 anchorscan 自动生成的 `config/default.yaml` 中把 `tools.fathom` 配置为
绝对路径；未配置时 anchorscan 深度识别补位波记 `skipped/tool_unconfigured` 跳过，不影响其余扫描。

## 平台矩阵

| 归档后缀 | Rust target |
|---|---|
| `linux-amd64` | `x86_64-unknown-linux-musl` |
| `linux-arm64` | `aarch64-unknown-linux-musl` |
| `darwin-arm64` | `aarch64-apple-darwin` |
| `windows-amd64` | `x86_64-pc-windows-gnu` |

## 升级与回滚

- **升级**：下载新 release，校验后替换二进制；历史 release 全部保留。
- **回滚**：任何验证失败 → 使用上一 release 覆盖。
- fathom 版本节奏与 anchorscan 解耦（ADR-0024）：fathom 独立发版，anchorscan 升级或
  回滚均不影响本仓 release；anchorscan 归档不携带 fathom，不存在混配版本问题。

## 内容

- 仅含授权二进制、安装说明、NOTICE、checksums、provenance、release metadata。
- **无源码**：不含 Rust 源码、内部路径、token 或客户数据。
- fathom 为闭源组件，受 `NOTICE` 中 proprietary 条款约束，禁止逆向。

## 验证

每个 release 由私有 fathom 仓交叉编译产物原样 promote 而来，保持原 digest，
**不在本仓重建**。校验见 `checksums.txt`、`provenance.json`。
