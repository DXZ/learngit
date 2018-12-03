# 日志相关
### 日志相关采集
（目前仅处理了error日志）
#### 服务业务日志采集  
#### 微服务nginx access日志  error 采集


## TODO
## nginx 配置统一化
##### openresty版本配置等模板化

## 日志存储范围
业务日志 
	1个月
访问日志
	半年



## 目的
统一查看
统一报警

业务日志先不考虑，且处理 nginx 日志



## nginx环境标准化及日志相关
### nginx 环境配置相关
1. 安装openresty 版本1.13.6.2
 - `wget https://openresty.org/download/openresty-1.13.6.2.tar.gz`
 - `tar -xzf openresty-1.9.7.4.tar.gz  && cd ./openresty-1.13.6.2`
 - `./configure --prefix=/usr/local/ \
            --with-luajit \
            --without-http_redis2_module \
            --with-http_iconv_module`
2. 安装 lua kafka 扩展 及lor
	- `wget https://github.com/doujiang24/lua-resty-kafka/archive/master.zip`
	- `unzip lua-resty-kafka-master.zip && cd ./lua-resty-kafka-master`
	- `cp -r lib/resty/kafka /usr/local/openresty/lualib/resty/`


## 日志处理
#### 概述
>随着微服务的添加，日志归档查询问题越发麻烦
>主要是人懒，想尽可能简单易用，通用可扩展性也有一定要求
#### 需求
这边的需求体现在服务多，日志种类比较杂，格式也尚未统一
1. nginx 相关日志
	- 预先需要标准化nginx 日志config
	- 相关调优参数也可一并处理
2. 业务日志

#### 方案
- 采取logkit直接采集
    - 日志切割工作由业务方自行负责
    - 清理工作可以由logkit 处理
    - logkit 统一管理业务集群及error log
 


- nginx log
	1.  如完成了切割行为可按logkit完成
	2.  通过luakafka直接搜集  
		- 好处  统一处理  稳定 磁盘io瓶颈可扩展
	- 坏处 安装周边生态有点麻烦，需要依赖openresty生态（相关包+代码）
  3. 通过rsyslog 中间过渡收集 集中处理 再又 logkit 接入
	  - 好处 配置简单易用   无周边依赖(nginx原生支持syslog协议)
	  - 坏处 
