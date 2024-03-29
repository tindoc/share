# 其他

## 零零散散
1. 绿色安装版软件
    
    使用非 exe 程序安装的软件，多数只要 **解压+设置环境变量** 就可以正常使用，例如：jdk, Tomcat, Maven
    
2. 如何配置环境变量（用命令行实现）？（依然存在问题，建议使用最后的方法设置）

    - Linux: `setenv('JAVA_HOME','C:\Java\jdk<版本>');` + `setenv('PATH', [getenv('PATH') ';目录\bin']);` （当然，目录可以是 环境变量名，例如 `%JAVA_HOME%\bin`）
    - Windows: `setx JAVA_HOME "C:\Java\jdk<版本>" /M` + `setx PATH "%PATH%;目录\bin" /M` 

        > 其中，`/M` 指明为设置系统变量而不是本地(用户)环境变量，加双引号是避免 Windows 目录中空格的影响。[参考](https://docs.microsoft.com/zh-cn/windows-server/administration/windows-commands/setx)。另，设置后在当前终端使用 `echo %JAVA_HOME%` 会显示设置前的值，需要打开一个新的终端才可查看到
        >
        > 可能会出现权限和需要重启系统的问题，如果实在不行请使用 `鼠标右键“我的电脑”->属性->高级系统设置->环境变量` 来添加

5. XML 文件的特殊字符 `&`

    `&` 是 XML 中的特殊字符之一，需要使用需用 `&amp;` 代替

## 用户文档/使用文档的编写安排

1. 如何使用本文档（对本文档使用方法的建议）
2. 系统简介（简单介绍系统的功能，与业务对应）
3. 相关术语（介绍本文档中可能难以理解或容易误解的名词）
4. 用户权限（使用用户权限对应系统菜单栏功能介绍）
5. 首次使用（系统的初始化，在所有用户能够正常使用前做的准备工作）
6. 使用场景（日常使用时针对使用场景对应的系统操作，按照 查看/添加/更新/删除 的顺序对有关业务介绍）
7. 注意事项（介绍一些注意事项）

## 代码探索方法
> form [link](https://zhuanlan.zhihu.com/p/68012293)
- 善用各类工具：包括但不限于：代码阅读工具，版本管理工具，代码检查工具，压力测试工具，性能分析工具，内存泄漏检测工具。
- 常用原则：看到递归就要想到用循环去重构；看到循环就要考虑是否会死循环；看到数组就要考虑是否越界；看到指针就要考虑是否正确析构；看到多线程就要考虑是否线程安全；看到频繁内存申请就要想到内存池。
- 了解内在原理：函数调用时栈的状态；对象析构的时机；高级语言中的可变对象与不可变对象；Map/Set/HashMap的存储结构；Map Reduce的原理；文件分布式存储的原理；画出完整的模块关系图和数据流图。
- 不放过任何Warning/异常：写出无Warning的代码，包括编译的Warning和各种检查工具的Warning；合理打印Warning日志。
- 善用搜索：能够从国内/外站中搜索问题。
- 记笔记：将每一个问题的追查过程记录下来，定期回顾与分享。

## 换源这件事（for Linux）
### 国内 Mirrors 站点
- [清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/) 

    所有的收录的源都有 [使用说明](https://mirrors.tuna.tsinghua.edu.cn/help/AOSP/)
- [阿里云开源镜像站](https://developer.aliyun.com/mirror/)
- [腾讯软件源](https://mirrors.cloud.tencent.com/) 
    
    少数收录的源有使用说明

### Maven
`settings.xml` Mirrors 段添加以下内容，建议修改 `~/.m2/settings.xml`
``` xml
<mirror>
    <id>aliyunmaven</id>
    <mirrorOf>*</mirrorOf> <!-- 使用 * 需要 Maven 2.0.5+ -->
    <name>阿里云公共仓库</name>
    <url>https://maven.aliyun.com/repository/public</url>
</mirror>
```

参考：
- [Introduction to Repositories](https://maven.apache.org/guides/introduction/introduction-to-repositories.html)
- [Using Mirrors for Repositories](https://maven.apache.org/guides/mini/guide-mirror-settings.html)
- [Best Practice - Using a Repository Manager](https://maven.apache.org/repository-management.html)

> [阿里云在线包搜索](https://developer.aliyun.com/mvn/search)

### pip
- 设为默认 `pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple` 要求 **pip >= 10.0.0**

    非 root 用户使用 `pip config set` 会创建/修改 `~/.config/pip/pip.conf` 文件
- 临时使用 `pip install <package> -i https://pypi.tuna.tsinghua.edu.cn/simple`

参考：
- [https://pip.pypa.io/en/stable/cli/pip_config/#description](https://pip.pypa.io/en/stable/cli/pip_config/#description) 
- [https://pip.pypa.io/en/stable/topics/configuration/#naming](https://pip.pypa.io/en/stable/topics/configuration/#naming)
- [Using a Proxy Server](https://pip.pypa.io/en/stable/user_guide/#using-a-proxy-server)

### pacman for Manjaro Linux
- 命令行 `sudo pacman-mirrors -c China -g` （建议先备份 `/etc/pacman.d/mirrorlist` 文件） 
    
    会根据 `-c` 指定的国家获取 mirrorlist 并自动修改 `/etc/pacman.d/mirrorlist` 里面的所有内容
- 通过应用 *Add/Remove Software* 设置 `Preferences -> General -> Use mirror from -> 选择 China 即可` 

参考：
- [man pacman-mirrors](https://wiki.manjaro.org/index.php/Pacman-mirrors/zh-cn)

### Docker registry
> 貌似官方的速度可以，实在不行再考虑修改

1. 新建或者修改 `/etc/docker/daemon.json` 添加一下内容
    ``` json
    {
    "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn/"]
    }
    ```
2. 执行 `sudo systemctl restart docker` 重启 docker 服务

### Alpine Linux
- `sed -i 's/dl-cdn.alpinelinux.org/mirrors.tuna.tsinghua.edu.cn/g' /etc/apk/repositories`


参考：
- [清华大学开源软件镜像站-使用说明](https://mirrors.tuna.tsinghua.edu.cn/help/alpine/)