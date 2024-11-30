# HBase Installation Guide for Linux

This guide provides step-by-step instructions for installing and configuring Apache HBase on Linux.

## Prerequisites

- JDK 8 installation
- Root or sudo access

## 1. Install JDK 8

1. Download JDK 8 from [Adoptium](https://adoptium.net/temurin/releases/?os=linux&arch=x64&package=jdk&version=8)

2. Extract and install JDK:
   ```bash
   sudo mv ~/Downloads/jdk8u432-b06 /usr/lib/jvm/jdk1.8.0
   sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0/bin/java 1
   sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0/bin/javac 1
   sudo update-alternatives --config java
   sudo update-alternatives --config javac
   ```

3. Verify installation:
   ```bash
   java -version
   ```

## 2. Install HBase

1. Create HBase directory:
   ```bash
   sudo mkdir -p /var/hbase
   ```

2. Download and extract HBase:
   ```bash
   # Download HBase
   wget https://dlcdn.apache.org/hbase/stable/hbase-2.5.10-bin.tar.gz
   
   # Extract the archive
   tar xzvf hbase-2.5.10-bin.tar.gz
   
   # Move to installation directory
   sudo mv ~/Downloads/hbase-2.5.10/ /var/hbase
   ```

3. Navigate to HBase directory:
   ```bash
   cd /var/hbase/hbase-2.5.10
   ```

## 3. Configure Environment Variables

1. Edit `.bashrc`:
   ```bash
   sudo nano ~/.bashrc
   ```

2. Add the following lines:
   ```bash
   export JAVA_HOME=/usr/lib/jvm/jdk1.8.0
   export PATH=$JAVA_HOME/bin:$PATH

   export HBASE_HOME=/var/hbase/hbase-2.5.10
   export PATH=$PATH:$HBASE_HOME/bin
   ```

## 4. Configure HBase

1. Configure Java home in `hbase-env.sh`:
   ```bash
   sudo nano conf/hbase-env.sh
   ```
   Add:
   ```bash
   export JAVA_HOME=/usr/lib/jvm/jdk1.8.0
   ```

2. Configure `hbase-site.xml`:
   ```bash
   sudo nano conf/hbase-site.xml
   ```
   Add:
   ```xml
   <configuration>
     <property>
       <name>hbase.tmp.dir</name>
       <value>/var/hbase</value>
     </property>
   </configuration>
   ```

## 5. Start HBase

Start the HBase server:
```bash
sudo ./bin/start-hbase.sh
```

## 6. Using HBase Shell

1. Start HBase shell:
   ```bash
   ./bin/hbase shell
   ```

2. Basic HBase commands:
   ```bash
   # List tables
   list
   
   # Create table
   create 'users', 'info'
   
   # Insert data
   put 'users', '1', 'info:name', 'John Doe'
   
   # Scan table
   scan 'users'
   
   # Get specific row
   get 'users', '1'
   ```

## Troubleshooting

- If you encounter permission issues, ensure you have the correct permissions on the HBase directory
- Check logs in the `logs` directory for any errors
- Verify that Java is properly installed and JAVA_HOME is correctly set
- Make sure all required ports are open and not being used by other services

## Additional Resources

- [HBase Documentation](https://hbase.apache.org/book.html)
- [HBase Shell Commands Reference](https://hbase.apache.org/book.html#shell)
