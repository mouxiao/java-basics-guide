### Consul 介绍
> Consul是一个服务发现，类似的服务发现有ETCD,zookeeper,还有eureka。Consul的服务发现，是比较的不错的，不经支持服务发现的功能， 还支持健康检查,Key/Value存储。
包含以下关键功能：多数据中心，服务发现、健康检查、键值存储和多数据中心支持


#### 端口说明
- 8300 服务间通信(tcp)
- 8500 用于ui展示以及Http Api调用(tcp)
- 8600 DNS通信(udp)

#### Consul 基本操作
常用基本命令
- dev 以开发模式启动代理(consul agent -dev)
- members 查看集群成员
- agent 启动一个consul代理
- config-dir 配置文件目录，里面所有以.json结尾的文件都会被加载
- client consul服务侦听地址，这个地址提供HTTP、DNS、RPC等服务，默认是127.0.0.1所以不对外提供服务，
  如果你要对外提供服务改成0.0.0.0
- reload 服务更新
- advertise：通知展现地址用来改变我们给集群中的其他节点展现的地址，一般情况下-bind地址就是展现地址
- bootstrap：用来控制一个server是否在bootstrap模式，在一个datacenter中只能有一个server处于bootstrap模式，当一个server处于bootstrap模式时，可以自己选举为raft leader。
- bootstrap-expect：在一个datacenter中期望提供的server节点数目，当该值提供的时候，consul一直等到达到指定sever数目的时候才会引导整个集群，该标记不能和bootstrap公用
- bind：该地址用来在集群内部的通讯，集群内的所有节点到地址都必须是可达的，默认是0.0.0.0
- client：consul绑定在哪个client地址上，这个地址提供HTTP、DNS、RPC等服务，默认是127.0.0.1
- config-file：明确的指定要加载哪个配置文件
- config-dir：配置文件目录，里面所有以.json结尾的文件都会被加载
- data-dir：提供一个目录用来存放agent的状态，所有的agent允许都需要该目录，该目录必须是稳定的，系统重启后都继续存在
- dc：该标记控制agent允许的datacenter的名称，默认是dc1
- encrypt：指定secret key，使consul在通讯时进行加密，key可以通过consul keygen生成，同一个集群中的节点必须使用相同的key
- join：加入一个已经启动的agent的ip地址，可以多次指定多个agent的地址。如果consul不能加入任何指定的地址中，则agent会启动失败，默认agent启动时不会加入任何节点。
- retry-join：和join类似，但是允许你在第一次失败后进行尝试。
- retry-interval：两次join之间的时间间隔，默认是30s
- retry-max：尝试重复join的次数，默认是0，也就是无限次尝试
- log-level：consul agent启动后显示的日志信息级别。默认是info，可选：trace、debug、info、warn、err。
- node：节点在集群中的名称，在一个集群中必须是唯一的，默认是该节点的主机名
- protocol：consul使用的协议版本
- rejoin：使consul忽略先前的离开，在再次启动后仍旧尝试加入集群中。
- server：定义agent运行在server模式，每个集群至少有一个server，建议每个集群的server不要超过5个
- syslog：开启系统日志功能，只在linux/osx上生效
- ui-dir:提供存放web ui资源的路径，该目录必须是可读的
- pid-file:提供一个路径来存放pid文件，可以使用该文件进行SIGINT/SIGHUP(关闭/更新)agent
- monitor 监控 (ex:监控日志 consul monitor -log-level info)

> 除了基本的命令的形式外，也可以使用配置文件的形式

ex:
```json
{
  "datacenter": "dc1",
  "data_dir": "/opt/consul",
  "log_level": "INFO",
  "node_name": "s1",
  "server": true,
  "bootstrap_expect": 3,
  "bind_addr": "10.201.102.198",
  "client_addr": "0.0.0.0",
  "ui_dir": "/root/consul_ui",
  "retry_join": ["10.201.102.198","10.201.102.199","10.201.102.200","10.201.102.248"],
  "retry_interval": "30s",
  "enable_debug": false,
  "rejoin_after_leave": true,
  "start_join": ["10.201.102.198","10.201.102.199","10.201.102.200","10.201.102.248"],
  "enable_syslog": true,
  "syslog_facility": "local5"
}
```


> 配置文件参数：

