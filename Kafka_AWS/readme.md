# Kafka with AWS
## Download and Setup Kafka - Linux(AWS)   
### Setup JAVA enviroment - Kafka is a Java-based system
1.  Install Java JDK version 11 
2.  For AWS there is an open-souce App - `Amazon Corretto` 
     - Open-source ✅
     - Free of charge ✅
     - Long-term supported ✅  
     - Therefore we use `Corretto` as the JDK runtime environment by default.
     - [ Amazon Corretto 11 Linux install page](https://docs.aws.amazon.com/corretto/latest/corretto-11-ug/linux-info.html)  
         ```
          wget -O - https://apt.corretto.aws/corretto.key | sudo gpg --dearmor -o /usr/share/keyrings/corretto-keyring.gpg && \  
          sudo add-apt-repository 'deb https://apt.corretto.aws stable main    
          sudo apt-get update; sudo apt-get install -y java-11-amazon-corretto-jdk
         ```  
      which is going to enbale us to install Java 11 from the Amazon distribution
  3. Verify whether or not Java is correctly installed  
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
   tar xzf kafka_2.13-3.0.0.tgz  
   mv kafka_2.13-3.0.0 ~
   ```
3. Setup the $PATH environment varibale, which could let you input the more concise commend instead of the whole path
   In order to easily access the Kafka binaries, you can edit your PATH variable by adding the following line to your system run commands (for example `~/.zshrc` if you use zshrc or `~/.bashrc` for Bash ):
   ```
   nano ~/.bashrc                     # access to config file
   PATH="$PATH:~/kafka_2.13-3.0.0/bin"
   source ~/.bashrc                   # save the change
   echo $PATH                         # show that indeed path into the account
   ```
   Maker sure you have the correct Kafka version and path right here.

### Lanch Kafka with Zookeeper  
1. Start Zookeeper using the binaries
   ```
   zookeeper-server-start.sh  ~/kafka_2.13-3.0.0/config/zookeeper.properties
   
   # If you did indecate the PATH of zookeeper-server-start.sh, if not do the following
   ~/kafka_2.13-3.0.0/bin/zookeeper-server-start.sh ~/kafka_2.13-3.0.0/config/zookeeper.properties
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
   ~/kafka_2.13-3.0.0/bin/kafka-server-start.sh ~/kafka_2.13-3.0.0/config/server.properties
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


