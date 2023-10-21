# git上传失败
观前提醒，导致报错的原因有很多，但报错内容有可能一致。所以此方法不一定有用。
***
## 1.报错消息
```text
fatal: unable to access 'https://github.com/Chi-Re/Mindustry-Java-dev-docs.git/': Unknown SSL protocol error in connection to github.com:443 
```

## 2.解决方式
直接打开git命令窗口(随便怎么打开都行)

然后输入:
```text
git config http.sslVerify "false"
```

```text
19:13:39.210: [Mindustry-Java模组开发] git -c core.quotepath=false -c log.showSignature=false push --progress --porcelain origin refs/heads/master:master
Counting objects: 22, done.
Delta compression using up to 4 threads.   
remote: Resolving deltas: 100% (11/11), completed with 4 local objects.        
To https://github.com/Chi-Re/Mindustry-Java-dev-docs.git
 	refs/heads/master:refs/heads/master	3341db9..63cad66
Done
```

然后就完美解决啦。

# 教程来源
[Github报错——Unknown SSL protocol error in connection to github.com:443](https://blog.csdn.net/pary__for/article/details/114393613)

