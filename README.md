# AMI 
```
Amazon Machine Language
```

# Techstack
```
Amazon Web Services
Hashicorp Packer
JSON
```

## Assignment 3: How to Demo?
### 1. Git Demo:

1. Create pull requests between
```
1. Main branch of organization and assignment branch of fork.
2. Main branch of organization and main branch of fork.
3. Main branch of fork and assignment branch of fork.
```

There should be nothing to compare.

2.  TAs and instructors are collaborators to the GitHub repository.
```
https://github.com/orgs/csye6225org/people
```

3. Show 'README.md' file
```
https://github.com/csye6225org/webapp
https://github.com/csye6225org/infrastructure
https://github.com/csye6225org/ami
```

4. Show that repository is cloned correctly.
   Execute the following commands in terminal.
```
# cd /home/varad/Desktop/NSC

# cd /home/varad/Desktop/NSC/Github/webapp/
# git remote -v

# cd /home/varad/Desktop/NSC/Github/infrastructure/
# git remote -v

# cd /home/varad/Desktop/NSC/Github/ami/
# git remote -v
```

### 2. Demo Building AMIs & Launch EC2 Instance

1. Build infrastructure

```
# cd /home/varad/Desktop/NSC/Github/infrastructure

# export AWS_PROFILE=prod

# terraform apply
```

2. Edit subnet_id in 'build.sh' file

```
# vi /home/varad/Desktop/NSC/Github/ami/buildAmi.sh
```

3. Build AMI

```
# cd /home/varad/Desktop/NSC/Github/ami

# ./buildAmi.sh
```

4. Launch EC2 instance

```
Things to remember:
1. Instance type: t2-micro

--> Next

1. Set Vpc to the one created from your infrastructure.
2. Set Subnet to the one created from your infrastructure.
3. Auto-assign Public IP: Enable

--> Next
--> Next
--> Next

1. Security Group:
    Http -> Anywhere Ipv4
    SSH  -> Anywhere Ipv4
    Custom TCP -> 8080 -> Anywhere Ipv4

--> Next

1. Select ssh key 
    Name -> aws_prod_ssh_key

--> Launch

```

5. SSH into the EC2 Instance from terminal

```
# ssh -i  ~/.ssh/aws_prod_ssh_key ubuntu@<Public_Ipv4_address>

# exit
```

### 3. Demo Webapp from EC2 instance

1. Generate Executable WAR of webapp

```
# cd /home/varad/Desktop/NSC/Github/webapp/project

# export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
# export PATH=$JAVA_HOME/bin:$PATH

# mvn clean install
```

2. Send WAR file to EC2 instance

```
# cd /home/varad/Desktop/NSC/Github/webapp/project/target

# scp -i ~/.ssh/aws_prod_ssh_key /home/varad/Desktop/NSC/Github/webapp/project/target/ROOT.war ubuntu@<Public_Ipv4_address>:/home/ubuntu/
```

3. SSH into the EC2 Instance from terminal

```
# ssh -i  ~/.ssh/aws_prod_ssh_key ubuntu@<Public_Ipv4_address>
```

4. Setup Postgresql Database

```
# pg_lsclusters

# sudo su postgres

# psql

# CREATE USER varad WITH PASSWORD 'Varad@123';

# ALTER USER varad WITH SUPERUSER;

# CREATE DATABASE rdb;

# exit

# exit
```

5. Stop tomcat 
```
sudo systemctl stop tomcat9
```

6. Run executable war file
```
# cd /home/ubuntu/

# java -jar ROOT.war 
```

### 4. Web Application Demo


1. Open Postman.
   5.1. Workspace 'Csye6225'
   5.2. Folder 'webapp_on_aws'
2. First we will demo POST request.

