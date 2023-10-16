## intellij-mysql-proxy
[A plugin for IDEA](https://plugins.jetbrains.com/plugin/22655-mysql-proxy) that records code CRUD operations, helping you identify potential issues in SQL and providing optimization suggestions.

[中文文档](README.zh_CN.md)

![mysql_proxy_原理架构图](https://github.com/huangcong12/intellij-mysql-proxy/assets/2867782/d4d0358a-842a-4feb-9466-5193e43f9eb2)

In the development process, have you ever found yourself mostly focused on coding, dedicating less time and effort to database table creation, indexing, and SQL optimization? Perhaps you've thought that as long as the table relationships are clear, optimization can wait until the data volume increases. This approach is fine, but often, we forget about it until customers complain about system slowness.

This tool is designed to address precisely this issue. By simply integrating your code with it, you can gain a clear view of all SQL execution records and their durations. You can easily identify which SQL operations can be replaced with caching mechanisms like Redis. When you encounter slow SQL queries, just select them, right-click, and choose an optimization tool (such as GPT or established vendors) to step-by-step improve them.

# Feature Description
![使用说明_en drawio](https://github.com/huangcong12/intellij-mysql-proxy/assets/2867782/fb6e0318-645f-456e-9180-21c0de0ca642)


## Suitable Frameworks:
PHP:
- Supports any framework, including: WordPress, Laravel, CI, Yii, ThinkPHP, and custom frameworks.

Go:
- Frameworks using GORM (Tested with go-admin)

Java 
- Frameworks using Mybatis (Tested successfully with the open-source Spring Mybatis framework.)

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
- Please check the "Start Proxy Service with Editor" option. This way, you won't need to start it manually in the future. 
- If you don't need to view SQL logs temporarily, click the "Stop Recording SQL" switch. When needed, you can reopen the logging.
- If the logged entries exceed 100,000, click the "Clear All Sql Log" button to restart logging from 1 and free up system resources.
- If you wish to copy SQL, besides using Ctrl + C to copy the entire line, you can also right-click and choose "Copy SQL." If there are SQL statements you'd rather not see, you can right-click to delete them or add them to the filter table. Once added to the filter table, they will no longer appear unless you remove them from the filter table.
- You can identify slow queries based on 'Duration.' When you wish to optimize them, select the query, right-click, and choose 'Optimize with OpenAI (Free GPT-3.5, Login Required)' or another optimization method. This way, you will receive professional optimization advice.

## FAQ:
### Q: The framework I'm using already logs executed SQL statements, so I don't see much use for this plugin.
A:It might depend on your specific needs. If you only want to know the SQL for a particular API request, then the built-in SQL execution logs of the framework should suffice. However, if you want to understand the SQL executed in more complex logic, such as a specific module or the entire project, then this plugin can be helpful. It can aggregate the information you need to focus on. Additionally, this plugin can serve as a kind of 'mirror' for some projects. Through SQL analysis, you can gain insights into the foundation of a project. Moreover, the plugin has SQL analysis capabilities, helping you collect information and jump to GPT analysis, saving you time and allowing you to work more efficiently.

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