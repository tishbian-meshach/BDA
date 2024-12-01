# Hadoop File Management Guide

This guide provides essential HDFS (Hadoop Distributed File System) commands for managing files and directories.

## Directory Operations

### Create a Directory
To create a new directory in HDFS:
```hdfs
hdfs dfs -mkdir /user/
```

### List Directory Contents
To list the contents of a directory:
```
hdfs dfs -ls /
```

## File Operations

### Upload and Download Files

#### Upload a File to HDFS
```
hdfs dfs -put <local_path/filename.txt> <hdfs_path>
```

#### Download a File from HDFS
```
hdfs dfs -get <hdfs_path/filename.txt> <local_path>
```

### View File Contents
To see the contents of a file:
```
hdfs dfs -cat <hdfs_path/filename.txt>
```

### Copy Files

#### Copy Within HDFS
To copy a file from one HDFS location to another:
```
hdfs dfs -cp <hdfs_source> <hdfs_dest>
```

#### Copy Between Local File System and HDFS

Copy from local to HDFS:
```
hdfs dfs -copyFromLocal <local_path> <hdfs_path>
```

Copy from HDFS to local:
```
hdfs dfs -copyToLocal <hdfs_path> <local_path>
```

### Move Files
To move a file from one location to another:
```
hdfs dfs -mv <src> <dst>
```

### Delete Files and Directories

Delete a file:
```
hdfs dfs -rm <hdfs_path/file.txt>
```

Delete a directory:
```
hdfs dfs -rmr <hdfs_path>
```

## File Information

### View End of File
To display the last few lines of a file:
```
hdfs dfs -tail <hdfs_path/file.txt>
```

### File Size
To display the aggregate length of a file:
```
hdfs dfs -du <hdfs_path/file.txt>
```

## Notes
- Replace `<local_path>`, `<hdfs_path>`, `<src>`, `<dst>`, etc., with actual file paths.
- Always ensure you have the necessary permissions before performing these operations.
- Use these commands responsibly, especially when deleting or moving files and directories.
