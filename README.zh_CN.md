## intellij-mysql-proxy
监控代码CURD的工具：协助你发现有问题的 SQL，并优化它。

![mysql_proxy_原理架构图](https://github.com/huangcong12/intellij-mysql-proxy/assets/2867782/d4d0358a-842a-4feb-9466-5193e43f9eb2)

在开发需求的时候，不知道你是否也是这样：把大部分时间、精力都用在编码上，而在数据库建表、建立索引、调优 SQL 上花费的时间较少，并想着只要确保表关系清晰，优化的事情等数据量上来了再讨论也不迟。其实这种做法没问题，只是在这之后我们就忘了这回事，并在客户投诉系统慢了，我们才记起来。这个工具正是为协助解决这种问题而诞生的，只需要把代码接入它，就能清晰地看到所有 SQL 执行记录和耗时，也可轻松地看出哪些 SQL 操作是可以使用缓存（redis）代替。当你发现慢 SQL 时，只需要选中并右键，选择优化工具（GPT、老牌厂商等），一步一步优化它。

## 功能概览
![使用说明 drawio](https://github.com/huangcong12/intellij-mysql-proxy/assets/2867782/4c8a97b3-601a-4b08-ab9b-9341f76d518b)

## 适合的框架：
PHP：
- 支持任何框架，包括：WordPress、Laravel、CI、Yii、ThinkPHP 以及自研框架

go:
- 使用 gorm 的框架（使用 go-admin 测试通过）

java：
- 使用 Mybatis 的框架（使用 Spring Mybatis 的开源框架测试通过）    

python:
- django（测试通过）

## 安装
打开 intellij 的编辑器，跟着一步一步操作：
`File` > `Settings` > `Plugins` > `Marketplace` > `type in"MySQL Proxy"` > `Install`

## 首次使用步骤：
- 查看编辑器的底部工具栏，是不是多了一个小鱼头的图标（应该在 git、TODO、Problems、Terminal 旁边），点击它弹出插件的页面
- 点击左边按钮组的“Modify Run Configuration”，并在弹框中配置远程 MySQL 服务器信息：Remote MySQL Server IP Address、Remote MySQL Server Port，和本地监听端口：Proxy Listener Port
- 再回到左边按钮组，点击“Start 'Mysql Proxy Server'”启动代理服务，如果看到小鱼头多了一个绿点，表示启动成功
- 现在回到你的项目代码，修改连接数据库的配置（通常是 Config 文件夹下的 Database 配置文件），改成本地 ip 和上面配置的“Proxy Listener Port”
- 重启（如果需要）你的项目并执行查询 MySQL 逻辑，看看是不是能正常监听到 SQL 日志。至此配置工作全部结束，如果你没有能看到清晰的 SQL 日志，请往下看 FAQ

### 图示使用步骤
下面以 WordPress 演示，怎么接入插件，并把执行的 SQL 打印出来
![配置例子_zh drawio](https://github.com/huangcong12/huangcong12.github.io/assets/2867782/0fa8e732-b1d9-4c7c-9d3b-87a608f85bdf)

## 使用建议：
- 配置完成后请勾选跟随编辑器启动，这样以后就不用手动启动了
- 如果暂时不需要查看 SQL，请点击关闭记录 SQL 日志开关，有需要了再重新打开记录日志
- 如果记录的日志超过了 10万，请点击清空按钮，重新从 1 开始记录，释放系统资源
- 如果你想复制 SQL，除了 ctrl + c 复制整行，还可以点击右键，选择复制SQL。如果一些SQL你不想看到，也可以通过点击右键，把SQL删除，或把它增加到过滤表里，增加到过滤表以后，就再也不会出现了，除非你把它从过滤表里删除
- 你可以根据“Duration” 找到慢查询，当你希望优化它的时候，选中并点击鼠标右键，选择“Optimize with OpenAI (Free GPT-3.5, Login Required)”或其他的优化方式，这样你将得到专业的优化建议

## FAQ:
### Q：我使用的框架有记录执行 SQL 的日志，感觉这个插件也没什么用
A:可能要看看你的需求。假如你只是想知道某个 API 请求的 SQL，那么框架自带的 SQL 执行日志是可以满足的；假如你想了解更多逻辑的执行 SQL，比如某个模块、或整个项目的，那这个插件可以帮到你，它能帮你聚合需要关注的信息。另外这个插件也能成为一些项目的照妖镜，通过 SQL，能知道这个项目的底子是怎么样的。而且插件也有 SQL 分析功能，它帮忙收集信息跳转到 GPT 分析，帮助你节省时间，更快完成工作。

### Q：我按照文档配置，已经启动代理服务，但是没有看到 SQL 日志
A：可能原因：
1、代码里的 MySQL 连接没有调整成代理的地址，请重新检查一遍，并尝试停止代理服务看看代码是否能请求到数据库。
2、框架关闭了记录日志功能，检查左边按钮组的第三个按钮，是否是 “Recording Synchronized SQL In Progress”，并且有一个小绿点。
3、其他未知情况，如果上面两项都是正确配置，请把你的数据库版本、代码框架等信息提交给我们，我们如果排查到问题，会在下一个版本修复它。

### Q:我的 SQL 日志都是乱码，应该怎么解决
A:数据库包被加密了。请给 MySQL 连接增加参数： useSSL=false，如：jdbc:mysql://localhost:3309?useSSL=false

### Q:支持什么版本的 MySQL？
A:我们测试了 MySQL5.7、Mysql 8.0 都可以，理论上所有使用 Mysql 协议的数据库都支持，比如：MariaDB、TIDB。

### Q:我使用自己研发的框架，可以支持吗？
A:建议您试一下，如果发现 sql 日志都是乱码，请增加参数： useSSL=false，如：jdbc:mysql://localhost:3309?useSSL=false。如果还不行，那可能是不支持。您可以把操作过程和结果反馈给我们，我们会在下个版本优化。

