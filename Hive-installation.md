# Apache Hive Installation Guide

This guide provides step-by-step instructions for installing Apache Hive on Windows.

## Prerequisites

- Hadoop 3.2.4 installed and configured

## 1. Download and Configure Apache Derby

1. Download Derby 10.14.2.0 from [Apache Derby Downloads](https://db.apache.org/derby/derby_downloads.html)
2. Extract the downloaded file
3. Copy the extracted folder to `C:` and rename it to `derby`

## 2. Download and Configure Apache Hive

1. Download Hive 3.1.2 from [Apache Hive Archive](https://archive.apache.org/dist/hive/hive-3.1.2/)
2. Extract the downloaded file
3. Copy the extracted folder to `C:` and rename it to `hive`
4. Copy all `*.jar` files from `C:\derby\lib` to `C:\hive\lib`

## 3. Configure Hive

1. Create a file named `hive-site.xml` in `C:\hive\conf` with the following content:

   ```xml
   <?xml version="1.0"?>
   <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
   <configuration>
       <property>
           <name>javax.jdo.option.ConnectionURL</name>
           <value>jdbc:derby://localhost:1527/metastore_db;create=true</value>
           <description>JDBC connect string for a JDBC metastore</description>
       </property>
       <property>
           <name>system:java.io.tmpdir</name>
           <value>C:/hive/tmp</value>
       </property>
       <property>
           <name>system:user.name</name>
           <value>${user.name}</value>
       </property>
       <property>
           <name>javax.jdo.option.ConnectionDriverName</name>
           <value>org.apache.derby.jdbc.ClientDriver</value>
           <description>Driver class name for a JDBC metastore</description>
       </property>
       <property>
           <name>hive.metastore.warehouse.dir</name>
           <value>/user/hive/warehouse</value>
           <description>Location of default database for the warehouse</description>
       </property>
       <property>
           <name>hive.server2.enable.doAS</name>
           <value>true</value>
           <description>Enable user impersonation for HiveServer2</description>
       </property>
       <property>
           <name>hive.server2.authentication</name>
           <value>NONE</value>
           <description>Client authentication types.</description>
       </property>
       <property>
           <name>datanucleus.autoCreateTables</name>
           <value>true</value>
           <description>Auto-create tables in the metastore</description>
       </property>
   </configuration>
   ```

## 4. Verify Guava JAR File

1. Ensure that the version of `guava-x.y-jre.jar` in `C:\hive\lib` matches the one in `C:\hadoop\share\hadoop\common\lib`
2. If versions don't match, copy the Hadoop version to `C:\hive\lib` and delete the other version

## 5. Create Temporary Folder

1. Create a folder named `tmp` in `C:\hive`
2. Set appropriate permissions for the `tmp` folder

## 6. Set Up Environment Variables

Add the following environment variables:
- `DERBY_HOME`: `C:\derby`
- `HIVE_HOME`: `C:\hive`
- `HIVE_BIN`: `C:\hive\bin`
- `HIVE_LIB`: `C:\hive\lib`
- `HADOOP_USER_CLASSPATH_FIRST`: `true`

## 7. Install Wget

1. Download Wget from [Eternally Bored](https://eternallybored.org/misc/wget/)
2. Extract the downloaded files
3. Copy the extracted folder to `C:` and rename it to `wget`
4. Add `C:\wget` to the system PATH

## 8. Download Windows Version of Hive Bin

1. Create a directory (e.g., `C:\bin`)
2. Open Command Prompt and run:
   ```
   wget -r -np -nH --cut-dirs=3 -R index.html https://svn.apache.org/repos/asf/hive/trunk/bin/
   ```
3. Navigate to `C:\bin` and its subdirectories to find the `bin` folder
4. Replace `C:\hive\bin` with the downloaded `bin` folder

## 9. Start Hadoop Daemons

1. Run `start-all.cmd`
2. Verify daemons are running with `jps`

## 10. Run Derby Server

Run the following command:
```
StartNetworkServer -h 0.0.0.0
```

## 11. Initialize Hive Schema

Navigate to `C:\hive\bin` and run:
```
hive --service schematool -dbType derby -initSchema
```

## 12. Start Hive

Run the following command:
```
hive
```

## 13. Hive Example Command

After successfully starting Hive, you can run Hive queries. Here's a simple example to create a table and insert data:

```sql
-- Create a new table
CREATE TABLE test_table (id INT, name STRING);

-- Insert data into the table
INSERT INTO test_table VALUES (1, 'John Doe'), (2, 'Jane Smith');

-- Query the table
SELECT * FROM test_table;
```

This example creates a table named `test_table` with two columns, inserts two rows of data, and then retrieves all records from the table.

## Troubleshooting

- If you encounter any issues, check the Hadoop and Hive log files
- Ensure all paths and environment variables are set correctly
- Verify that all required services (Hadoop, Derby) are running before starting Hive
