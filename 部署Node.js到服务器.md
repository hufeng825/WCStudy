### 部署Node.js项目(Ubuntu16.04)
#### Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境，用来方便地搭建快速的易于扩展的网络应用。Node.js 使用了一个事件驱动、非阻塞式 I/O 的模型，使其轻量又高效，非常适合运行在分布式设备的数据密集型的实时应用。Node.js 的包管理器 npm，是全球最大的开源库生态系统。

### 基本流程
- ssh连接到云服务器 
 
- 安装NVM，NVM是Node.js的版本管理软件。安装好git的前提下

```
git clone https://github.com/cnpm/nvm.git ~/.nvm && cd ~/.nvm && git checkout `git describe --abbrev=0 --tags`
```
- 激活NVM 

```echo ". ~/.nvm/nvm.sh" >> /etc/profile ```

```source /etc/profile```

- 列出Node.js的所有版本。

```nvm list-remote```

- 安装指定版本

```nvm install v7.4.0```

- 部署测试项目

```cd ~```

```touch exampleApi.js```

- 编辑exampleApi.js ```vim exampleApi.js```

- 运行项目

```
const http = require('http');
const hostname = '0.0.0.0';
const port = 3389;
var data = {
    'code':'000',
    'message':'message',
    'lists':[
        {
          'name':'lilei',
            'age': '12',
            'sex':'M'
        },{
          'name':'hanmeimei',
            'age': '112',
            'sex':'F'
        }
    ]

}
const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/json');
  res.end(JSON.stringify(data));
});
server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```

```node ~/exampleApi.js```

- 后台运行 

```node ~/exampleApi.js &```

- 访问<http://47.98.177.125:3389/>

### 补充：forever也能实现后台运行，而且还能记录输出和错误日志  

1.安装
```
npm install forever -g
```

2.启动服务
```
service forever start
```

3.使用forever启动js文件
```
forever start exampleApi.js
```

4.停止js文件

```
forever stop app.js
```

5.启动js文件并输出日志文件

```
forever start -l forever.log -o out.log -e err.log exampleApi.js
```

6.重启js文件

```
7.forever restart exampleApi.js
```

8.查看正在运行的进程

```
forever list
```

- 查看效果

![](http://p2bzzkn05.bkt.clouddn.com/18-6-30/49287068.jpg)

### 补充：
#### 查看端口使用情况
```ps -ef | grep node```
#### 查看某一个端口
```
netstat -ap | grep 3389
```
eg:

```
root@ubuntu:~# netstat -ap | grep 3389
tcp        0      0 *:3389                  *:*                     LISTEN      6162/node
```

#### Linux关闭nodejs服务
```
1、ps -ef | grep node

查看node对应的pid，然后kill pid，再进入对应项目npm start

2、如果以上方法不行可以这样：

kill node 或者kilall node
```
eg:

```root@ubuntu:~# ps -ef | grep node
root     30080 28754  1 15:42 pts/4    00:00:00 node exampleApi.js
root     30087 28754  0 15:42 pts/4    00:00:00 grep --color=auto node
root@ubuntu:~# kill 30080
[1]+  Terminated              node exampleApi.js
root@ubuntu:~# ps -ef | grep node
root      6162  5422  0 12:28 pts/1    00:00:00 node exampleApi.js
root      6195  5422  0 12:42 pts/1    00:00:00 grep --color=auto node

```

