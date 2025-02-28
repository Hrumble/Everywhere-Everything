
`ls -l` lists all files in a directory, `-l` stands for long listing and displays the metadata of a given file/directory, including its **permissions**.

```shell
root@localhost:~/obs/priv_dir# ls -l

total 4
drwxr-xr-x 2 root root 4096 Mar 11 20:52 my_dir_1
-rw-r--r-- 1 root root    0 Mar 11 20:52 my_file_1.txt
-rwxr-xr-- 1 root group1  4 Mar 11 20:52 my_file_2.sh
```

**Let's break this down.**

the first column contains:
1. `-` or `d`, which means the following *entry* is either a file `-` or a directory `d`
2. the following 9 bits `rw-r--r--` are the **permissions** which will be focused on here.
3. the last bit `.` (there's none here), i have no clue what it is.

The second column `root` contains the **user that owns the file**.

The Third column contains the **group** that owns the file.

The Fourth column represents the file size in [[Binary|Bytes]].

The Fifth column represents the **last modified date and time**.

The Sixth is the **filename**.

***
# Permissions

The file permissions e.g. `rwxr-x-r--` are actually read as 3 different sets of permissions for different user classes. The permissions can be `r` (read) `w` (write) and `x` (execute), `-` means that no permission is set.

The first set of permissions `rwx` are the permissions that the owner has over the file, in our case, the owner has `rwx` so `read write execute` permissions.

The second set of permissions are the permissions that the **group** has over the file, in our case, 
`r-x` means that the group can `r` read, `-` can't write, `x` can execute.

The third set of permissions are the permissions set for **others**, which are user that are not the owners, and not in the owning group, in our case, **others** can only `r` read the file (`--` for `write` and `execute`).

**The user classes can be represented in symbolic mode: `u` (user) `g` (group) `o` (others)**

>[!danger] Of course, the root user overrides any permission, it can even change the permissions.

>[!info] Check Order
>1. It first checks to see whether you are the user that owns the file. If so, then you are granted the user owner's permissions, and no further checks will be completed.
> 2. If you are not the user that owns the file, next your group membership is validated to see whether you belong to the group that matches the group owner of the file. If so, then you're covered under the group owner field of permissions, and no further checks will be made.
> 3. "Others" permissions are applied when the account interacting with the file is neither the user owner nor in the group that owns the files. Or, to put it another way, the three fields are mutually exclusive: You can not be covered under more than one of the fields of permission settings on a file.

>[!note] A directory, to be accessed by any user needs execute permission.
## Octal Values

Linux file permissions are most often times represented by numbers (e.g. `744`), these are called octal values, and work as follows:

`r` (read) = $4$
`w` (write) = $2$
`x` (execute) = $1$

Each digit in the `744` represents respectively the **owning user**, the **owning group**, and the **others** permissions.
The `7` is the addition of `4+2+1` = `r+w+x` = `7`, so **the owner has `read write execute` permissions**.
`4` is `4+0+0` = `read` permissions, so group and other can only read the file.

as a little table


| value | permissions          |
| ----- | -------------------- |
| 0     | no permissions       |
| 3     | `write` `execute`    |
| 5     | `read` `execute`     |
| 6     | `read` `write`       |
| 7     | `read write execute` |
***
# Setting Permissions

You can set permissions with the `chmod` command which stands for **change mode**. 

simply 
```shell
root@localhost:~/obs/priv_dir# chmod <permission> <filename>
```

You can input the permission either in [[#Octal Values]] or **Symbolic Mode**.
**Symbolic Mode** works by writing the **user class** a `+` and the desired permissions you want to grant that user class

```shell
root@localhost:~/obs/priv_dir# chmod u+rw my_file_1
```
 **or**
```shell
root@localhost:~/obs/priv_dir# chmod 644 my_file_1
```
These two grant the **owning user** `read` and `write` permissions.

You can specify multiple user classes in symbolic mode, by just sticking them together, setting the `rw` permissions for `u` and `g` would look like
```shell
root@localhost:~/obs/priv_dir# chmod ug+rw my_file_1
```
***
# Special Permissions

>[!info] Special permissions are available for files and directories instead of the execute permissions, and provide additional privileges than those seen above

## SUID
**Set User ID**
`u+s`

This permission allows any user class with an execute permission to execute the file as the **owner of the file**. So a file containing the `s` **SUID** permission can be executed as if the owner was executing it.
```shell
-rwsr-xr-x 1 root group1    0 Mar 11 20:52 my_executable.sh
```
`my_executable.sh` can be executed by both `others` and `group1` as `root`.

The **SUID** perm set on a file means the file is executed as the owner of it.
The **SUID** perm set on a directory means every file created under that directory will be owned by the owner of the directory.

If the owner himself does **not** have executable permission, then the **SUID** perm is set as `S` instead of `s`.
```shell
-rwSr-xr-x 1 root group1    0 Mar 11 20:52 my_executable.sh
```

>[!info] The SUID permission is a permission only for the **USER** (owning user) user class.

## SGID
**Set Group ID**
`g+s`

This permission is the equivalent of [[#SUID]] except for groups. This means users can execute files as the **owning group**, or create files owned by the group in a directory.

```shell
-rwxr-sr-x 1 root group1    0 Mar 11 20:52 my_executable.sh
```
`my_executable.sh` can be executed by `others` as `group1`.
```shell
drwxr-sr-x 1 root group1    0 Mar 11 20:52 my_dir
```
any file created inside `my_dir` will inherit the `group1` user group.


## Sticky Bit
`o+t`

This permission does not affect individual files. However, at the directory level, it restricts file deletion. Only the **owner** (and **root**) of a file can remove the file within that directory
```shell
drwxr-xr-t 1 root group1    0 Mar 11 20:52 my_dir
```


# Setting Special Permissions

To set them, either use their symbolic notation:
```shell
root@localhost:~# chmod u+s my_executable.sh
```
This adds [[#SUID]] permission to `my_executable.sh`

or use octal values. To use octal values, we must add a 4th preceding digit to our octal permission, with the following values.

`SUID` = $4$
`SGID` = $2$
`Sticky` = $1$

so setting an executable's permission to `-rwsr-xr-x` would be `chmod 4755` `4` for `SUID` `7` for `rwx`, and `5` for `r-x`