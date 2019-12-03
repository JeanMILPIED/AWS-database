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
 - use mysql connector: does not work?
 - use pymysql
  ```
  import pymysql.cursors
import pymysql

# Connect to the database
connection = pymysql.connect(host='localhost',
                             user='user',
                             password='passwd',
                             db='db',
                             charset='utf8mb4',
                             cursorclass=pymysql.cursors.DictCursor)

try:
    with connection.cursor() as cursor:
        # Create a new record
        sql = "INSERT INTO `users` (`email`, `password`) VALUES (%s, %s)"
        cursor.execute(sql, ('webmaster@python.org', 'very-secret'))

    # connection is not autocommit by default. So you must commit to save
    # your changes.
    connection.commit()

    with connection.cursor() as cursor:
        # Read a single record
        sql = "SELECT `id`, `password` FROM `users` WHERE `email`=%s"
        cursor.execute(sql, ('webmaster@python.org',))
        result = cursor.fetchone()
        print(result)
finally:
    connection.close()
    ```
