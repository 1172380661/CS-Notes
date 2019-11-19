## 下载安装 ##
先下载erlang，再下载rabbitMQ。
## 启动服务及可视化管理 ##
打开rabbitmq的命令行界面输入

    //启动rabbitmq
    C:\Program Files\RabbitMQ Server\rabbitmq_server-3.7.16\sbin>rabbitmq-plugins.bat
	//开启management 可以通过http://localhost:15672/进行访问
    C:\Program Files\RabbitMQ Server\rabbitmq_server-3.7.16\sbin>rabbitmq-plugins enable rabbitmq_management
## 基本概念 ##
生产者producer：生产者直接将消息放入交换机中。  
binding key：消息队列和交换机绑定的关键字。一个消息队列可以有多个binding key。  
routing key：由用户来指定的路由转发策略，和binding key以及交换机共同决定消息要发送到哪个Queue。  
交换机exchange：交换机和Queue直接用binding key绑定，一个交换机可以绑定多个bindkey。通过匹配binding key和producer指定的routing key来判断将消息发送往哪个Queue。当然消息的转发策略也和交换机有关。  
交换机种类：  
1. fonout:把所有发送到该Exchange的消息路由到所有与它绑定的Queue中。  
2. direct：把消息路由到那些binding key与routing key完全匹配的Queue中。  
3. topic：和direct类似，但是并不要求binding key和routing key完全匹配。它约定：

- routing key为一个句点号“.”分隔的字符串（我们将被句点号“. ”分隔开的每一段独立的字符串称为一个单词），如“stock.usd.nyse”、“nyse.vmw”、“quick.orange.rabbit”
- binding key与routing key一样也是句点号“. ”分隔的字符串。
- binding key中可以存在两种特殊字符“*”与“#”，用于做模糊匹配，其中“*”用于匹配一个单词，“#”用于匹配多个单词（可以是零个）

4. header：headers类型的Exchange不依赖于routing key与binding key的匹配规则来路由消息，而是根据发送的消息内容中的headers属性进行匹配。
在绑定Queue与Exchange时指定一组键值对；当消息发送到Exchange时，RabbitMQ会取到该消息的headers（也是一个键值对的形式），对比其中的键值对是否完全匹配Queue与Exchange绑定时指定的键值对；如果完全匹配则消息会路由到该Queue，否则不会路由到该Queue。


