### 这是nacos适配达梦数据库的源码
### nacos-2.3.2 达梦数据库8
原文链接 https://blog.csdn.net/pky86676022/article/details/137726884


编译后的产物下载链接 https://download.csdn.net/download/pky86676022/89131353


参考：https://github.com/nacos-group/nacos-plugin

参考：https://github.com/alibaba/nacos

参考：https://nacos.io/

**我已经制作好了docker镜像，运行命令如下（此docker镜像支持arm64和amd64架构，官方的镜像编译是https://github.com/nacos-group/nacos-docker**    ）
```shell
docker run -d --name=nacos-dm \
-e MODE=standalone \
-v /root/application.properties:/home/nacos/conf/application.properties \
-p 8848:8848 -p 9848:9848 \
pkyit/nacos:2.3.2-dm8
```
启动时的配置文件如下 application.properties
```properties
### Default web context path:
server.servlet.contextPath=/nacos
### Include message field
server.error.include-message=ALWAYS
### Default web server port:
server.port=8848

### Count of DB:
db.num=1

### Connect URL of DB:
spring.sql.init.platform=dm
db.url.0=jdbc:dm://10.10.10.104:5236?schema=NACOS
db.user.0=SYSDBA
db.password.0=SYSDBA001
db.pool.config.driverClassName=dm.jdbc.driver.DmDriver

### Connection pool configuration: hikariCP
db.pool.config.connectionTimeout=30000
db.pool.config.validationTimeout=10000
db.pool.config.maximumPoolSize=20
db.pool.config.minimumIdle=2

### the maximum retry times for push
nacos.config.push.maxRetryTime=50

#***********Metrics for tomcat **************************#
server.tomcat.mbeanregistry.enabled=true

### Metrics for elastic search
management.metrics.export.elastic.enabled=false
#management.metrics.export.elastic.host=http://localhost:9200

### Metrics for influx
management.metrics.export.influx.enabled=false

#*************** Access Log Related Configurations ***************#
### If turn on the access log:
server.tomcat.accesslog.enabled=true

### file name pattern, one file per hour
server.tomcat.accesslog.rotate=true
server.tomcat.accesslog.file-date-format=.yyyy-MM-dd-HH
### The access log pattern:
server.tomcat.accesslog.pattern=%h %l %u %t "%r" %s %b %D %{User-Agent}i %{Request-Source}i

### The directory of access log:
server.tomcat.basedir=file:.

#spring.security.enabled=false

### The ignore urls of auth
nacos.security.ignore.urls=/,/error,/**/*.css,/**/*.js,/**/*.html,/**/*.map,/**/*.svg,/**/*.png,/**/*.ico,/console-ui/public/**,/v1/auth/**,/v1/console/health/**,/actuator/**,/v1/console/server/**

### The auth system to use, currently only 'nacos' and 'ldap' is supported:
nacos.core.auth.system.type=nacos

### If turn on auth system:
nacos.core.auth.enabled=true

### Turn on/off caching of auth information. By turning on this switch, the update of auth information would have a 15 seconds delay.
nacos.core.auth.caching.enabled=true

### Since 1.4.1, Turn on/off white auth for user-agent: nacos-server, only for upgrade from old version.
nacos.core.auth.enable.userAgentAuthWhite=true

### Since 1.4.1, worked when nacos.core.auth.enabled=true and nacos.core.auth.enable.userAgentAuthWhite=false.
### The two properties is the white list for auth and used by identity the request from other server.
nacos.core.auth.server.identity.key=nacos
nacos.core.auth.server.identity.value=nacos

### worked when nacos.core.auth.system.type=nacos
### The token expiration in seconds:
nacos.core.auth.plugin.nacos.token.cache.enable=false
nacos.core.auth.plugin.nacos.token.expire.seconds=18000
### The default token (Base64 String):
nacos.core.auth.plugin.nacos.token.secret.key=SecretKey012345678901234567890123456789012345678901234567890123456789

#*************** Istio Related Configurations ***************#
### If turn on the MCP server:
nacos.istio.mcp.server.enabled=false

#nacos.console.ui.enabled=true
```
