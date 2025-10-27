# Java Web Application on Tomcat using AWS-RDS(PAAS)

**Project Overview**

This project demonstrates deploying a Java Web Application (LoginWebApp.war) using Maven, Apache Tomcat, and AWS RDS MySQL. The system allows users to register and login, storing user details in a MySQL database hosted on AWS RDS.

**1. Setup AWS EC2 Instances**
    OS: Ubuntu 22.04 LTS
    Instance type: t3.micro
    Key pair: data.pem
    Security group: allow SSH (22)

<img width="1887" height="933" alt="Image" src="https://github.com/user-attachments/assets/3c883d6d-9118-4588-9820-1a0e0b3f934c" />

<img width="1911" height="798" alt="Image" src="https://github.com/user-attachments/assets/d225ab4e-3712-4ddd-99f6-8e9bc76e9974" />

<img width="1918" height="797" alt="Image" src="https://github.com/user-attachments/assets/6b6128cc-a224-416d-8fb1-90fbdcb095c4" />

**2. Connect to Build Server**
   ssh -i data.pem ubuntu@<BUILD_EC2_IP>

<img width="1473" height="620" alt="Image" src="https://github.com/user-attachments/assets/b3f2a9e4-a626-49d3-8d50-110e2d9d61d2" />

**3. Check if Java & Maven are installed**
   java -version
   mvn -version
   Verify output shows installed versions

<img width="1467" height="758" alt="Image" src="https://github.com/user-attachments/assets/bd5636c3-f7ab-4ce0-976d-1ea1d644832b" />

**4. Install Java & Maven if not present**
   sudo apt update
   sudo apt install default-jdk maven -y
   java -version
   mvn -version

<img width="1476" height="752" alt="Image" src="https://github.com/user-attachments/assets/08db7961-c054-4fc1-b32b-bb137aafa7db" />

**5. Clone the Project from GitHub**
   git clone <your-repo-link>
   cd aws-rds-java

<img width="1461" height="752" alt="Image" src="https://github.com/user-attachments/assets/73e412d3-c34b-43c1-b969-ec3da79686dd" />

**6. Build the Project**

<img width="1467" height="761" alt="Image" src="https://github.com/user-attachments/assets/900b4556-84ca-4658-86ee-a340ef503e93" />

**7. mvn package**

<img width="1475" height="756" alt="Image" src="https://github.com/user-attachments/assets/b5d1a28b-37f1-4657-935d-92d1bc899068" />

**8. After building, verify WAR file: target/LoginWebApp.war**

<img width="1483" height="582" alt="Image" src="https://github.com/user-attachments/assets/5903cb35-bfd3-40bb-b6e1-fb9e5e7d3fc1" />


**B. Deploy Server (EC2) Setup**
**1.Launch Deploy EC2 Instance**
OS: Ubuntu 22.04 LTS
Instance Type: t3.micro
Key Pair: Same data.pem
Security Group: Allow SSH (22), Tomcat (8080).

<img width="1900" height="865" alt="Image" src="https://github.com/user-attachments/assets/b018e048-da03-4cd5-8f45-05c47690d572" />

<img width="1896" height="870" alt="Image" src="https://github.com/user-attachments/assets/8e92586c-1544-40eb-96dd-fd1d639fb175" />

<img width="1893" height="852" alt="Image" src="https://github.com/user-attachments/assets/ffd34c57-f4d8-4f63-8ad9-d8e96efcd849" />

**2. Connect to Deploy Server**
     ssh -i data.pem ubuntu@<DEPLOY_EC2_IP>

<img width="1470" height="756" alt="Image" src="https://github.com/user-attachments/assets/be141fc8-cb0f-408d-b97b-06e69e8457f5" />

**3. Install Java**
sudo apt update
sudo apt install default-jdk -y
java -version

<img width="1472" height="757" alt="Image" src="https://github.com/user-attachments/assets/bd08a9c4-daf2-4435-9d32-1b77f2fb9cda" />

**4. Install Apache Tomcat**
   Download latest Tomcat using wget:

<img width="1905" height="937" alt="Image" src="https://github.com/user-attachments/assets/5fcf7056-569f-4340-90b6-faf4e2e30d07" />

<img width="1466" height="752" alt="Image" src="https://github.com/user-attachments/assets/c520c905-d843-4ed5-a31a-6044eaf417df" />

**5. Extract Apache Tomcat**
     tar -xvzf apache-tomcat-9.x.xx.tar.gz

<img width="1467" height="747" alt="Image" src="https://github.com/user-attachments/assets/f438268e-f695-4f69-8c0e-93cfea01289b" />

**6. Start Apache Tomcat**
      cd apache-tomcat-9.x.xx/bin
     ./startup.sh

<img width="1481" height="761" alt="Image" src="https://github.com/user-attachments/assets/a3200051-6b93-4cc0-a99f-192c619d102f" />
 
**7. Verify Tomcat is running**

<img width="1917" height="968" alt="Image" src="https://github.com/user-attachments/assets/15eb1354-13e3-4aa3-b227-8c560f70d9df" />

   Connect  to manager app