2.1. URL: 
```
http://<Public_Ipv4_address>:8080/v1/user
```
2.2. Request Body:
```
{
    "first_name": "Gargi",
    "last_name": "Desai",
    "password": "Gargi@123",
    "username": "gargi@gmail.com"
}
``` 
2.3. Click on send. Response to expect.
```
201 Created

{
    "firstName": "Gargi",
    "lastName": "Desai",
    "account_created": "2021-10-06T17:07:29.776Z",
    "emailId": "gargi@gmail.com",
    "id": "4fe8360c-9a31-4488-a2fa-80e18c4235be",
    "account_updated": "2021-10-06T17:07:29.776Z"
}
```
After getting the response, open the database and execute the following query. Then show that 
1. Password is encrypted
2. id, account_created and account_updated fields are getting populated automatically.

*Do the following in New terminal window:*
```
# ssh -i  ~/.ssh/aws_prod_ssh_key ubuntu@<Public_Ipv4_address>
# sudo su postgres
# psql
# \l
# \c rdb
# SELECT * FROM "user";
```

2.4. Click on send once more, Response to expect.
```
400 Bad Request

User Already Exists
```

2.6. Show that Non email username does not get created.
Request Body:
```
{
    "first_name": "Gargi",
    "last_name": "Desai",
    "password": "Gargi@123",
    "username": "gargigmailcom"
}
```
Response to expect:
```
400 Bad Request

Username is not a valid Email
```

3. Second we will demo GET request

URL:
```
http://<Public_Ipv4_address>:8080/v1/user/self
```
Authorization:
```
Select 'Basic Auth'
Username: gargi@gmail.com
Password: Gargi@123
```
Response to expect:
```
200 OK

{
    "firstName": "Gargi",
    "lastName": "Desai",
    "account_created": "2021-10-06T17:07:29.776Z",
    "emailId": "gargi@gmail.com",
    "id": "4fe8360c-9a31-4488-a2fa-80e18c4235be",
    "account_updated": "2021-10-06T17:07:29.776Z"
}
```
3.1. Change Username and click send. Response to expect.
```
400 Bad Request

User dont Exists
```
Correct the username.

3.2. Change Password and click send. Response to expect.
```
400 Bad Request

Invalid Password
```

4. Third, we will demo PUT request.

URL:
```
http://<Public_Ipv4_address>:8080/v1/user/self
```
Request Body:
```
{
    "first_name": "Gargi Ratnakar",
    "last_name": "Desai",
    "password": "Gargi@123",
    "username": "gargi@gmail.com"
}
```
Authorization:
```
Select 'Basic Auth'
Username: gargi@gmail.com
Password: Gargi@123
```

4.1. Click on send, Response to expect.
```
200 OK

{
    "firstName": "Gargi Ratnakar",
    "lastName": "Desai",
    "account_created": "2021-10-06T17:07:29.776Z",
    "emailId": "gargi@gmail.com",
    "id": "4fe8360c-9a31-4488-a2fa-80e18c4235be",
    "account_updated": "2021-10-06T17:07:29.776Z"
}
```
4.2. Change Username and click send. Response to expect.
```
400 Bad Request

User dont Exists
```
Correct the username.

4.3. Change Password and click send. Response to expect.
```
400 Bad Request

Invalid Password
``` 
4.4. Showcase handling update to fields other than firstname, lastname, password. 
```
{
    "first_name": "Gargi Ratnakar",
    "last_name": "Desai",
    "password": "Gargi@123",
    "username": "gargi@gmail.com",
    "account_created": "2021-10-06T17:07:29.776Z"

}

Utility strings:  
"account_updated": "2021-10-06T17:07:29.776Z"
"id": "4fe8360c-9a31-4488-a2fa-80e18c4235be"
```
Response to expect:
```
400 Bad Request

You cannot update fields other than Firstname, Lastname and Password.
``` 

### 4. Do Cleanup
 - Go to Terminal
 - Press Ctrl + C, to close webapp
 - Then execute the following command
```
# exit
```
- Further, go to AWS console and do the following:
  - Terminate Instance
  - Delete security group
  - Then execute the following commands in terminal
```
# cd /home/varad/Desktop/NSC/Github/infrastructure

# terraform destroy

# export AWS_PROFILE=
```
