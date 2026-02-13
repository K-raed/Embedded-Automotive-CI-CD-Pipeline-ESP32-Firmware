README_FILE_1.0(system installation steps)

1. setup your Virtual machine with a Linux system (Ubuntu, Debian, CentOS or Arch distribution)

2. install your Jenkins by following these steps: 
   1- JAVA:
      open your terminal and run :
        sudo apt update
        sudo apt install default-jdk
        java --version 
      after these commands, you must see your jdk version by the latest command 
      then run 
        sudo nano /etc/environment
      add this line to the file opened in the editor 
        JAVA_HOME="/usr/lib/jvm/java-11-openjdk-amd64/"
      CTRL+s CTRL+x to save and exit
      apply changes by :
        source /etc/environment
      verify the configuration by:
        echo $JAVA_HOME
      you must see the path you added : /usr/lib/jvm/java-11-openjdk-amd64/
   2- MAVEN:
      open your terminal and run :
        sudo apt install maven -y
        M2_HOME="opt/apache-maven-3.6.3"
        PATH="$M2_HOME/bin:$PATH"
        export PATH
      verify the installation by:
        mvn -version
      if you see your maven version run:
        sudo nano /etc/environment
      then add this line : 
        M2_HOME="opt/apache-maven-3.6.3"
      CTRL+s CTRL+x to save and exit
   3- GIT:
      open your terminal and run :
        sudo apt install git
      verify the installation by:
        git --version
   4- JENKINS:
      open the terminal and run :
          wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
          sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/etc/apt/sources.list.d/jenkins.list'
          sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 5BA31D57EF5975CA
          sudo apt update
          sudo apt install jenkins
          sudo systemctl start jenkins
          sudo systemctl enable jenkins
          wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
          sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ >
/etc/apt/sources.list.d/jenkins.list'
          sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 5BA31D57EF5975CA
          sudo apt update
          sudo apt install jenkins
          sudo systemctl start jenkins
          sudo systemctl enable jenkins

      or just follow the steps on the official Jenkins website:
        https://www.jenkins.io/doc/book/installing/linux/#debianubuntu

3. now you must switch to Jenkins user in your VM terminal to proceed with the following steps properly 
      run these commands:
        sudo -i
        sudo visudo
      add this line:
        jenkins ALL=(ALL) NOPASSWD:ALL
      CTRL+s CTRL+x to save and exit
      then run 
      sudo -i -u jenkins
      printenv   to check environment variables 

4. now you are ready to install the esp-idf framework by following the steps mentioned on the official website:
   https://docs.espressif.com/projects/esp-idf/en/stable/esp32/get-started/linux-macos-setup.html

5. install docker by running :
      sudo apt-get install ca-certificates curl gnupg
      sudo install -m 0755 -d /etc/apt/keyrings
      curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o
      /etc/apt/keyrings/docker.gpg
      sudo chmod a+r /etc/apt/keyrings/docker.gpg
      echo \
      "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg]
      https://download.docker.com/linux/ubuntu \
      "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
      sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
      sudo apt-get update
      sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin
      docker-compose-plugin
      sudo service docker start
      sudo service docker enable
      
6. pull the docker image by running: 
      docker pull espressif/idf
   then you can run your pipeline stages that are responsible for building containers and building code 

7. pull the nexus image and run the nexus container 
      docker pull sonatype/nexus3
      docker run -d -p 8081:8081 --name nexus sonatype/nexus3

8. add the nexus stage to upload your artifact

9. for static code test you can integrate sonarqube Developer Edition(paid version)
   or you can integrate other static analyses tools like Cppcheck, Flawfinder or Splint
   but the output report will be in html format.

10. for mqtt connection install mqtt mosquito on your vm and you can create public broker with emqx 
    emqx website: https://www.emqx.com/en
    mosquitto installation: https://mosquitto.org/download/

*****for secure connection (mqtts) you must download the ssl/tls certification to use it in the esp code   

11. if you work with localhosted VM and you need to expose your artifactory ip you need to configure endpoint domain with ngrok  
    ngrok website: https://dashboard.ngrok.com/domains

*************************************************************************************************************
  **always after pushing new directory into github repository make sure to make those commands :
sudo chown -R jenkins:jenkins /var/lib/jenkins/workspace/you_pipeline_name
sudo chmod -R 755 /var/lib/jenkins/workspace/you_pipeline_name
  and pull if you make changes before your first build
*************************************************************************************************************
      
       
      
      

      