<img width="1435" height="632" alt="Image" src="https://github.com/user-attachments/assets/5d3bc92f-b7ff-4dd7-9f54-0a07f4640294" />

    we get 403 error
    **Edit Tomcat Context File**

<img width="1477" height="762" alt="Image" src="https://github.com/user-attachments/assets/f04fd4a8-ab3e-4ebf-8a54-56e055677e84" />

   Edit conf/context.xml to remove any access restrictions for manager GUI.

<img width="1471" height="747" alt="Image" src="https://github.com/user-attachments/assets/b3bc17fd-7023-42af-b81a-c28fbcc9f0c3" />

   **Edit Tomcat Users File**
   Edit conf/tomcat-users.xml and add:

<img width="1477" height="446" alt="Image" src="https://github.com/user-attachments/assets/e6b595c3-25f1-4450-a209-018be5b309e1" />

   edit and add user credentials
   <role rolename="manager-gui"/>
  <user username="admin" password="admin123" roles="manager-gui"/>

<img width="1483" height="775" alt="Image" src="https://github.com/user-attachments/assets/4186abb2-87c6-49b7-bef0-181a46fc5bcd" />

  **Restart Tomcat:**
  ./shutdown.sh
  ./startup.sh

<img width="1911" height="815" alt="Image" src="https://github.com/user-attachments/assets/89e6bd9b-c434-4c79-96e4-bf807660f7b8" />

**C. AWS RDS MySQL Setup**

**1. Create VPC and Security Group**
   Allow port 3306 for MySQL

<img width="1905" height="870" alt="Image" src="https://github.com/user-attachments/assets/496e04cf-64cd-4b4e-9f34-0a9b53b8ed4d" />

**2. Create RDS MySQL Instance**
   Database Name: jwt
   Username: admin
   Password: admin123
   Attach to VPC created above

<img width="1917" height="923" alt="Image" src="https://github.com/user-attachments/assets/b90394dd-2cfe-4a30-a213-bfd9c1ccf604" />

**3. Install mysql in deploy server**

<img width="1478" height="707" alt="Image" src="https://github.com/user-attachments/assets/51b9ece8-a50b-4c40-a2bd-5b71df240d1a" />

**4. Connect to RDS MySQL via CLI**
By using a command--mysql -h <your-rds-endpoint> -u admin -p

<img width="1475" height="767" alt="Image" src="https://github.com/user-attachments/assets/63eaea89-2aab-4bd9-a600-3dab9dfa0659" />

**5. After connecting to mysql, Create Table Schema**

<img width="1482" height="757" alt="Image" src="https://github.com/user-attachments/assets/36dc474a-eff5-4c9d-888d-5162275c96ef" />

 **D. Update Application Code**
     Edit both login.jsp and userRegistration.jsp to replace
     Update database credentials:
     String jdbcURL = "jdbc:mysql://<your-rds-endpoint>:3306/jwt?useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=UTC";
     String dbUser = "admin";
     String dbPass = "admin123";
     Rebuild WAR

<img width="1482" height="762" alt="Image" src="https://github.com/user-attachments/assets/ed0b3bdc-6c89-4023-a2c4-de55d7df3574" />

 **E. Deploy WAR to Tomcat**
     Copy WAR to Deploy Server

<img width="1462" height="225" alt="Image" src="https://github.com/user-attachments/assets/c0df2db2-7a8f-4744-8841-abf4f7f3b779" />

 Set Permissions on WAR

<img width="1475" height="756" alt="Image" src="https://github.com/user-attachments/assets/ad0cd49a-40dd-4b3a-95f9-d0dee8261362" />

**1. Deploy  your Java web application (LoginWebApp.war) to the Tomcat server running on another EC2 instance.**

By using the command: scp -i data.pem /home/ubuntu/aws-rds-java/target/LoginWebApp.war ubuntu@<Deploy IP>:/home/ubu
ntu/apache-tomcat-9.0.110/webapps/

<img width="1500" height="781" alt="Image" src="https://github.com/user-attachments/assets/49a47c0a-8d1d-4837-b6d1-151d896ffee1" />

**2. Verify Deployment**
    Open browser: http://<DEPLOY_EC2_IP>:8080/manager/html
    Open App: http://<DEPLOY_EC2_IP>:8080/LoginWebApp
    You can see your loignpage in manager application

<img width="1922" height="975" alt="Image" src="https://github.com/user-attachments/assets/1e26c9b8-a13d-4b69-b2cc-2f8c89a022ed" />

  Test Application
  Register a user and login.

<img width="1912" height="980" alt="Image" src="https://github.com/user-attachments/assets/bf3cd798-c080-4cd8-82aa-4afd9a1a551f" />

<img width="1918" height="967" alt="Image" src="https://github.com/user-attachments/assets/c79a60d1-9e10-4279-95a0-7aa4d881ab59" />

 **3. Verify stored data in RDS:**
 SELECT * FROM USER;

<img width="1488" height="766" alt="Image" src="https://github.com/user-attachments/assets/aa83408e-ec8b-48a0-9547-ad243313a33f" />
 













 




