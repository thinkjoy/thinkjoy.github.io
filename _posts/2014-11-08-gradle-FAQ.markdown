---
layout: post
title:  "gradle使用FAQ"
date:   2014-11-09 00:12:00
categories: jekyll update
---

###SNAPSHOT包更新问题
在gradle管理下，SNAPSHOT的包有更新并上传到maven服务器后，并不会自动下载最新的包，可以在gradle工程根目录下执行
{% highlight ruby %}
gradle -ai --refresh=dependencies
{% endhighlight %}


如果使用IDEA这个IDE，可以执行以下命令
{% highlight ruby %}
gradle -ai --refresh=dependencies ideaModule
{% endhighlight %}
                      


###容器包含包剔除问题
例如 servlet-api.jar此类，web容器会提供，但在项目编译时也需要，可以通过如下配置解决：

* 和 war plugin配套使用
{% highlight ruby %}
apply plugin: 'war'

dependencies {
  providedCompile 'javax.servlet:javax.servlet-api:3.1.0'
}
{% endhighlight %}

* 仅有jar plugin的情况
{% highlight ruby %}
configurations { providedCompile }

dependencies {
    providedCompile "javax.servlet:javax.servlet-api:3.+"
}

sourceSets.main.compileClasspath += configurations.providedCompile
sourceSets.test.compileClasspath += configurations.providedCompile
sourceSets.test.runtimeClasspath += configurations.providedCompile
{% endhighlight %}


另外：需要注意 providedCompile会将此jar依赖的包也不会打进部署包，如果需要 需要加上@jar。例如
{% highlight groovy %}
providedCompile  'javax.servlet:javax.servlet-api:3.1.0@jar'
{% endhighlight %}


...待续