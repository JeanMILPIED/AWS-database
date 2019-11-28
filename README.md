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
- chose the appropriate VPC
  - your VPC should have some particular design: 2 AZ, 2 Subnets
  - one option is to chose "create a VPC"
  - create a DB Subnet group
  - create a DB security group: DB SG group
- launch
- check you can connect using Toad
  - install Toad: https://www.techrepublic.com/blog/tr-dojo/manage-mysql-databases-from-a-windows-desktop-with-toad/
  - create a new connection in Toad:
    - DOES NOT SEEM TO WORK
  - or use mySQL workbench: https://dev.mysql.com/downloads/file/?id=490464
    - end point: myfirstdatabase2.c4md1iztmtjn.us-east-1.rds.amazonaws.com
    - use your username and passwork credentials
  
- in your VPC
  - check that the security rule of the DB instance meets a private route table
