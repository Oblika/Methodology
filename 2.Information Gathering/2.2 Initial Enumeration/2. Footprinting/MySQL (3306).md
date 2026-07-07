
```bash
# Default Configuration
sudo apt install mysql-server -y
cat /etc/mysql/mysql.conf.d/mysqld.cnf | grep -v "#" | sed -r '/^\s*$/d'

# Connected to a MySQL Server
mysql -u <user> -h <IP>

# Local connection (no password)
mysql -u root

# Local connection with password
mysql -u username -p

# Connect to specific database
mysql -u username -p database_name

# Remote connection
mysql -u username -h target.com -P 3306 -p

# Connect and execute query
mysql -u username -p -e "SELECT @@version;"

# Connect without database selection
mysql -u username -h target.com -p --skip-database

# MySQL Commands
show databases;	# Show all databases.
use <database>;	# Select one of the existing databases.
show tables;	# Show all available tables in the selected database.
show columns from <table>;	# Show all columns in the selected table.
select * from <table>;	# Show everything in the desired table.
select * from <table> where <column> = "<string>";
```