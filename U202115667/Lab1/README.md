# 实验记录

1.下载docker for Windows

2.下载Openstack Swift 的 docker镜像

3.构造docker镜像docker build -t openstack-swift-docker .

4.准备数据卷docker run -v /srv --name SWIFT_DATA busybox

5.运行容器docker run -d --name openstack-swift -p 12345:8080 --volumes-from SWIFT_DATA -t openstack-swift-docker

6.检查容器docker logs openstack-swift ； docker ps

7.搭建客户端，即安装swiftclient库

8.验证功能swift -A http://127.0.0.1:12345/auth/v1.0 -U test:tester -K testing stat ； swift -A http://127.0.0.1:12345/auth/v1.0 -U test:tester -K testing list

# 错误记录

在步骤6出现报错：

![错误结果](figure/%E5%A4%B1%E8%B4%A5%E7%BB%93%E6%9E%9C.png)

经查阅，可能是库路径问题，于是在Dockerfile文件中添加环境变量设置：ENV LD_LIBRARY_PATH="/usr/local/lib"

重新构造docker镜像，发现构造时间显著增长（数个数量级），按步骤执行得到正确结果：

![正确结果](figure/%E6%89%A7%E8%A1%8C%E7%BB%93%E6%9E%9C.png)
