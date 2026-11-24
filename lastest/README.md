# TKX Boot App A/B（version 0x00010001）

| 文件 | 槽 | 链接地址 | `VECT_TAB_OFFSET` | 大小 | CRC32 |
|---|---|---|---|---|---|
| `tkx-boot-app-A.bin` | A | `0x08020000` | `0x00020000` | 7140 B | `9BBC68F5` |
| `tkx-boot-app-B.bin` | B | `0x08040000` | `0x00040000` | 7140 B | `AD7F49FD` |

SHA256 见 `SHA256SUMS.txt`。

## 发布步骤

1. GitHub 新建 Release（tag 例如 `v1.0.2`）。
2. 上传：
   - `tkx-boot-app-A.bin`
   - `tkx-boot-app-B.bin`
   - `manifest.txt`
3. `manifest.txt` 里的 `version` 必须和 ESP 固件里的 `FW_VERSION_U32` 一致。
4. 如果这次改了版本号，ESP 端也要同步改 `FW_VERSION_U32` 并重新编译；这个值会写进 BL 的 Meta。

## manifest 说明

`manifest.txt` 只写了版本：

```text
version=0x00010001
```

`a_url` / `b_url` 省略时，ESP 会用 `config.h` 里的 `FW_URL_A` / `FW_URL_B`，默认指向：

```text
https://github.com/ZasshokuDoubutsu/tkx-boot-release-bin/releases/latest/download/tkx-boot-app-A.bin
https://github.com/ZasshokuDoubutsu/tkx-boot-release-bin/releases/latest/download/tkx-boot-app-B.bin
```

想锁定某个 release，把 `manifest.txt` 里注释掉的 `a_url` / `b_url` 改成对应 tag 的地址。

## ESP 端怎么启用版本比较

把 `MANIFEST_URL` 改成：

```cpp
#define MANIFEST_URL "https://github.com/ZasshokuDoubutsu/tkx-boot-release-bin/releases/latest/download/manifest.txt"
```

- 留空：每次进 BL 都会重新下载并刷写
- 填上：A/B 两个槽的版本都等于 manifest 的 `version` 后，直接跳过，不碰 BL

## 校验

```powershell
Get-FileHash .\tkx-boot-app-A.bin -Algorithm SHA256
Get-FileHash .\tkx-boot-app-B.bin -Algorithm SHA256
```

对着 `SHA256SUMS.txt` 核一下即可。
