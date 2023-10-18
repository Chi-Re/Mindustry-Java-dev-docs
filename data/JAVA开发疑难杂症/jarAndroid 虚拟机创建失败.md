# 虚拟机创建失败
***
## 1.报错消息
阅读之前请切记，这个方法有不确定性，慎用。

并且可能导致其他jar文件运行失败。

### :jar
```text
> Task :jar UP-TO-DATE
Error: Could not create the Java Virtual Machine.
Could not create the Java Virtual Machine.

-Djava.ext.dirs=F:\android-sdk-windows\build-tools\29.0.3\lib is not supported.  Use -classpath instead.
Error: A fatal exception has occurred. Program will exit.
A fatal exception has occurred. Program will exit.
```

### [jarAndroid]
```text
上午 1:04:08: 正在执行 'jarAndroid'…

aliyun repositories
aliyun allprojects CrimsonStar
> Task :compileJava UP-TO-DATE
> Task :processResources NO-SOURCE
> Task :classes UP-TO-DATE
> Task :jar UP-TO-DATE
Error: Could not create the Java Virtual Machine.
Error: A fatal exception has occurred. Program will exit.
-Djava.ext.dirs=F:\android-sdk-windows\build-tools\29.0.3\lib is not supported.  Use -classpath instead.
> Task :jarAndroid

Deprecated Gradle features were used in this build, making it incompatible with Gradle 9.0.

You can use '--warning-mode all' to show the individual deprecation warnings and determine if they come from your own scripts or plugins.

For more on this, please refer to https://docs.gradle.org/8.2.1/userguide/command_line_interface.html#sec:command_line_warnings in the Gradle documentation.

BUILD SUCCESSFUL in 1s
3 actionable tasks: 1 executed, 2 up-to-date
上午 1:04:10: 执行完成 'jarAndroid'。
```

## 2.解决方式
注意这里的报错消息
```text
-Djava.ext.dirs=F:\android-sdk-windows\build-tools\29.0.3\lib is not supported.  Use -classpath instead.
```
看来是因为`-Djava.ext.dirs`的问题

那就好办了

请找到`android-sdk`的文件路径，在`\build-tools\29.0.3`文件下的`d8.bat`文件。

打开后，你会在最后一行看到
```text
call "%java_exe%" %javaOpts% -Djava.ext.dirs="%frameworkdir%" -cp "%jarpath%" com.android.tools.r8.D8 %params%
```
更改`-Djava.ext.dirs=`为`-classpath`。

注意！`-classpath`后面是有空格，然后再是`"%frameworkdir%"`


更改后
```text
call "%java_exe%" %javaOpts% -classpath "%frameworkdir%" -cp "%jarpath%" com.android.tools.r8.D8 %params%
```

```text
上午 12:24:40: 正在执行 'deploy'…

aliyun repositories
aliyun allprojects CrimsonStar
> Task :compileJava UP-TO-DATE
> Task :processResources NO-SOURCE
> Task :classes UP-TO-DATE
> Task :jar
> Task :jarAndroid
> Task :deploy

Deprecated Gradle features were used in this build, making it incompatible with Gradle 9.0.

You can use '--warning-mode all' to show the individual deprecation warnings and determine if they come from your own scripts or plugins.

For more on this, please refer to https://docs.gradle.org/8.2.1/userguide/command_line_interface.html#sec:command_line_warnings in the Gradle documentation.

BUILD SUCCESSFUL in 7s
4 actionable tasks: 3 executed, 1 up-to-date
上午 12:24:48: 执行完成 'deploy'。
```


