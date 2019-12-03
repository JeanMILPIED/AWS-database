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
 
 # create a connection from python
 - use mysql connector
 `import mysql.connector
from mysql.connector import errorcode

try:
  cnx = mysql.connector.connect(user='scott',
                                database='employ')
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with your user name or password")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist")
  else:
    print(err)
else:
  cnx.close()`
