# promote 流程（内部，随 release 执行后更新本仓）

1. 私有 fathom 仓按 tag 交叉编译四平台（矩阵见 README），产物 sha256 记录。
2. 组包：`fathom-<ver>-<os>-<arch>.tar.gz`（内含 `fathom`/`fathom.exe`）+ 同名 `.sha256`。
3. 更新 `checksums.txt`（全部历史行保留）、`provenance.json`、`release-metadata.json`。
4. `gh release create v<ver> dist/fathom-*.tar.gz dist/fathom-*.sha256` 于 RBKD-SEC/fathom-dist。
5. anchorscan 发行归档自 ADR-0024 起不携带 fathom，无需向 anchorscan 侧做任何同步。