- acl_datacenter：只用于server，指定的datacenter的权威ACL信息，所有的servers和datacenter必须同意ACL datacenter
- acl_default_policy：默认是allow
- acl_down_policy：
- acl_master_token：
- acl_token：agent会使用这个token和consul server进行请求
- acl_ttl：控制TTL的cache，默认是30s
- addresses：一个嵌套对象，可以设置以下key：dns、http、rpc
- advertise_addr：等同于-advertise
- bootstrap：等同于-bootstrap
- bootstrap_expect：等同于-bootstrap-expect
- bind_addr：等同于-bind
- ca_file：提供CA文件路径，用来检查客户端或者服务端的链接
- cert_file：必须和key_file一起
- check_update_interval：
- client_addr：等同于-client
- datacenter：等同于-dc
- data_dir：等同于-data-dir
- disable_anonymous_signature：在进行更新检查时禁止匿名签名
- disable_remote_exec：禁止支持远程执行，设置为true，agent会忽视所有进入的远程执行请求
- disable_update_check：禁止自动检查安全公告和新版本信息
- dns_config：是一个嵌套对象，可以设置以下参数：allow_stale、max_stale、node_ttl 、service_ttl、enable_truncate
- domain：默认情况下consul在进行DNS查询时，查询的是consul域，可以通过该参数进行修改
- enable_debug：开启debug模式
- enable_syslog：等同于-syslog
- encrypt：等同于-encrypt
- key_file：提供私钥的路径
- leave_on_terminate：默认是false，如果为true，当agent收到一个TERM信号的时候，它会发送leave信息到集群中的其他节点上。
- log_level：等同于-log-level
- node_name:等同于-node
- ports：这是一个嵌套对象，可以设置以下key：dns(dns地址：8600)、http(http api地址：8500)、rpc(rpc:8400)、serf_lan(lan port:8301)、serf_wan(wan port:8302)、server(server rpc:8300)
- protocol：等同于-protocol
- recursor：
- rejoin_after_leave：等同于-rejoin
- retry_join：等同于-retry-join
- retry_interval：等同于-retry-interval
- server：等同于-server
- server_name：会覆盖TLS CA的node_name，可以用来确认CA name和hostname相匹配
- skip_leave_on_interrupt：和leave_on_terminate比较类似，不过只影响当前句柄
- start_join：一个字符数组提供的节点地址会在启动时被加入
- statsd_addr：
- statsite_addr：
- syslog_facility：当enable_syslog被提供后，该参数控制哪个级别的信息被发送，默认Local0
- ui_dir：等同于-ui-dir
- verify_incoming：默认false，如果为true，则所有进入链接都需要使用TLS，需要客户端使用ca_file提供ca文件，只用于consul server端，因为client从来没有进入的链接
- verify_outgoing：默认false，如果为true，则所有出去链接都需要使用TLS，需要服务端使用ca_file提供ca文件，consul server和client都需要使用，因为两者都有出去的链接
- watches：watch一个详细名单

#### HTTP API
> consul的主要接口是RESTful HTTP API，该API可以用来增删查改nodes、services、checks、configguration。所有的endpoints主要分为以下类别：

- kv - Key/Value存储
- agent - Agent控制
- catalog - 管理nodes和services
- health - 管理健康监测
- session - Session操作
- acl - ACL创建和管理
- event - 用户Events
- status - Consul系统状态

> 下面我们就单独看看每个模块的具体内容

##### agent
> agent endpoints用来和本地agent进行交互，一般用来服务注册和检查注册，支持以下接口

```text
/v1/agent/checks : 返回本地agent注册的所有检查(包括配置文件和HTTP接口)
/v1/agent/services : 返回本地agent注册的所有 服务
/v1/agent/members : 返回agent在集群的gossip pool中看到的成员
/v1/agent/self : 返回本地agent的配置和成员信息
/v1/agent/join/<address> : 触发本地agent加入node
/v1/agent/force-leave/<node>>: 强制删除node
/v1/agent/check/register : 在本地agent增加一个检查项，使用PUT方法传输一个json格式的数据
/v1/agent/check/deregister/<checkID> : 注销一个本地agent的检查项
/v1/agent/check/pass/<checkID> : 设置一个本地检查项的状态为passing
/v1/agent/check/warn/<checkID> : 设置一个本地检查项的状态为warning
/v1/agent/check/fail/<checkID> : 设置一个本地检查项的状态为critical
/v1/agent/service/register : 在本地agent增加一个新的服务项，使用PUT方法传输一个json格式的数据
/v1/agent/service/deregister/<serviceID> : 注销一个本地agent的服务项
```

##### catalog
> catalog endpoints用来注册/注销nodes、services、checks
```text
/v1/catalog/register : Registers a new node, service, or check
/v1/catalog/deregister : Deregisters a node, service, or check
/v1/catalog/datacenters : Lists known datacenters
/v1/catalog/nodes : Lists nodes in a given DC
/v1/catalog/services : Lists services in a given DC
/v1/catalog/service/<service> : Lists the nodes in a given service
/v1/catalog/node/<node> : Lists the services provided by a node
```

##### health
> health endpoints用来查询健康状况相关信息，该功能从catalog中单独分离出来
```text
/v1/healt/node/<node>: 返回node所定义的检查，可用参数?dc=
/v1/health/checks/<service>: 返回和服务相关联的检查，可用参数?dc=
/v1/health/service/<service>: 返回给定datacenter中给定node中service
/v1/health/state/<state>: 返回给定datacenter中指定状态的服务，state可以是"any", "unknown", "passing", "warning", or "critical"，可用参数?dc=
```

##### session
> session endpoints用来create、update、destory、query sessions
```text
/v1/session/create: Creates a new session
/v1/session/destroy/<session>: Destroys a given session
/v1/session/info/<session>: Queries a given session
/v1/session/node/<node>: Lists sessions belonging to a node
/v1/session/list: Lists all the active sessions
```

##### acl
> acl endpoints用来create、update、destory、query acl
```text
/v1/acl/create: Creates a new token with policy
/v1/acl/update: Update the policy of a token
/v1/acl/destroy/<id>: Destroys a given token
/v1/acl/info/<id>: Queries the policy of a given token
/v1/acl/clone/<id>: Creates a new token by cloning an existing token
/v1/acl/list: Lists all the active tokens
```

##### event
> event endpoints用来fire新的events、查询已有的events
```text
/v1/event/fire/<name>: 触发一个新的event，用户event需要name和其他可选的参数，使用PUT方法
/v1/event/list: 返回agent知道的events
```

##### status
> status endpoints用来或者consul 集群的信息
```text
/v1/status/leader : 返回当前集群的Raft leader
/v1/status/peers : 返回当前集群中同事
```


>>> 内容太多，参考：https://blog.csdn.net/liuzhuchen/article/details/81913562