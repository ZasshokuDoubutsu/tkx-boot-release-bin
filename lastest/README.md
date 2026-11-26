# TKX Boot App A/B（version 0x00010002）

| 文件 | 槽 | 链接地址 | `VECT_TAB_OFFSET` | 大小 | CRC32 |
|---|---|---|---|---|---|
| `tkx-boot-app-A.bin` | A | `0x08020000` | `0x00020000` | 11732 B | `8AC8FF0F` |
| `tkx-boot-app-B.bin` | B | `0x08040000` | `0x00040000` | 11732 B | `7EA9D957` |

这一版 App 启动后会在 OLED 上显示：

```text
        TKX-APP
A v1.0.2 0x00010002
```

SHA256 见 `SHA256SUMS.txt`。

## 发布步骤

1. GitHub 新建 Release（tag 例如 `v1.0.2`），不要勾 draft / prerelease，保证它是 latest。
2. 上传 **三个** 文件作为 release asset：
   - `tkx-boot-app-A.bin`
   - `tkx-boot-app-B.bin`
   - `version.txt`（必须有，否则 ESP 拉版本失败会显示 `no net`）
3. `version.txt` 必须和本次 App 的版本一致，否则会出现“刷完还提示更新”。
4. 当前 ESP 已启用 `VERSION_URL`：写入 BL Meta 的版本号直接取 `version.txt`；
   `FW_VERSION_U32` 只在关闭版本检查时兜底，仍建议同步修改。

## version.txt

内容只有一行：

```text
0x00010002
```

ESP 下载后和 HELLO 返回的 `app_ver` 比较：

- 相同 → `no update: click`，不下载
- 不同 → `new: long press`，长按后下载

A/B 的下载地址固定用 `config.h` 里的 `FW_URL_A` / `FW_URL_B`：

```text
https://github.com/ZasshokuDoubutsu/tkx-boot-release-bin/releases/latest/download/tkx-boot-app-A.bin
https://github.com/ZasshokuDoubutsu/tkx-boot-release-bin/releases/latest/download/tkx-boot-app-B.bin
```

## 校验

```powershell
Get-FileHash .\tkx-boot-app-A.bin -Algorithm SHA256
Get-FileHash .\tkx-boot-app-B.bin -Algorithm SHA256
```

对着 `SHA256SUMS.txt` 核一下即可。
