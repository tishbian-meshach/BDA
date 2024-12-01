# Sqoop Installation and Usage Guide

This guide provides step-by-step instructions for installing and using Apache Sqoop on Windows.

## Prerequisites

- MySQL: [Download MySQL 9.1](https://dev.mysql.com/get/Downloads/MySQL-9.1/mysql-9.1.0-winx64.msi)
  - After installing, configure MySQL with MySQL Configurator
- MySQL Workbench: [Download MySQL Workbench 8.0.40](https://dev.mysql.com/get/Downloads/MySQLGUITools/mysql-workbench-community-8.0.40-winx64.msi)
- Sqoop: [Download Sqoop 1.4.7](https://archive.apache.org/dist/sqoop/1.4.7/sqoop-1.4.7.bin__hadoop-2.6.0.tar.gz)

## Installation Steps

### 1. Unzip and Install Sqoop

1. Extract the `sqoop-1.4.7.bin__hadoop-2.6.0.tar.gz` file
2. Extract the resulting `.tar` file
3. Move the extracted folder to a desired location (e.g., `D:\sqoop`)

**Note:** Avoid using spaces in folder names to prevent issues.

### 2. Set Up Environment Variables

1. Add a new User Variable:
   - Variable name: `SQOOP_HOME`
   - Variable value: Path to your Sqoop installation (e.g., `D:\sqoop`)

2. Edit the System `Path` variable:
   - Add `%SQOOP_HOME%\bin`

3. Verify the setup:
   - Open a new Command Prompt
   - Run: `echo %SQOOP_HOME%`

### 3. Replace commons-lang.jar

1. Delete the existing `commons-lang.jar` in `C:\sqoop\lib`
2. Download [commons-lang-2.6-bin.tar.gz](https://dlcdn.apache.org//commons/lang/binaries/commons-lang-2.6-bin.tar.gz)
3. Extract and place the new `commons-lang.jar` in `C:\sqoop\lib`

### 4. Configure Sqoop

#### 4.1 Install MySQL Connector

1. Download [mysql-connector-java-8.0.15.tar.gz](http://ftp.ntu.edu.tw/MySQL/Downloads/Connector-J/mysql-connector-java-8.0.15.tar.gz)
2. Extract and place `mysql-connector-java-8.0.15.jar` in the `lib` folder of Sqoop

#### 4.2 Configure sqoop-env.cmd

1. In `C:\sqoop\conf`, rename `sqoop-env-template.cmd` to `sqoop-env.cmd`
2. Edit `sqoop-env.cmd` and uncomment these lines:
   ```
   set HADOOP_COMMON_HOME=C:\Hadoop\hadoop-3.3.0
   set JAVA_HOME=C:\Program Files\Java\jdk1.8.0_202
   set HADOOP_MAPRED_HOME=C:\Hadoop\hadoop-3.3.0
   ```

#### 4.3 Create MySQL Users

1. Open MySQL Workbench
2. Go to Administration > Users and Privileges
3. Create two users:
   - Username: `sqoop`, Host: `localhost`
   - Username: `hive`, Host: `localhost`
4. Assign roles: DBManager, DBDesigner, BackupAdmin
5. Grant schema privileges for both users

#### 4.4 Grant Permissions to Users

Run these commands in MySQL command line:

```mysql
grant all privileges on _bigdata.* to 'sqoop'@'localhost';
grant all privileges on _bigdata.* to 'hive'@'localhost';
```


## Testing the Setup

Run the following command to test the connection:

```
sqoop list-databases --connect jdbc:mysql://localhost/ --username sqoop -P
```

Enter the password when prompted. You should see a list of databases.

## Importing Data from MySQL to HDFS

Before importing, create a database and table in MySQL. Then run:

```
sqoop import --connect jdbc:mysql://localhost:3306/tish --username root --password root --table users --m 1 --direct
```

Replace `tish` with your database name, and `users` with your table name.

## Exporting Data from HDFS to MySQL

To export data from HDFS to MySQL:

```
sqoop export --connect jdbc:mysql://localhost:3306/tish --username root --password root --table users --export-dir /user/wwwst/users
```

Replace `tish` with your database name, `users` with your table name, and `/user/wwwst/users` with the HDFS directory containing your data.

## Troubleshooting

- If you encounter any issues, check the Sqoop and Hadoop log files
- Ensure all paths and environment variables are set correctly
- Verify that MySQL and Hadoop services are running before using Sqoop

