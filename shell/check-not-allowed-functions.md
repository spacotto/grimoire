# How to scan for not-allowed FNS
The `nm -u` command in a Linux shell displays only the **undefined symbols** from an object file or executable.
Undefined symbols are those that are **referenced within the file but not defined in it**, meaning their definitions must be resolved by the linker through other object files or libraries.
This is particularly useful for identifying missing dependencies or unresolved functions during the linking phase of compilation.

For example, running `nm -u filename.o` will list all symbols that are declared but not defined within the file `filename.o`, typically indicated by the `U` symbol type in the output.
This helps developers pinpoint why a linking error might occur, as the linker cannot find the actual implementation for these symbols.
However, it can also be used to scan, for example, an archive to make sure only allowed functions are present.

### Application
For example, let's say we want to scan `libft.a` to avoid getting a -42.
Let's create our archive with our Makefile. Now that we have our archive, we can run the command `nm -u libft.a`. This will display the list of all the undefined functions.

> [!TIP]
> If we change the command into `nm -u libft.a | less`, we can use the search function (`/`) to look for specific functions.
