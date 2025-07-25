# Kafka with AWS
## Download and Setup kafka_2.13-3.9.1 with Zookeeper - Linux(AWS)   
### Setup JAVA enviroment - Kafka is a Java-based system
1.  Install Java JDK version 11 
2.  For AWS there is an open-souce App - `Amazon Corretto` 
     - Open-source ✅
     - Free of charge ✅
     - Long-term supported ✅  
     - Therefore we use `Corretto` as the JDK runtime environment by default.
     - [ Amazon Corretto 11 Linux install page](https://docs.aws.amazon.com/corretto/latest/corretto-11-ug/linux-info.html)  
         ```
         # grab the pb key from link, and save that in following path     
          wget -O - https://apt.corretto.aws/corretto.key | sudo gpg --dearmor -o /usr/share/keyrings/corretto-keyring.gpg
         
         #将 Amazon Corretto 的软件源添加到你的系统中，以便可以通过 apt 命令直接安装它提供的软件包（比如 Corretto JDK）。  
         #deb <网址> <发布版本名> <组件> 
          sudo add-apt-repository deb https://apt.corretto.aws stable main
         
         # 如果没有add-apt-repository use the following code to update
          sudo apt-get update; sudo apt-get install -y java-11-amazon-corretto-jdk
         ```  
      which is going to enbale us to install Java 11 from the Amazon distribution
 3.  Amazon Linux / CentOS / RHEL , they don't have `apt`, use `yum` or `dnf`.  
     ```
       sudo dnf install java-11-amazon-corretto  
       ```

 4. Verify whether or not Java is correctly installed  
       ```
       java -version  
       ```
     - if you already have another Java version and it is not a right verions, you can choose which version you want to use. Don't fogget reboot the terminal.  
       ```
       sudo  update-alternatives -- config java
       ```
    
### Install Apache Kafka  
1. Download the latest version of Apache Kafka from https://kafka.apache.org/downloads under Binary downloads. （Binary .tgz文件 免编译，不用构建Maven）
2. Extract the contents to a directory of your choice
   ```
   tar xzf kafka_2.13-3.9.1.tgz  
   mv kafka_2.13-3.9.1 ~
   ```
3. Setup the $PATH environment varibale, which could let you input the more concise commend instead of the whole path
   In order to easily access the Kafka binaries, you can edit your PATH variable by adding the following line to your system run commands (for example `~/.zshrc` if you use zshrc or `~/.bashrc` for Bash ):
   ```
   nano ~/.bashrc                     # access to config file
   export PATH="$PATH:~/kafka_2.13-3.9.1/bin"
   source ~/.bashrc                   # save the change forever
   echo $PATH                         # show that indeed path into the account
   ```
   Maker sure you have the correct Kafka version and path right here.
4. ###### Remarque
   如果 PATH 被你意外覆盖了（只包含了 Kafka 路径），你可能看到类似：
   ```
   /home/ec2-user/kafka_2.13-3.9.1/bin
   ```
   这样的话，系统就找不到 `dirname`、`java`、`yum`、甚至 `ls`、`sudo` 这些命令了。
   如何正确恢复 PATH
   先手动恢复一个最基本的 PATH 临时用：
   ```
   export PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
   ```
   然后再把 Kafka 的路径加上：
   ```
   which dirname
   which kafka-server-start.sh
   ```
   

   
### Lanch Kafka with Zookeeper  
1. Start Zookeeper using the binaries
   ```
   zookeeper-server-start.sh  ~/kafka_2.13-3.9.1/config/zookeeper.properties
   
   # If you did indecate the PATH of zookeeper-server-start.sh, if not,do the following
   ~/kafka_2.13-3.9.1/bin/zookeeper-server-start.sh ~/kafka_2.13-3.9.1/config/zookeeper.properties
   ```
   This command does two things: it starts the Zookeeper server and indicate it where to find the properties file.
   它是一个 属性文件（.properties）格式的文本配置文件，里面定义了 Zookeeper 启动时所需的各种关键参数。
   ```
   dataDir=/tmp/zookeeper              # 存储数据的目录
   clientPort=2181                    # 客户端连接端口
   maxClientCnxns=60                  # 最大客户端连接数
   tickTime=2000                      # 心跳单位（ms）
   initLimit=10                       # 集群初始化时的心跳周期数
   syncLimit=5                        # 同步时的心跳容忍数
   ```
2. Start Kafka using the binaries in another process
   ```
   ~/kafka_2.13-3.9.1/bin/kafka-server-start.sh ~/kafka_2.13-3.9.1/config/server.properties
   ```
3. You could also change the zookeeper data storage in the properties file
   ```
   @zookeeper.properties
   # the dircatory where the snapshot is stored.
   dataDir = /temp/zookeeper
   ```
4. Change the Log file in server.properties file
   ```
   # A comma separated list of directories under which to store log files
   log.dirs = /temp/kafka-logs
   ```
### Summary & Impove
1. Linux上用apt, 但是AWS上直接用yum.
2. 正确的 .bashrc 设置应该是：
   ```
   export PATH="~/kafka_2.13-3.9.1/bin"    # ❌ 错！覆盖了系统路径
   # 保持系统原有路径，并添加 Kafka 路径
   export PATH="$PATH:$HOME/kafka_2.13-3.9.1/ sebin"
   ```
   只有`source ~/.bashrc`后才会永久保存，同时要写进`~/.bashrc`才会对所有process生效
3. 如果不小心覆盖了`$PATH`,就没有办法使用`sudo`,`nano`,`ls`等命令  
   [you can see there](#Remarque)
4. #### How can I create a new EC2 instance using my previous setup or environment?
   We can create an Amazon Machine Image (AMI) as what we do on docker  
   'Actions' → 'Image and templates' → 'Create image'

## Start Kafka in KRaft(wihtout Zookeeper) - Linux(AWS)    
1. why we DEPRECATE(Obsolète) Zookeeper?  
   a. prevent 'Split-Brain': Based on the architecture of Zookeeper, the `controller` is selected by Zookeeper.
   It is possible there may be multiple `controller` identifying as `Master` simultaneously, **leading to data inconsistency.**

   b. ZooKeeper has ALL configuration of Kafka, However,Due to ZooKeeper's limited support for encryption mechanisms such as TLS（Transport Layer Security） and SASL（Simple Authentication and Security Layer）, **it can become a potential attack vector, even when Kafka is well secured.**
   
2. MUST: Grenerate a cluster ID and Format the storage using `kafak-storage.sh` to initialize the **metadata directory**; otherwise, Kafka will not be able to start."  
    > WHY?
    >When using KRaft, Kafka itself stores all metadata instead of relying on ZooKeeper. This means:  
    > 1. All topics, broker IDs, configurations, and ISR (in-sync replica) states are written into an internal metadata log.  
    > 2. Kafka must be aware of which cluster the metadata belongs to.  
    > 3. Therefore, 、 must manually initialize the metadata directory and assign a unique Cluster ID before starting the broker.

3. Installing Java JDK 11
4. Download Apache Kafka from https://kafka.apache.org/downloads under Binary Downloads
5. Start Kafka
   - the first step is to genertate a new ID for the cluster  
     ```
     kafka-storage.sh random-uuid
     ```
   - Next,format your storage directory(replace <uuid> by your uuid obtained above)
     ```
     kafka-storage.sh format -t <uuid> -c ~/kafka_2.13-3.0.0.0/config/kraft/sesrver.properties
     ```  
     this will format the directory that is in the `log.dirs` in the `config/kraft/server.properties` file by default `/temp/kraft-combined-logs`)
