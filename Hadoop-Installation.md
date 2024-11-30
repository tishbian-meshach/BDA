# Hadoop Installation Guide for Windows

This guide provides step-by-step instructions for installing Hadoop 3.3.0 on Windows.

## Prerequisites

- JDK 8: [Download from Adoptium](https://adoptium.net/temurin/releases/?os=windows&arch=x64&package=jdk&version=8) (.msi)
- Hadoop 3.3.0: [Download from Apache Archive](https://archive.apache.org/dist/hadoop/common/hadoop-3.3.0/hadoop-3.3.0.tar.gz)

## Installation Steps

### Step 1: Unpack the Hadoop Package

1. Create a directory for Hadoop:
   ```
   mkdir C:\Hadoop
   ```

2. Unzip the Hadoop package:
   ```
   cd Downloads
   tar -xvzf hadoop-3.3.0.tar.gz -C C:\Hadoop\
   ```

### Step 2: Install Java

Install the downloaded JDK 8 (jdk8.msi).

### Step 3: Install Hadoop Native IO Binary

1. Clone the repository with pre-built Hadoop Windows native libraries:
   ```
   git clone https://github.com/ruslanmv/How-to-install-Hadoop-on-Windows.git
   ```

2. Copy the files to the Hadoop bin folder:
   ```
   cd How-to-install-Hadoop-on-Windows\winutils\hadoop-3.3.0-YARN-8246\bin
   copy *.* C:\Hadoop\hadoop-3.3.0\bin
   ```

### Step 4: Configure Environment Variables

1. Set JAVA_HOME:
   - Variable name: `JAVA_HOME`
   - Variable value: C:\Program Files\Java\jdk1.8.0_202

2. Set HADOOP_HOME:
   - Variable name: HADOOP_HOME
   - Variable value: C:\Hadoop\hadoop-3.3.0

3. Add to PATH:
   - C:\Program Files\Java\jdk1.8.0_202\bin
   - C:\Hadoop\hadoop-3.3.0\bin

### Step 5: Configure Hadoop

1. Edit core-site.xml (C:\Hadoop\hadoop-3.3.0\etc\hadoop\core-site.xml):
   ```xml
   <configuration>
      <property>
        <name>fs.default.name</name>
        <value>hdfs://0.0.0.0:19000</value>
      </property>
   </configuration>
   ```

2. Create directories for HDFS:
   ```
   mkdir C:\hadoop\hadoop-3.3.0\data\datanode
   mkdir C:\hadoop\hadoop-3.3.0\data\namenode
   ```

3. Edit hdfs-site.xml (C:\Hadoop\hadoop-3.3.0\etc\hadoop\hdfs-site.xml):
   ```xml
   <configuration>
      <property>
        <name>dfs.replication</name>
        <value>1</value>
      </property>
      <property>
        <name>dfs.namenode.name.dir</name>
        <value>/hadoop/hadoop-3.3.0/data/namenode</value>
      </property>
      <property>
        <name>dfs.datanode.data.dir</name>
        <value>/hadoop/hadoop-3.3.0/data/datanode</value>
      </property>
   </configuration>
   ```

4. Edit mapred-site.xml (C:\Hadoop\hadoop-3.3.0\etc\hadoop\mapred-site.xml):
   ```xml
   <configuration>
       <property>
           <name>mapreduce.framework.name</name>
           <value>yarn</value>
       </property>
       <property> 
           <name>mapreduce.application.classpath</name>
           <value>%HADOOP_HOME%/share/hadoop/mapreduce/*,%HADOOP_HOME%/share/hadoop/mapreduce/lib/*,%HADOOP_HOME%/share/hadoop/common/*,%HADOOP_HOME%/share/hadoop/common/lib/*,%HADOOP_HOME%/share/hadoop/yarn/*,%HADOOP_HOME%/share/hadoop/yarn/lib/*,%HADOOP_HOME%/share/hadoop/hdfs/*,%HADOOP_HOME%/share/hadoop/hdfs/lib/*</value>
       </property>
   </configuration>
   ```

5. Edit yarn-site.xml (C:\Hadoop\hadoop-3.3.0\etc\hadoop\yarn-site.xml):
   ```xml
   <configuration>
       <property>
           <name>yarn.nodemanager.aux-services</name>
           <value>mapreduce_shuffle</value>
       </property>
       <property>
           <name>yarn.nodemanager.env-whitelist</name>
           <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
       </property>
   </configuration>
   ```

6. Edit hadoop-env.cmd (C:\Hadoop\hadoop-3.3.0\etc\hadoop\hadoop-env.cmd):
   ```
   set JAVA_HOME=C:\Progra~1\Java\jdk1.8.0_202
   @rem set HADOOP_CONF_DIR=C:\Hadoop\hadoop-3.3.0\etc\hadoop
   ```

   If you encounter a system name error, change the username in the last line:
   ```
   set HADOOP_IDENT_STRING=%USERNAME%
   ```

### Step 6: Initialize HDFS

Run the following command:
```
hdfs namenode -format
```

### Step 7: Start HDFS Daemons

Run the following command:
```
%HADOOP_HOME%\sbin\start-dfs.cmd
```

Verify HDFS web portal UI: [http://localhost:9870/](http://localhost:9870/)

### Step 8: Start YARN Daemons

Run the following command as administrator:
```
%HADOOP_HOME%\sbin\start-yarn.cmd
```

Verify YARN resource manager UI: [http://localhost:8088](http://localhost:8088)

## Troubleshooting

- If you encounter any DLL errors, download the missing DLL and place it in the C:\Windows\System32 directory.
- Ensure all paths are correct and match your system configuration.
- If you face permission issues, try running the commands as an administrator.
