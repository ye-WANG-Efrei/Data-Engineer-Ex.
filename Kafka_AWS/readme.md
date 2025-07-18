##Download and Setup Kafka - Linux(AWS)   
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
Stable in AWS and cloud environments ✅

Fully compatible with Apache Kafka ✅

Therefore, many Kafka installation guides—especially those on AWS—recommend or use Corretto as the JDK runtime environment by default.

Start Kafka using the binaries in another process

Setup the $PATH environment variables for easy access to the Kafka binaries
3. 
