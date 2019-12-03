# AWS-database
## "If you need a relational database, you need RDS!"
## please folow the easy steps hereunder
- AWS/RDS
- create database
- standard
- chose engine like mySQL
- chose freeTier option
- create a DB instance name : myfirstdatabase
- create a mastername and masterpassword: JeanM / JeanM1234
- check options
  - connectivity options: 
    - here to have access from an instance outside EC2, please consider "PUBLIC"
    - otherwise you will need to connect through a EC2 that you will create
- chose the appropriate VPC
  - your VPC should have some particular design: 2 AZ, 2 Subnets
  - one option is to chose "create a VPC"
  - create a DB Subnet group
  - create a DB security group: DB SG group
- launch (and wait 5 minutes)
- check you can connect using Toad
  - install Toad: https://www.techrepublic.com/blog/tr-dojo/manage-mysql-databases-from-a-windows-desktop-with-toad/
  - create a new connection in Toad:
    - DOES NOT SEEM TO WORK
  - or use mySQL workbench: https://dev.mysql.com/downloads/file/?id=490464
    - end point: myfirstdatabase2.c4md1iztmtjn.us-east-1.rds.amazonaws.com
    - use your username and passwork credentials
  - you can also check this usefull link: https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ConnectToInstance.html
  
- in your VPC
  - check that the security rule of the DB instance meets a private route table
 
 #create an IAM role credentials to connect to the database
 see the link: https://aws.amazon.com/premiumsupport/knowledge-center/users-connect-rds-iam/
 
 ## create a connection from python
 - follow these doc: https://dev.mysql.com/doc/connector-python/en/connector-python-coding.html
 - prepare your EC2 instance with the following code
 ```#update of the instance
    2  sudo yum update -y

#install of mysql server
    9  sudo yum install mysql-server

#connection to "myfirstdb" using mysql
   11  mysql -h myfirstdb.c4md1iztmtjn.us-east-1.rds.amazonaws.com -P 3306 -u JeanM -p

#creation of the token to add credentials
   13  aws rds generate-db-auth-token --hostname myfirstdb.c4md1iztmtjn.us-east-1.rds.amazonaws.com --port 3306 --username JeanM
   14  wget https://s3.amazonaws.com/rds-downloads/rds-ca-2019-root.pem
   19  mysql -h myfirstdb.c4md1iztmtjn.us-east-1.rds.amazonaws.com -p 3306 "user=JeanM sslrootcert=/home/ec2-user/rds-combined-ca-bundle.pem sslmode=verify-ca"

#install of python and pip
   20  sudo yum install python3 -y
   21  sudo yum -y install python-pip

#go to the python directory
   30  whereis python
   31  cd bin
   32  ls
 ```
 - then run the following python2 script on your EC2 instance
 ```
 #script to connect, create table and write

import mysql.connector


cnx = mysql.connector.connect(user='JeanM', password='JeanM1234', host='myfirstdb.c4md1iztmtjn.us-east-1.rds.amazonaws.com')
cursor=cnx.cursor()

DB_NAME='FIRST_DATABASE'
cursor.execute("CREATE DATABASE {} DEFAULT CHARACTER SET 'utf8'".format(DB_NAME))

cursor.execute("USE {}".format(DB_NAME))
TABLES = {}
TABLES['Student'] = ("CREATE TABLE `Student` (""  `first_name` varchar(14) NOT NULL,""  `last_name` varchar(16) NOT NULL"") ENGINE=InnoDB")
cursor.execute(TABLES['Student'])

add_student = ("INSERT INTO Student ""(first_name, last_name) ""VALUES (%s, %s)")
data_student=('Jean','MILPIED')
cursor.execute(add_student,data_student)

cnx.commit()
cursor.close()

cnx.close()

****
#script to read the data in database

import mysql.connector

cnx = mysql.connector.connect(user='JeanM', password='JeanM1234', host='myfirstdb.c4md1iztmtjn.us-east-1.rds.amazonaws.com', database='FIRST_DATABASE')
cursor = cnx.cursor()

query = ("SELECT first_name, last_name FROM Student")
cursor.execute(query)

for (first_name, last_name) in cursor:
	print("{}, {}".format(last_name, first_name))

cursor.close()
cnx.close()
```
