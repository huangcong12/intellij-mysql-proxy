## intellij-mysql-proxy
Tool for Monitoring Code Execution CRUD.

[中文文档](README.zh_CN.md)

![mysql_proxy_原理架构图](https://github.com/huangcong12/intellij-mysql-proxy/assets/2867782/d4d0358a-842a-4feb-9466-5193e43f9eb2)

During the process of API development, whether it's troubleshooting or debugging, it's crucial to know which SQL statements have just been executed. While some modern frameworks make this information easily accessible, dealing with older frameworks can be quite cumbersome. This tool was born to address the challenge of quickly inspecting executed SQL statements within legacy frameworks. With its assistance, every SQL statement within older frameworks can be effortlessly revealed.

# Feature Description
![使用说明_en drawio](https://github.com/huangcong12/intellij-mysql-proxy/assets/2867782/fb6e0318-645f-456e-9180-21c0de0ca642)


## Suitable Frameworks:
PHP:
- Supports any framework, including: WordPress, Laravel, CI, Yii, ThinkPHP, and custom frameworks.

Go:
- Frameworks using GORM (Tested with go-admin)

Java (To be tested):

Python:
- Django (Tested and passed).

## Install
Open the intellij editor and follow through step by step
`File` > `Settings` > `Plugins` > `Marketplace` > `type in"MySQL Proxy"` > `Install`

## Initial Setup Steps:
- Check the bottom toolbar of your editor to see if there is an icon resembling a fish head (it should be located next to Git, TODO, Problems, and Terminal). Click on it to open the plugin's page.
- Click on the "Modify Run Configuration" button in the left button group and configure the remote MySQL server information in the popup: Remote MySQL Server IP Address, Remote MySQL Server Port, and the local listening port: Proxy Listener Port.
- Return to the left button group and click on "Start 'Mysql Proxy Server'" to initiate the proxy service. If you see a green dot next to the fish head icon, it indicates successful startup.
- Now, go back to your project's code and modify the database connection configuration (usually found in the Config folder's Database configuration file) to use the local IP and the "Proxy Listener Port" you configured earlier.
- Restart your project (if necessary) and execute your MySQL queries to check if SQL logs are being monitored correctly. With this, the configuration process is complete. If you still cannot see clear SQL logs, please refer to the FAQ below.

### Illustrated Usage Steps
Here is a demonstration using WordPress to show how to integrate the plugin and print the executed SQL statements.
![配置例子_en drawio](https://github.com/huangcong12/huangcong12.github.io/assets/2867782/c2f95656-4b81-434b-8487-a2b5242d9ac8)

## Usage Recommendations:
- After the initial installation, you'll need to configure the remote MySQL's IP, port, and the local proxy listening port. Once configured, please check the "Start Proxy Service with Editor" option. This way, you won't need to start it manually in the future. For the initial installation, you'll need to manually click "Start 'Mysql Proxy Server'".
- If you don't need to view SQL logs temporarily, click the "Stop Recording SQL" switch. When needed, you can reopen the logging.
- If the logged entries exceed 100,000, click the "Clear All Sql Log" button to restart logging from 1 and free up system resources.
- If you wish to copy SQL, besides using Ctrl + C to copy the entire line, you can also right-click and choose "Copy SQL." If there are SQL statements you'd rather not see, you can right-click to delete them or add them to the filter table. Once added to the filter table, they will no longer appear unless you remove them from the filter table.

## FAQ:
### Q: I followed the documentation, configured everything, and started the proxy service, but I can't see the SQL logs.
A: Possible reasons:
- 1.The MySQL connection in your code might not have been adjusted to the proxy's address. Please double-check and try stopping the proxy service to see if your code can connect to the database.
- 2.It's possible that your framework has disabled the logging feature. Check the third button in the left button group to see if it reads "Recording Synchronized SQL In Progress" and has a small green dot.
- 3.If both of the above points are correctly configured and you still encounter issues, please provide us with information about your database version, code framework, etc. If we identify the problem, we will address it in the next version.

### Q: My SQL logs appear as garbled text. How can I resolve this?
A: The database packets are encrypted. Please add the parameter "useSSL=false" to your MySQL connection, like this: "jdbc:mysql://localhost:3309?useSSL=false."

### Q: What versions of MySQL are supported?
A: We have tested it with MySQL 5.7 and MySQL 8.0, and theoretically, it should support all databases using the MySQL protocol, such as MariaDB and TiDB.

### Q: Can I use my own custom framework with this?
A: We recommend giving it a try. If you find that SQL logs appear as garbled text, please add the parameter "useSSL=false" to your configuration, like this: "jdbc:mysql://localhost:3309?useSSL=false." If it still doesn't work, it might not be supported. You can provide us with your process and results, and we will work on optimizations in the next version.