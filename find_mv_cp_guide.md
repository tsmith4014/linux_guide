## Guide to Using `find`, `mv`, and `cp` Commands

### Table of Contents

1. Introduction
2. [Using `find`](#using-find)
   - [Basic Syntax](#basic-syntax)
   - Examples
3. [Using `mv`](#using-mv)
   - [Basic Syntax](#basic-syntax-1)
   - Examples
4. [Using `cp`](#using-cp)
   - [Basic Syntax](#basic-syntax-2)
   - Examples
5. [Combining `find` with `mv` and `cp`](#combining-find-with-mv-and-cp)
   - [Move Files Based on Pattern](#move-files-based-on-pattern)
   - [Copy Files Based on Pattern](#copy-files-based-on-pattern)
6. Conclusion

## Introduction

This guide covers the usage of `find`, `mv`, and `cp` commands in Unix-like operating systems. These commands are essential for file management and manipulation.

## Using `find`

### Basic Syntax

```sh
find [path] [options] [expression]
```

### Examples

1. **Find All Files**:

   ```sh
   find /path/to/directory -type f
   ```

2. **Find Files by Name**:

   ```sh
   find /path/to/directory -type f -name "*.txt"
   ```

3. **Find Files by Size**:

   ```sh
   find /path/to/directory -type f -size +1M
   ```

4. **Find Files Modified in the Last 7 Days**:
   ```sh
   find /path/to/directory -type f -mtime -7
   ```

## Using `mv`

### Basic Syntax

```sh
mv [source] [destination]
```

### Examples

1. **Move a File**:

   ```sh
   mv file1.txt /path/to/destination/
   ```

2. **Rename a File**:

   ```sh
   mv oldname.txt newname.txt
   ```

3. **Move Multiple Files**:
   ```sh
   mv file1.txt file2.txt /path/to/destination/
   ```

## Using `cp`

### Basic Syntax

```sh
cp [options] [source] [destination]
```

### Examples

1. **Copy a File**:

   ```sh
   cp file1.txt /path/to/destination/
   ```

2. **Copy Multiple Files**:

   ```sh
   cp file1.txt file2.txt /path/to/destination/
   ```

3. **Copy a Directory Recursively**:
   ```sh
   cp -r /path/to/source_directory /path/to/destination_directory
   ```

## Combining `find` with `mv` and `cp`

### Move Files Based on Pattern

To move all `.png` files from one directory to another:

```sh
find /path/to/source_directory -type f -name "*.png" -exec mv {} /path/to/destination_directory \;
```

### Copy Files Based on Pattern

To copy all `.png` files from one directory to another:

```sh
find /path/to/source_directory -type f -name "*.png" -exec cp {} /path/to/destination_directory \;
```

### Explanation:

- **`find /path/to/source_directory -type f -name "*.png"`**: Finds all `.png` files in the source directory.
- **`-exec mv {} /path/to/destination_directory \;`**: Executes the `mv` command to move each found file to the destination directory.
- **`-exec cp {} /path/to/destination_directory \;`**: Executes the `cp` command to copy each found file to the destination directory.

## Conclusion

This guide provides a comprehensive overview of using `find`, `mv`, and `cp` commands for file management. By combining these commands, you can efficiently search for, move, and copy files based on various criteria. These tools are powerful for automating and streamlining file operations in Unix-like operating systems.
