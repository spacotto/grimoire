# Long Listing Format (`ls -l`)
`ls` lists  information about the items (in the current directory by default). It sorts entries alphabetically if none of `-cftuvSUX` nor `--sort` is specified.

By adding the `-l` flag, the so-called **long listing format** is triggered.

```
.------------------------------------------------------ Data type
|    .------------------------------------------------- Permissions (read, write, execute)
|    |     .------------------------------------------- Number of hard links
|    |     |   .--------------------------------------- Owner (user) name or ID
|    |     |   |     .--------------------------------- Group name or ID
|    |     |   |     |    .---------------------------- File size in bytes
|    |     |   |     |    |       .-------------------- Last modification (month, day, and time)
|    |     |   |     |    |       |         .---------- Item name
|    |     |   |     |    |       |         |
-rw-r--r-- 1 owner group 256 Jan 1 15:15 file.txt
```

