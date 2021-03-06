## 配置管理示例

### 场景需求

假定某个业务（微）服务具有大量的配置参数，产品运营通过这些配置开关随时控制产品提供的服务。该业务服务存在开发、测试和线上三套环境。另外，出于安全考虑，一些敏感信息如API访问密钥，数据库的访问密码在三套环境中各不相同，同时要求不能出现在配置文件和代码中。在不定制开发的前提下，如何简单有效的跟踪和管理配置开关？如何确保敏感信息的安全隔离？

### 解决方案


![](/assets/cfg4j-distributed-config-management.png)

本例采用上图所示的解决方案，大致步骤如下：

1. 应用配置文件采用Java的Properties格式，存放在Gitlab的仓库中。**dev**分支下的配置文件对应着开发环境，**test**分支对应着测试环境，**release**分支对应着线上环境。

2. 敏感信息如访问数据库的用户名/密码存放在Vault中的“generic”后端中。开发数据库密钥存放在“secret/db-dev”下；测试数据库密钥存放在“secret/db-test”下；线上数据库密钥存放在“secret/db-prod”下。

3. Vault服务器的地址存放在Java的Properties文件中。

4. 配置Jenkins任务分别监控Git仓库三个分支，在应用配置信息变更时自动推送到Consul。

5. 应用通过用户名/密码方式访问Vault获取数据库访问密钥。

6. 应用定时从Consul拉取配置信息。

### 详细过程

本示例以DC/OS计算环境为基础运行环境。示例仅用于验证方案的可行性，具体运行环境配置及代码示例不应在生产环境使用。

#### 准备工作

Gitlab

Jenkins

Consul

Vault

#### 准备配置存储

#### 准备数据库访问密钥和Vault访问权限

#### 配置Jenkins任务推送配置变更到Consul

#### 示例代码

### 结论

1. 实际的应用根据实际需求和业务流程进行调整。

2. 可以通过实现整合的UI对配置信息、任务、Vault的管理等进行集成配置。

### 参考

- https://github.com/cfg4j/cfg4j-sample-apps
- https://github.com/cfg4j/cfg4j-pusher
- http://docs.spring.io/spring-vault/docs/current-SNAPSHOT/reference/html
- https://www.vaultproject.io/docs/auth/index.html