---
layout: single
title: Rails中的测试那些事  
date: 2010-12-10 23:23:33
categories: [ruby, rails, test]
comments: true
---                        

这个本早该记一下的。

现在RUBY社区发展得太快了，各种新技术层出不穷，测试方面同样如此。

我记下我在Rails3项目中所用的测试方面的技术点。

首先是rspec-rails，它是rspec 2的对rails3项目支持的一个gem，只需要在Gemfile中指定rspec-rails就可以了，不用指定rspec2依赖。

然后我还要用到factorygirl-rails，它同样是factorygirl对于rails3项目支持的一个gem，只需要在Gemfile中指定factorygirl-rails就可以了，不用指定factorygirl依赖。

还有一个rspec的match封装gem，remarkable/active_record，用它可以测试 validates,association(关联等)，说到这里，“测试”这个词我觉得不合适，我用到来感觉这个gem太强大了，它简直就是一个代码监管的工具。

另外，[ffaker](https://github.com/EmmanuelOga/ffaker) 这个是用来产生随机数据的gem，它可以用来产生email, name, company name等，另外我提交了可以产生中文姓名的[代码](https://github.com/EmmanuelOga/ffaker/pull/9) ，可是到现在作者还没有将其融合进去，真希望他快点啊。

其它的如cucumber太过先进的东西我还没有在项目中常用。感觉上面介绍的组合就够我用了。

最后持续集成测试方面我还没有在项目中用到，有时间的时候我打算研究一下 [cinabox](https://github.com/rails/rails/blob/master/ci/ci_setup_notes.txt) ，为什么要研究这个呢，因为我看到rails项目中就是到的这个来作为它的持续测试的工具，它是基本ubuntu系统上的一个项目，看介绍，安装很方便的，到时候试试。

我以下写下这些，一是作了大致的记录，另外也防止自己out了，如果大家有更好用的工具，也请分享一下。
