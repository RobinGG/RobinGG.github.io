---
layout: post
title: 安装 Jekyll
date: 2017-07-20
tags: [jekyll]
---

我的这个博客就是通过 Jekyll 搭起来的，折腾 vps 的时候把 Jekyll 给弄没了。得，又得重新装，索性记录一下是怎么装起来的。

## 安装 Ruby

根据官网的安装文档，是需要 Ruby 的，所以上来先装个 Ruby。这里我们用第三方包 `ruby-install`

### 安装 ruby-install

ruby-install 的 github 主页：
[ruby-install](https://github.com/postmodern/ruby-install)

你可以按照他推荐的方式安装 ruby-install：

~~~shell
wget -O ruby-install-0.6.1.tar.gz https://github.com/postmodern/ruby-install/archive/v0.6.1.tar.gz
tar -xzvf ruby-install-0.6.1.tar.gz
cd ruby-install-0.6.1/
sudo make install
~~~

事实上，我们在下载压缩包解压后直接运行 ruby-install 脚本也是没有问题的，所以不一定要运行 install 命令。


### 通过 ruby-install 安装 ruby

通过 ruby-install 去安装 ruby 就很简单了。直接运行

~~~shell
# 如果你运行了 install 命令的话
ruby-install --system ruby

# 如果你只是解压了压缩包
cd ruby-install-0.6.1/bin/
chmod +x ruby-install
./ruby-install --system ruby
~~~

接下来就安静地等待就可以了。会需要下载源码，然后编译，最后安装。需要一定的时间，可以离开座位接杯水喝。

## 安装 Jekyll

### 安装本体

运行：

~~~shell
gem install jekyll
~~~

完事，大功告成

### 安装插件

事实上，根据你的 jekyll theme 的不同，会依赖不同的插件。所以如果只安装 jekyll 启动是会报错。
这个时候我们先通过 RubyGem 安装 bundler，然后通过 bundler 自动安装注明在 Gemfile 里面的那些插件。

~~~shell
gem install bundler
bundle install
~~~

如果作者没有将插件写入 Gemfile，却又实实在在在 _config.yml 里面用了，那就需要手动去安装插件了。

~~~shell
gem install <jekyll-plugin>
~~~
