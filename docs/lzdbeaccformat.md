## .LZ/DBE/ACC
.LZ files or DBE/ACC elements within .AP archives are run-length compressed data. The compression algorithm is essentially the variant of LZW used in .GIF files.

### Layout
| Name | Offset | Size | Format | Notes |
| ---- | ------ | ---- | ------ | ----- |
| Magic Number | `0x00000000` | 4 bytes | 3 ASCII characters with `0x01` terminator | "DBE" or "ACC" + 0x01 |
| Decompressed Length | `0x00000004` | 4 bytes | little-endian unsigned 32-bit integer ||
| Compressed Length | `0x00000008` | 4 bytes | little-endian unsigned 32-bit integer ||
| (reserved) | `0x0000000C` | 4 bytes | n/a | |
| Data | `0x00000010` | 1 byte * Compressed length | LZW compressed codestream | |

### Decompression

LZW is a dictionary-based compression method. 
