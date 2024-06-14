## .AP 
.AP files are archives comprising multiple elements. Each element is a resource file used by the game. .AP files may also be stored as elements within other .AP files.

### Header
| Offset | Size | Format | Description | Notes |
| --- | --- | --- | --- | --- |
| `0x00000000` | 2 bytes | little-endian unsigned 16-bit integer | Number of element offsets | One greater than the number of elements |
| `0x00000002` | 4 bytes x number of element offsets | little-endian unsigned 32-bit integer array | Offset of each element relative to the start of the file | The final offset points one-beyond the end of the file |

The remainder of the file consists of the elements concatenated together.

### Example

![alt text](APExample.png?raw=true "An abridged image of the contents of C29.AP with header entries and elements highlighted" )
