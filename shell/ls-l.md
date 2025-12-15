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

## Hard links

## Owner

## Group

## Item size

## Last modification

## Item name
