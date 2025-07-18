##Download and Setup Kafka - Linux(AWS)   
1. Setup JAVA enviroment - Kafka is a Java-based system
  1. Install Java JDK version 11
  2. For AWS there is an open-souce App - `Amazon Corretto`
     - Open-source ✅
     - Free of charge ✅
     - Long-term supported ✅  
     - Therefore we use `Corretto` as the JDK runtime environment by default.
     - [ Amazon Corretto 11 Linux install page](https://docs.aws.amazon.com/corretto/latest/corretto-11-ug/linux-info.html)
     - `wget -O - https://apt.corretto.aws/corretto.key | sudo gpg --dearmor -o /usr/share/keyrings/corretto-keyring.gpg && \ `  
        `sudo add-apt-repository 'deb https://apt.corretto.aws stable main`  
        `sudo apt-get update; sudo apt-get install -y java-11-amazon-corretto-jdk`  
         which is going to enbale us to install Java 11 from the Amazon distribution 


Stable in AWS and cloud environments ✅

Fully compatible with Apache Kafka ✅

Therefore, many Kafka installation guides—especially those on AWS—recommend or use Corretto as the JDK runtime environment by default.

Start Kafka using the binaries in another process

Setup the $PATH environment variables for easy access to the Kafka binaries
3. 
