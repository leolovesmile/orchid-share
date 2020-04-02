# MySQL operation logs

- Change root's password  
Just termiate the mysql process and start it with `--skip-grant-tables` parameter, then you could change password or update permission of user.  
```
service mysql restart --skip-grant-tables
## or
mysqld --skip-grant-tables --user=mysql &
```

- When you want to save the query result into an output file but get the file write permission error. Please keep in mind that there is an configuration item to control the security about the file output.  
```
UPDATE mysql.user SET File_priv = 'Y' WHERE user='theuser';
```