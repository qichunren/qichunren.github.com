---
layout: post
title: 在Rails项目中使用compass来管理css 
date: 2010-12-23 23:12:12
comments: true
categories: [rails, css]
---                            

对如今的web项目开发（个人开发），用Rails来快速实现，但是前端这一块会花掉比较多的时间，其中CSS的管理维护是一个越来越麻烦的事情，由于它只是一门单纯的静态标记的语法（说法不准确），我的意思是，比如我设计一个三栏的网页，页面宽度是960px，那么每栏应该是960px的三分之一宽度320px，在css文件中，需要“硬编码”写入单栏的宽度： .box{width:320px;} 
   然后有一天，某某人说要改网站页面宽度，要调成1200px，这样需要找出css文件中相关的地方都要改成新的400px的宽度。对我来说，做这种事情一点也不爽。如果能自动计算出每栏是网页宽度的三分之一那多好。 [Sass](http://sass-lang.com/) 就是做这事情的，而 [Compass](https://github.com/chriseppstein/compass) 是基于Sass之上的一个集成，它让我们在Rails项目中可以更为容易地使用sass，它集成了如 [blueprint](http://www.blueprintcss.org/) 这样的css框架。 
   以上都是废话，哈哈，看看在项目中如何使用起compass，通过它来高级地管理项目的css。 
   # 在Gemfile中加入gem "compass", 然后bundle install # 在项目的根目录执行compass init rails . 
   # 以上两步搭建好了基础，接下来就是编写sass和引入css文件. 
   # in application.html.erb 
{% highlight html %}
   <%= stylesheet_link_tag "compiled/screen.css", :media => "screen, projection" %> <%= stylesheet_link_tag "compiled/print.css", :media => "print" %> 
   接上开头说的那个问题，我可以在/app/stylesheets/screen.scss中这样来写：
    
   $page_width: 960px; body { width: $page_width; } .box { width: $page_width / 3; } 
{% endhighlight %}
   具体的操作信息从我这里可以看出：    
   
```
usermatoMacBook-Pro:weixiaoshuo qichunren$ compass init rails .
Compass recommends that you keep your stylesheets in app/stylesheets
instead of the Sass default location of public/stylesheets/sass.
Is this OK? (Y/n) y

Compass recommends that you keep your compiled css in public/stylesheets/compiled/
instead the Sass default of public/stylesheets/.
However, if you're exclusively using Sass, then public/stylesheets/ is recommended.
Emit compiled stylesheets to public/stylesheets/compiled/? (Y/n) y
directory ./app/stylesheets/
directory ./public/stylesheets/compiled/
 exists ./config
 create ./config/compass.rb
 exists ./config/initializers
 create ./config/initializers/compass.rb
convert screen.sass
 create ./app/stylesheets/screen.scss
convert print.sass
 create ./app/stylesheets/print.scss
convert ie.sass
 create ./app/stylesheets/ie.scss

Congratulations! Your rails project has been configured to use Compass.
Just one more thing left to do: Register the compass gem.

In Rails 2.2 & 2.3, add the following to your environment.rb:

config.gem "compass", :version => ">= 0.10.6"

In Rails 3, add the following to your Gemfile:

gem "compass", ">= 0.10.6"

Then, make sure you restart your server.

Sass will automatically compile your stylesheets during the next
page request and keep them up to date when they change.

Next add these lines to the head of your layouts:
%head
= stylesheet_link_tag 'compiled/screen.css', :media => 'screen, projection'
= stylesheet_link_tag 'compiled/print.css', :media => 'print'
/[if IE]
  = stylesheet_link_tag 'compiled/ie.css', :media => 'screen, projection'

(You are using haml, aren't you?)
usermatoMacBook-Pro:weixiaoshuo qichunren$  
```