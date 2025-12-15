# Long Listing Format (`ls -l`)
`ls` lists  information  about the files (in the current directory by default). It sorts entries alphabetically if none of `-cftuvSUX` nor `--sort` is specified.

By adding the `-l` flag, the so-called **long listing format** is triggered.

```
.
|    .
|    |     .
|    |     |   .
|    |     |   |     .
|    |     |   |     |    .----------------------------
|    |     |   |     |    |       .--------------------
|    |     |   |     |    |       |         .---------- 
|    |     |   |     |    |       |         |
-rw-r--r-- 1 owner group 256 Jan 1 15:15 file.txt
```

