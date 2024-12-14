# compression in linux

## compression

```bash
tar -czvf archive_name.tar.gz /path/to/directory
```

## decompression

```bash
tar -xzvf archive_name.tar.gz
```

# Handy Linux Commands

## 1. Compressing and Decompressing in Linux

### Compressing a Directory

#### Using Gzip

```bash
tar -czvf archive_name.tar.gz /path/to/directory
```

Using Bzip2

```bash
tar -cjvf archive_name.tar.bz2 /path/to/directory
```

Using XZ

```bash
tar -cJvf archive_name.tar.xz /path/to/directory
```

# decompressing

Decompressing Gzip Archive (.tar.gz)

```bash
tar -xzvf archive_name.tar.gz
```

Decompressing Bzip2 Archive (.tar.bz2)

```bash
tar -xjvf archive_name.tar.bz2
```


Decompressing XZ Archive (.tar.xz)

```bash
tar -xJvf archive_name.tar.xz
```

## 2. Changing Permissions

Change the Permissions of All Folders Inside a Directory

```bash
find /path/to/parent_directory -type d -exec chmod 755 {} \;
```

Change the Permissions of All Files Inside a Directory

```bash
find /path/to/parent_directory -type f -exec chmod 644 {} \;
```

# Compressing and Decompressing with 7z

### Installing 7zip

```bash
sudo apt install p7zip-full
```

### Compressing
usage example :

```bash
7z a archive_name.7z /path/to/directory
```

### Decompressing

```bash
7z x archive_name.7z
```








