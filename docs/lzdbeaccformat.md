## .LZ/DBE/ACC
.LZ files or DBE/ACC elements within .AP archives are run-length compressed data. The compression algorithm is essentially the variant of LZW used in .GIF files.

### Layout
| Offset | Size | Format | Name | Notes |
| ------ | ---- | ------ | ----------- | ----- |
| `0x00000000` | 4 bytes | 3 ASCII characters with `0x01` terminator | Magic number | "DBE" or "ACC" + 0x01 |
| `0x00000004` | 4 bytes | little-endian unsigned 32-bit integer | Decompressed length ||
| `0x00000008` | 4 bytes | little-endian unsigned 32-bit integer | Compressed length ||
| `0x0000000C` | 4 bytes | n/a | (unused) |
| `0x00000010` | 1 byte * Compressed length | LZ compressed | Data | |

