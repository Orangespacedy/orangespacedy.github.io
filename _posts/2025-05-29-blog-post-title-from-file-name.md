## Ubuntu Useful Commands

--- system Ubuntu 25.04

#### - 1. System and user
Display system info
```
$ uname -a
```
display date
```
$ date
```
Sudo means run the command line with superuser privileges
```
$ sudo [command]
```
shutdown immediately 
```
$ sudo shutdown -h now
```
list all running processes
```
$ ps aux
```
#### - 2. Directory and folder
If the folder name contains space, use "". For example, "Data Analysis".
list directory contents
```
$ ls [option] [directory]
```
change directory
```
$ cd [directory]
```
print working directory
```
$ pwd
```
create a new directory
```
$ mkdir [newdirectory]
```
remove
'chmod +w' command adds write permissions to the directory. 
'rm -r' deletes the directory without requiring superuser privileges.
```
$ chmod +w [directory]
$ rm [option] [directory]
```
copy and move
```
$ cp [options] [source] [destination]
$ mv [options] [source] [destination]
```
#### - 3. File

Create a new empty file
```
$ touch [file_name]
```
display content of a file
```
$ cat [file_name]
```
edit a text file
```
$ nano [file_name]
```
search for patterns in files
```
$ grep [options] [pattern] [file_name]
```
change file permissions
```
$ chmod [options] [mode] [file_name]
```
change file owner
```
$ chown [options] [owner:group] [file_name]
```

#### - 4. Download and package
Download from internet
```
$ wget [URL]
```
Extract tar archives, for example 'tar -cvzf'
```
$ tar [options] [archive_name.tar.gz] [files/directories]
```

