# TASK-8-Run-a-Simple-Java-Maven-Build-Job-in-Jenkins


# Objective:
- Build and package a basic Java app using Maven in Jenkins running inside Docker.
#  Tools Required:
- Ec2 instance (ubuntu)
- Docker
- Pull the Jenkins from docker hub
# install docker 
- sudo apt update
- sudo apt install -y docker.io -y
- sudo systemctl enable docker
- sudo systemctl start docker
- docker --version
- docker pull jenkins/jenkins
# Start Jenkins using Docker
```sh
docker run -d --name jenkins \
  -p 8080:8080 -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  jenkins/jenkins:lts
```
- -d = detached
- Jenkins UI will be at http://localhost:8080


![image](https://github.com/user-attachments/assets/481f6c11-1f46-4825-86ab-7abc84af3f9c)

# Unlock Jenkins
- Check the admin password:
    - docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
    - Copy that password → Open browser → http://localhost:8080 → paste it
    - Click: Install Suggested Plugins
        - Stage view
        - Maven integration
        - Github integration
# Create Java Project in Docker 
- docker exec -it jenkins /bin/bash
- apt update -y
- git clone <url> ---- this is your wish I used a github for mvn build
-	mkdir -p /var/jenkins_home/hello-java-maven/src/main/java
-	cd /var/jenkins_home/hello-java-maven/src/main/java
-	Create HelloWorld.java:
```sh
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, Jenkins + Maven!");
    }
}
```
- Create pom.xml in /var/jenkins_home/hello-java-maven/:
```sh
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.example</groupId>
  <artifactId>hello</artifactId>
  <version>1.0</version>
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.8.1</version>
        <configuration>
          <source>1.8</source>
          <target>1.8</target>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>
```
- I send the code into git hub repo
# Create Freestyle Jenkins Job


![image](https://github.com/user-attachments/assets/6f91d447-6686-4d29-acbe-3385ec20d817)


- Go manage Jenkins----> tools----> search maven installations---> add maven--->version 3.8.6 ----> save and apply
- Jenkins UI → New Item → Name: hello-java-maven → Freestyle Project
- Go to configuration----> source code management -----> code is present in locally choice none or for git means give <url>-----> branches to build main

  ![image](https://github.com/user-attachments/assets/32236b1c-01e0-4db8-9f2f-60792438b33b)


![image](https://github.com/user-attachments/assets/bc5e104b-b37c-4c53-bb42-721c6817bf9c)


- Search the Build steps----> give the maven version---> goals (clean package) ------>add advanced--------> In POM give the path of pom.xml file other show an error for build time ---> save and apply

![image](https://github.com/user-attachments/assets/e4b176c9-9dda-4576-957f-4ac4c04e1392)


- Execute the build now

![image](https://github.com/user-attachments/assets/7f0fdbb3-7532-4b70-b5c4-f0379aef1833)


- Build is successfully

![image](https://github.com/user-attachments/assets/e2634eee-865d-4210-9d04-e0487fa7c846)


- Finally check the target file and .war or jar file
    - Go to job----> workspace---> check hello-java-maven----> target file

![image](https://github.com/user-attachments/assets/b80ec651-9a86-454f-af10-8cb187210a10)

       
  -  Finally check the .jar file inside the target dir

![image](https://github.com/user-attachments/assets/bd396609-86fd-4121-ac26-8b7b2ef290d8)



#   THANK YOU
