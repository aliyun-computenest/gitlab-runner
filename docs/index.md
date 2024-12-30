# Gitlab Runner

>**免责声明：**本服务由第三方提供，我们尽力确保其安全性、准确性和可靠性，但无法保证其完全免于故障、中断、错误或攻击。因此，本公司在此声明：对于本服务的内容、准确性、完整性、可靠性、适用性以及及时性不作任何陈述、保证或承诺，不对您使用本服务所产生的任何直接或间接的损失或损害承担任何责任；对于您通过本服务访问的第三方网站、应用程序、产品和服务，不对其内容、准确性、完整性、可靠性、适用性以及及时性承担任何责任，您应自行承担使用后果产生的风险和责任；对于因您使用本服务而产生的任何损失、损害，包括但不限于直接损失、间接损失、利润损失、商誉损失、数据损失或其他经济损失，不承担任何责任，即使本公司事先已被告知可能存在此类损失或损害的可能性；我们保留不时修改本声明的权利，因此请您在使用本服务前定期检查本声明。如果您对本声明或本服务存在任何问题或疑问，请联系我们。

## 概述
Gitlab Runner 是一个开源的应用程序身份验证和授权解决方案。您可以拥有自己的数据库，并完全控制用户信息。您可以自行托管 Gitlab Runner 实例并连接到任何数据库（目前支持 11 种以上的数据库，包括 Postgres、MySQL、SQLite、SQLServer、YugaByte、MariaDB、PlanetScale、CassandraDB、ScyllaDB、MongoDB、ArangoDB）。

Gitlab Runner提供了一下的功能：
- ✅ 使用电子邮件ID和密码登录/注册
- ✅ 安全的会话管理
- ✅ 电子邮件验证
- ✅ 兼容 OAuth2 和 OpenID 的 API
- ✅ 安全更新个人资料的 API
- ✅ 使用电子邮件的忘记密码流程
- ✅ 社交登录（Google、Github、Facebook、LinkedIn、Apple，更多即将推出）
- ✅ 基于角色的访问管理
- ✅ 无密码登录
- ✅ 多因素认证
- ✅ 电子邮件模板
- ✅ Webhooks

## 前提条件
<font style="color:rgb(51, 51, 51);">部署Gitlab Runner社区版服务实例，需要对部分阿里云资源进行访问和创建操作。因此您的账号需要包含如下资源的权限。</font><font style="color:rgb(51, 51, 51);"> </font>**<font style="color:rgb(51, 51, 51);">说明</font>**<font style="color:rgb(51, 51, 51);">：当您的账号是RAM账号时，才需要添加此权限。</font>

| <font style="color:rgb(51, 51, 51);">权限策略名称</font> | <font style="color:rgb(51, 51, 51);">备注</font> |
| --- | --- |
| <font style="color:rgb(51, 51, 51);">AliyunECSFullAccess</font> | <font style="color:rgb(51, 51, 51);">管理云服务器服务（ECS）的权限</font> |
| <font style="color:rgb(51, 51, 51);">AliyunVPCFullAccess</font> | <font style="color:rgb(51, 51, 51);">管理专有网络（VPC）的权限</font> |
| <font style="color:rgb(51, 51, 51);">AliyunROSFullAccess</font> | <font style="color:rgb(51, 51, 51);">管理资源编排服务（ROS）的权限</font> |
| <font style="color:rgb(51, 51, 51);">AliyunComputeNestUserFullAccess</font> | <font style="color:rgb(51, 51, 51);">管理计算巢服务（ComputeNest）的用户侧权限</font> |
除此以外还需要一个域名和SSL证书，因为Gitlab Runner软件只能在HTTPS协议下访问。


## 计费说明
<font style="color:rgb(51, 51, 51);"> 社区版在计算巢部署的费用主要涉及：</font>

+ <font style="color:rgb(51, 51, 51);">所选vCPU与内存规格</font>
+ <font style="color:rgb(51, 51, 51);">系统盘类型及容量</font>
+ <font style="color:rgb(51, 51, 51);">公网带宽</font>

## 部署架构
![](./img/deploy.png)



