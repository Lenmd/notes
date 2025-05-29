
下载

配置环境变量
```
GRADLE_HOME

gradle -v

配置用户目录
GRADLE_USER_HOME   D:\repos\mavenrepos  gradle本地库和maven本地库公用一个
```

创建项目

```
命令行方式
gradle init

```

C:\Soft\Dev\gradle-7.6.4\init.d 下添加gradle初始化脚本
文件以.gradle结尾

``` java
allprojects {
    repositories {
	    mavenLocal
        // 使用阿里云Maven镜像
        maven { url 'https://maven.aliyun.com/repository/public/' }
        maven { url 'https://maven.aliyun.com/repository/central/' }
        maven { url 'https://maven.aliyun.com/repository/google/' }
        maven { url 'https://maven.aliyun.com/repository/gradle-plugin/' }
        
        // 保留默认的Maven Central仓库
        mavenCentral()
    }
	buildscript{
		repositories {
			 maven { url 'https://maven.aliyun.com/repository/public/' }
	        maven { url 'https://maven.aliyun.com/repository/central/' }
	        maven { url 'https://maven.aliyun.com/repository/google/' }
	        maven { url 'https://maven.aliyun.com/repository/gradle-plugin/' }
		}
	}
}
```