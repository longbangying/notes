##kafka 笔记
###配置文件 server.properties
1. broker.id  
	broker 的编号，如果集群中有多个 broker，则每个 broker 的编号需要设置的不同
1. zookeeper.connect 
	配置zookeeper服务的地址，多个用逗号隔开，如果有多个kafka集群用同个zookeeper集群，则需要指定zookeeper的路径(默认是在根路径上)，如192.168.153.131:2181/kafka1,多个则 192.168.153.131:2181,192.168.153.132:2181,192.168.153.133:2181/kafka1
1. listeners 
	该参数指明 broker 监听客户端连接的地址列表，即为客户端要连接 broker 的入口地址列表， 配置格式为 protocoll : //hostnamel:portl, protocol2://hostname2:port2 ，其 中 protocol 代表协议类型， Kafka 当前支持的协议类型有 PLAINTEXT、 SSL、 SASL_SSL 等， 如果未开启安全认证，则使用简单的 PLAINTEXT 即可。 hostname 代表主机名， p。此代表服务 端口，此参数的默认值为 null。比如此参数配置为 PLAINTEXT: //198 .162. 0 . 2:9092 ，如 果有多个地址，则中间以逗号隔开。如果不指定主机名，则表示绑定默认网卡，注意有可能会 绑定到 127.0.0.1 ，这样无法对外提供服务，所以主机名最好不要为空； 如果主机名是 0.0.0.0, 则表示绑定所有的网卡。与此参数关联的还有 advertised.listeners， 作用和 listeners 类似，默认值也为 null。不过 advertised.listeners 主要用于 IaaS (Infrastructure as a Service）环境，比如公有云上的机器通常配备有多块网卡，即包含私网网卡和公网网卡，对于 这种情况而言，可以设置 advertised.listeners 参数绑定公网 IP 供外部客户端使用，而 配置 listeners 参数来绑定私网 IP 地址供 broker 间通信使用.
1. log.dir和log.dirs 
	Kafka 把所有的消息都保存在磁盘上，而这两个参数用来配置 Kafka 日志文件存放的根目 录。一般情况下， log . dir 用来配置单个根目录，而 log . dirs 用来配置多个根目录（以逗 号分隔〉，但是 Kafka 井没有对此做强制性限制，也就是说， log.dir 和 log.dirs 都可以 用来配置单个或多个根目录 。 log.dirs 的优先级比 log.dir 高，但是如果没有配置 log . dirs ，则会以 log . dir 配置为准。默认情况下只配置了 log . dir 参数，其默认值为/tmp/kafka-logs
1. message.max.bytes
	该参数用来指定 broker 所能接收消息的最大值，默认值为 1000012 (B ），约等于 976.6阻。 如果 Producer 发送的消息大于这个参数所设置的值，那么（ Producer 〉就会报出 RecordTooLargeException 的异常。如果需要修改这个参数，那么还要考虑 max.request.size （客户端参数）,max.message.bytes (topic 端参数）等参数的影响。为了避免修改此参数 而引起级联的影响，建议在修改此参数之前考虑分拆消息的可行性.