[//]: # (## 3.解决方式&#40;不建议&#41;)

[//]: # (请在```{JAVA安装路径}\jdk-17\lib```下找到```jvm.cfg```，这是jvm的配置文件)

[//]: # ()
[//]: # (注意！！！)

[//]: # ()
[//]: # (这个文件十分重要，尽量不要顺便更改)

[//]: # ()
[//]: # ()
[//]: # (一般，文件的内容为)

[//]: # (```text)

[//]: # (-server KNOWN)

[//]: # (-client IGNORE)

[//]: # (```)

[//]: # ()
[//]: # (我们在后面加上)

[//]: # (```text)

[//]: # (-Xms2048m)

[//]: # (-Xmx4096m)

[//]: # (-XX:PermSize=1024M )

[//]: # (-XX:MaxPermSize=2048M)

[//]: # (```)

[//]: # (就大功告成了)

[//]: # ()
[//]: # (不过再运行的话可能会有别的报错)

[//]: # (### :jar)

[//]: # (```text)

[//]: # (Error: keywords 'java|openjdk version' not found in 'warning: no leading - on line 3 ')

[//]: # (Error: keywords 'java|openjdk version' not found in 'warning: no leading - on line 3 ')

[//]: # (Error: keywords 'java|openjdk version' not found in 'warning: no leading - on line 3 ')

[//]: # (keywords 'java|openjdk version' not found in 'warning: no leading - on line 3 ')

[//]: # ()
[//]: # (Error: keywords 'java|openjdk version' not found in 'warning: no leading - on line 3 ')

[//]: # (keywords 'java|openjdk version' not found in 'warning: no leading - on line 3 ')

[//]: # ()
[//]: # (Error: keywords 'java|openjdk version' not found in 'warning: no leading - on line 3 ')

[//]: # (Error: keywords 'java|openjdk version' not found in 'warning: no leading - on line 3 ')

[//]: # (```)

[//]: # (不过似乎不会影响正常打包，请使用```:deploy```打包。)

[//]: # ()
[//]: # (### :deploy)

[//]: # (```text)

[//]: # (上午 1:14:54: 正在执行 'deploy'…)

[//]: # ()
[//]: # (aliyun repositories)

[//]: # (aliyun allprojects CrimsonStar)

[//]: # (> Task :compileJava UP-TO-DATE)

[//]: # (> Task :processResources NO-SOURCE)

[//]: # (> Task :classes UP-TO-DATE)

[//]: # (> Task :jar UP-TO-DATE)

[//]: # (Error: keywords 'java|openjdk version' not found in 'warning: no leading - on line 3 ')

[//]: # (Error: keywords 'java|openjdk version' not found in 'warning: no leading - on line 3 ')

[//]: # (Error: keywords 'java|openjdk version' not found in 'warning: no leading - on line 3 ')

[//]: # (Error: keywords 'java|openjdk version' not found in 'warning: no leading - on line 3 ')

[//]: # (Error: keywords 'java|openjdk version' not found in 'warning: no leading - on line 3 ')

[//]: # (Error: keywords 'java|openjdk version' not found in 'warning: no leading - on line 3 ')

[//]: # (> Task :jarAndroid)

[//]: # (> Task :deploy)

[//]: # ()
[//]: # (Deprecated Gradle features were used in this build, making it incompatible with Gradle 9.0.)

[//]: # ()
[//]: # (You can use '--warning-mode all' to show the individual deprecation warnings and determine if they come from your own scripts or plugins.)

[//]: # ()
[//]: # (For more on this, please refer to https://docs.gradle.org/8.2.1/userguide/command_line_interface.html#sec:command_line_warnings in the Gradle documentation.)

[//]: # ()
[//]: # (BUILD SUCCESSFUL in 5s)

[//]: # (4 actionable tasks: 2 executed, 2 up-to-date)

[//]: # (上午 1:15:00: 执行完成 'deploy'。)

[//]: # (```)

[//]: # ()
[//]: # (仍然，这个方法不确定性很高，记得注意。)

[//]: # (<br>)

[//]: # (<br>)

[//]: # (<br>)

[//]: # (<br>)

[//]: # (如果你觉得上面的报错特别烦人的话，可以试试以下方法)

[//]: # ()
[//]: # (同样的，请在```{JAVA安装路径}\jdk-17\lib```下找到```jvm.cfg```，这是jvm的配置文件)

[//]: # ()
[//]: # (将文件内所有参数删除后，添加如下参数：)

[//]: # (```text)

[//]: # (-j9vm KNOWN)

[//]: # (-hotspot IGNORE)

[//]: # (-classic IGNORE)

[//]: # (-native IGNORE)

[//]: # (-green IGNORE)

[//]: # (```)

[//]: # ()
[//]: # (这个方法同样可以解决`:jarAndroid`报错。)

[//]: # (```text)

[//]: # (下午 10:29:11: 正在执行 'jarAndroid'…)

[//]: # ()
[//]: # (aliyun repositories)

[//]: # (aliyun allprojects CrimsonStar)

[//]: # (> Task :compileJava UP-TO-DATE)

[//]: # (> Task :processResources NO-SOURCE)

[//]: # (> Task :classes UP-TO-DATE)

[//]: # (> Task :jar)

[//]: # (> Task :jarAndroid)

[//]: # ()
[//]: # (Deprecated Gradle features were used in this build, making it incompatible with Gradle 9.0.)

[//]: # ()
[//]: # (You can use '--warning-mode all' to show the individual deprecation warnings and determine if they come from your own scripts or plugins.)

[//]: # ()
[//]: # (For more on this, please refer to https://docs.gradle.org/8.2.1/userguide/command_line_interface.html#sec:command_line_warnings in the Gradle documentation.)

[//]: # ()
[//]: # (BUILD SUCCESSFUL in 4s)

[//]: # (3 actionable tasks: 2 executed, 1 up-to-date)

[//]: # (下午 10:29:16: 执行完成 'jarAndroid'。)

[//]: # (```)

[//]: # ()
[//]: # (注意！！！)

[//]: # ()
[//]: # (这个方法会导致命令行中`java -version`产生报错)

[//]: # (```text)

[//]: # (Error: missing `j9vm' JVM at `C:\Program Files\Java\jdk-17\bin\j9vm\jvm.dll'.)

[//]: # (Please install or use the JRE or JDK that contains these missing components.)

[//]: # (```)

[//]: # ()
[//]: # (结果就是你的mindustry启动不了了...&#40;不过如果你是臭猫的压缩包安装的话没事&#41;)

[//]: # ()
[//]: # ()