## 参数说明
| <font style="color:rgb(51, 51, 51);">参数组</font> | <font style="color:rgb(51, 51, 51);">参数项</font> | <font style="color:rgb(51, 51, 51);">说明</font> |
| --- | --- | --- |
| <font style="color:rgb(51, 51, 51);">服务实例</font> | <font style="color:rgb(51, 51, 51);">服务实例名称</font> | <font style="color:rgb(51, 51, 51);">长度不超过64个字符，必须以英文字母开头，可包含数字、英文字母、短划线（-）和下划线（_）</font> |
| | <font style="color:rgb(51, 51, 51);">地域</font> | <font style="color:rgb(51, 51, 51);">服务实例部署的地域</font> |
| | <font style="color:rgb(51, 51, 51);">付费类型</font> | <font style="color:rgb(51, 51, 51);">资源的计费类型：按量付费和包年包月</font> |
| <font style="color:rgb(51, 51, 51);">ECS实例配置</font> | <font style="color:rgb(51, 51, 51);">实例类型</font> | <font style="color:rgb(51, 51, 51);">可用区下可以使用的实例规格</font> |
| | <font style="color:rgb(51, 51, 51);">实例密码</font> | <font style="color:rgb(51, 51, 51);">长度8-30，必须包含三项（大写字母、小写字母、数字、 ()`~!@#$%^&*-+=|{}[]:;'<>,.?/ 中的特殊符号）</font> |
| <font style="color:rgb(51, 51, 51);">网络配置</font> | <font style="color:rgb(51, 51, 51);">可用区</font> | <font style="color:rgb(51, 51, 51);">ECS实例所在可用区</font> |
| | <font style="color:rgb(51, 51, 51);">VPC ID</font> | <font style="color:rgb(51, 51, 51);">资源所在VPC</font> |
| | <font style="color:rgb(51, 51, 51);">交换机ID</font> | <font style="color:rgb(51, 51, 51);">资源所在交换机</font> |


## 部署流程
1. 访问计算巢 [部署链接](https://computenest.console.aliyun.com/service/instance/create/default?type=user&ServiceName=Gitlab Runner%E7%A4%BE%E5%8C%BA%E7%89%88)，按提示填写部署参数
2. 填写实例参数![](./img/param1.png)
3. 根据需求选择新建专用网络或直接使用已有的专有网络。填写可用区和网络参数![](./img/param2.png)
4. 填写您的域名。注意需要购买SSL证书。![](./img/param3.png)
5. 点击立即创建，等待服务实例部署完成![](./img/param4.png)
6. 服务实例部署完成后，点击实例ID进入到详情界面![](./img/serviceInstance2.png)
7. 访问服务实例的使用URL![](./img/serviceInstance3.png)
8. 该应用提供了一个端口，可直接点击使用。
9. 让我们进入到登录界面，注册admin账号。这里固定了账号名为admin，密码由你输入![](./img/app1.png)
10. 在使用前我们需要将你的域名绑定到服务实例的公网IP上，比如使用阿里云的云解析服务。![](./img/app4.png)
11. 可参考该图示例设置dns的解析。![](./img/app5.png)
12. 如果没有SSL证书，可在阿里云上购买。![](./img/app6.png)
13. 登录进来后可以看到整体的控制台如图![](./img/app2.png)
14. 支持了很多种的OAuth登录方式，比如谷歌，Github等。
15. 配置完，可访问https://你的域名:8080/app实现OAuth登录。


## 进阶配置

### 通过配置HPA规则实现Runner Manager节点动态伸缩


### ACK场景下使用ECI+Spot弹性运行Worker节省成本

选择对应的ack集群，运维管理->组件管理->安装ACK Virtual Node 开启ECI弹性能力 update-0.png 进入服务实例->修改配置
![](./img/update-0.png)
通过添加如下注解，可以让ACK中的work节点使用spot实例。
```yaml

    alibabacloud.com/eci: 'true'
  extraAnnotations:
    k8s.aliyun.com/eci-spot-strategy: SpotAsPriceGo

```
### 镜像构建缓存配置
本服务实例已经预装了OSS，在编写构建镜像的流水线可设置缓存的位置。可参考下部分配置示例：
```yaml

```

### 将Gitlab runner构建的镜像上传到镜像仓库

### 完整Values参考
https://gitlab.com/gitlab-org/charts/gitlab-runner/blob/main/values.yaml
可基于此实现一些更高阶的配置，比如通过Hpa实现Runner manager节点的动态扩缩容。