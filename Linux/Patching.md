
Patching is a common practice in the Open-Source community.

It consists of updating specific files to match a modified version aimed to improve, remove or add features. 
>[!tip] Patches and Patchfiles
>Patches are done according to **patch files** which are `.patch` or `.diff` files. Those files are just a set of instruction to correctly modify the initial document to add the **patch**

To patch a file, use the command line utility `patch`.

```shell
patch -i /path/to/patch.diff /path/to/initial-file
```

the `-i` specifies the path to the patch file.
