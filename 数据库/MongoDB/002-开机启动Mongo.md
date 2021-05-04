## Mac OS 的开机启动方式

`launchd` 是 Mac OS 下用于初始化系统环境的关键进程，它是内核装载成功之后在OS环境下启动的第一个进程。其实它的作用就是我们平时说的守护进程，简单来说，用户守护进程是作为系统的一部分运行在后台的非图形化程序。采用这种方式来配置自启动项很简单，只需要一个plist文件，该plist文件存在的目录有:

- LaunchDaemons `~/Library/LaunchDaemons`
  用户登陆前运行 plist（程序）
- LaunchAgents `~/Library/LaunchAgents`
  用户登录后运行相应的 plist（程序）

你需要`.plist`文件来指定需要开机启动的程序。首先来创建一个.plist文件：

```
sudo vim /Library/LaunchDaemons/org.mongodb.mongod.plist
```

以下是开机启动的.plist配置文件的示例：

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN""http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
    <dict>
        <key>Label</key>
        <string>org.mongodb.mongod</string>
        <key>ProgramArguments</key>
        <array>
            <string>/usr/local/mongodb-2.0.3/bin/mongod</string>
            <string>run</string>
            <string>--config</string>
            <string>/usr/local/mongodb-2.0.3/mongod.conf</string>
        </array>
        <key>RunAtLoad</key>
        <true/>
        <key>KeepAlive</key>
        <true/>
        <key>WorkingDirectory</key>
        <string>/usr/local/mongodb-2.0.3</string>
        <key>StandardErrorPath</key>
        <string>/usr/local/mongodb-2.0.3/log/error.log</string>
        <key>StandardOutPath</key>
        <string>/usr/local/mongodb-2.0.3/log/mongo.log</string>
    </dict>
</plist>
```

粘贴完以后，我们需要对上面的内容做两处修改：一个是“redis-server”的路径，这里需要改为你自己的安装路径(可以使用”which redis-server”命令来查看具体的路径)。还有一个就是redis配置文件的路径，这个是可选的，如果没有使用配置文件那么就把第二个删除，如果使用了配置文件，一定要改为正确的路径。

##  将plist文件载入到launchd

编辑完plist文件以后，我们需要将该文件载入到launchd里，使用launchctl命令，具体命令如下：

```
sudo launchctl load /Library/LaunchDaemons/org.mongodb.mongod.plist
```

重启以后，redis就可以开机启动了。如果不想重新启动，也可以使用如下命令：

```
sudo launchctl start org.mongodb.mongod
```

如果想关闭redis的话，使用如下命令：

```
sudo launchctl stop org.mongodb.mongod
```

为了使用方便，我们可以给redis的开启和关闭命令设置别名：

```
alias mongodbstart='sudo launchctl start org.mongodb.mongod' 
alias mongodbstop='sudo launchctl stop org.mongodb.mongod'
```

 参考：

1、https://www.teakki.com/p/57e224390906a5d20a8f90d3

2、https://www.jianshu.com/p/e73978416920