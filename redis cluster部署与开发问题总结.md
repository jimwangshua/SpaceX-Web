## ����Ҫ��
- ruby����version >= 2.2.2.
ʹ��ruby�ű���װ��Ⱥ������osҪ��װruby��
��Ȼ����
```
/usr/bin/env: ruby: No such file or directory
```
- ��Ҫ��װruby����redis�Ŀͻ���

    ��Ȼ����
```
[root@hadoop-slave-002 rediscluster]# ./redis-trib.rb create --replicas 1 192.168.253.131:7000 192.168.253.131:7001  192.168.253.131:7002 192.168.253.131:7003 192.168.253.131:7004 192.168.253.131:7005
/usr/share/rubygems/rubygems/core_ext/kernel_require.rb:55:in `require': cannot load such file -- redis (LoadError)
	from /usr/share/rubygems/rubygems/core_ext/kernel_require.rb:55:in `require'
	from ./redis-trib.rb:25:in `<main>'

```
    ִ������
```
gem install redis
```
    ����
```
[root@hadoop-slave-002 rediscluster]# gem install redis
Fetching: redis-4.0.1.gem (100%)
ERROR:  Error installing redis:
	redis requires Ruby version >= 2.2.2.
```
- ��װ2.2���ϰ汾ruby
    
    ɾ������ruby�汾
    ```
    yum remove ruby
    ```
    ��С������װ��
    ��װ������
    ```
    yum -y groupinstall "Development Tools"
    yum -y install gdbm-devel libdb4-devel libffi-devel libyaml libyaml-devel ncurses-devel openssl-devel readline-devel tcl-devel
    ```
    ```
    wget http://cache.ruby-lang.org/pub/ruby/2.2/ruby-2.2.3.tar.gz -P /root/rpmbuild/SOURCES
    
    wget https://raw.githubusercontent.com/tjinjin/automate-ruby-rpm/master/ruby22x.spec -P /root/rpmbuild/SPECS
    
    rpmbuild -bb /root/rpmbuild/SPECS/ruby22x.spec
    
    yum -y localinstall /root/rpmbuild/RPMS/x86_64/ruby-2.2.3-1.el7.centos.x86_64.rpm
    ```

    ����
    ```
    [root@hadoop-slave-002 rediscluster]# rpmbuild -bb rpmbuild/SPECS/ruby22x.spec
    bash: rpmbuild: command not found...
    ```
- ��װrmpbuild
    ```
    #���Һ��ʵ�rpm-build��
    yum list |grep rpm-build 
    yum install -y rpm-build.x86_64 
    ```
    ```
    sysparm_processor:SysMeta
    sysparm_scope:x_159404_complianc
    sysparm_want_session_messages:true
    sysparm_method:getTreeNodes
    sysparm_type:column
    sysparm_value:incident
    ni.nolog.x_referer:ignore
    x_referer:x_159404_complianc_SecuredEntities.do
    ```
## M-S��Ⱥ
```
Using 3 masters:
192.168.253.131:7000
192.168.253.131:7001
192.168.253.131:7002
Adding replica 192.168.253.131:7003 to 192.168.253.131:7000
Adding replica 192.168.253.131:7004 to 192.168.253.131:7001
Adding replica 192.168.253.131:7005 to 192.168.253.131:7002
M: ab6ff95643ba1487b8a041a9ec2d6c9260232104 192.168.253.131:7000
   slots:0-5460 (5461 slots) master
M: 0b421aa5855c11375cdd573187ba31815f1a0a9c 192.168.253.131:7001
   slots:5461-10922 (5462 slots) master
M: 6e1f29b6573e6c364203d5c29ef984438af60ce4 192.168.253.131:7002
   slots:10923-16383 (5461 slots) master
S: 70dad75d25f55be03880f304381c9f5f8bc3e3d7 192.168.253.131:7003
   replicates ab6ff95643ba1487b8a041a9ec2d6c9260232104
S: 5272d500c7eea0ea92bdeb6a32ec196f1e7a4056 192.168.253.131:7004
   replicates 0b421aa5855c11375cdd573187ba31815f1a0a9c
S: e39b3c4f8f92549d8e16012052b737ac8b332234 192.168.253.131:7005
   replicates 6e1f29b6573e6c364203d5c29ef984438af60ce4

```

��װ�ɹ���ʾ��16384��ʾ��Ⱥ�е� 16384 ���۶�������һ�����ڵ��ڴ��� ��Ⱥ����������

```
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.

```
## spring redis ��Ⱥ

redis��Ⱥ�汾4.1��spring redis cliet 2.8
����
```
org.springframework.beans.factory.BeanCreationException:
 Error creating bean with name 'jedisCluster' defined in file 
 [C:\Users\lge\Documents\elegant_ssm\target\elegant_ssm_0.0.1.SNAPSHOT_
 20180108\WEB-INF\classes\spring\spring-redis.xml]: Bean instantiation via c
 onstructor failed; nested exception is org.springframework.beans.BeanInstantiationException:
 Failed to instantiate [redis.clients.jedis.JedisCluster]: 
 Constructor threw exception; nested exception is java.lang.NumberFormatException: For input string: "7005@17005"
```
==��redis client2.8��Ϊ2.9,ͬʱ����commons-pool2������==
```
<dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-pool2</artifactId>
            <version>2.3</version>
        </dependency>
```