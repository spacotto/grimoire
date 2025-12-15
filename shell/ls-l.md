# Long Listing Format (`ls -l`)
`ls` lists  information about the items (in the current directory by default). It sorts entries alphabetically if none of `-cftuvSUX` nor `--sort` is specified.

By adding the `-l` flag, the so-called **long listing format** is triggered.

```
.------------------------------------------------------ Data type
|    .------------------------------------------------- Permissions (read, write, execute)
|    |     .------------------------------------------- Number of hard links
|    |     |   .--------------------------------------- Owner (user) name or ID
|    |     |   |     .--------------------------------- Group name or ID
|    |     |   |     |    .---------------------------- Item size in bytes
|    |     |   |     |    |       .-------------------- Last modification (month, day, and time)
|    |     |   |     |    |       |         .---------- Item name
|    |     |   |     |    |       |         |
-rw-r--r-- 1 owner group 256 Jan 1 15:15 file.txt
```

## Data types
| Symbol | Type                    | Description                                                          |
| :----- | :---------------------- | :------------------------------------------------------------------- |
| `-`    | Regular file            | A normal file (text, binary, etc.)                                   |
| `d`    | Directory               | A folder containing other files/directories                          |
| `l`    | Symbolic link (symlink) | A shortcut or reference to another file                              |
| `c`    | Character device file   | For devices that handle data as streams (e.g. keyboard, serial port) |
| `b`    | Block device file       | For devices that handle data in blocks (e.g. hard drives)            |
| `s`    | Socket                  | Used for inter-process communication                                 |
| `p`    | Named pipe (FIFO)       | A special file for communication between processes                   |
| `D`    | Door                    | Special type on Solaris for fast RPC                                 |

## Permissions (read, write, execute)
| `rwx`            | `rwx`             | `rwx`              |
| ---------------- | ----------------- | ------------------ |
| User Permissions | Group Permissions | Others Permissions |

### `chmod`
You can change the permissions by running this command in the terminal:
```
chmod xx file_name         # Enter the chosen operator and permission aliases (see below) instead of -x
```

| Operator | Meaning                |
| :------- | :--------------------- |
| `+`      | Add the file mode bits to the existing file mode bits of the item      |
| `-`      | Remove the file mode bits from the existing file mode bits of the item |
| `=`      | Substitutes the existing file mode bits of the item with the ones specified after the operator |

| Permission | Meaning |
| :--------- | :------ |
| `r`        | Read    |
| `w`        | Write   |
| `x`        | Execute |

---

You can also set up the permissions altogether by using their respective values:
```
chmod xxx file_name        # Enter the chosen value (see below) instead of xxx
```

| Permission | Binary | Value | Meaning                |
| :--------- | :----- | :---- | :--------------------- |
| `---`      | `000`  | `0`   | No permission          |
| `--x`      | `001`  | `1`   | Execute only           |
| `-w-`      | `010`  | `2`   | Write only             |
| `-wx`      | `011`  | `3`   | Write + Execute        |
| `r--`      | `100`  | `4`   | Read only              |
| `r-x`      | `101`  | `5`   | Read + Execute         |
| `rw-`      | `110`  | `6`   | Read + Write           |
| `rwx`      | `111`  | `7`   | Read + Write + Execute |

## Hard links

## Owner

## Group

## Item size

## Last modification

## Item name
