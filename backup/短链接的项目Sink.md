## 短链接的项目Sink

在Youtube上看到介绍的一个有趣的短链接项目，也是在CF上搭建，但新增了数据统计和分析，页面也做了美观，很有趣。就试了一下。

> 该项目的[github仓库](https://github.com/ccbikai/Sink)，可以按照说明文档一步步的在CF上部署。建好后的界面如下，很简约；数据统计界面可以看到访问量，各种分类统计图标，很有趣。

![sink1](https://github.com/sunbonoy/sunbonoy.github.io/assets/169503861/5b9c589f-58c9-4b9c-9159-8d82ff574d5a)
![sink2](https://github.com/sunbonoy/sunbonoy.github.io/assets/169503861/46c046c3-f02d-40c7-9ae6-646c96ab7186)
![sink3](https://github.com/sunbonoy/sunbonoy.github.io/assets/169503861/48ef9910-8f64-43ca-822e-7c8c99990bfe)

> [!NOTE]
> 
> 1. CF新建pages时候，需要选择Nuxt.js的框架，这个是必须的。
> 
> 2. 三个环境标量需要先添加后再保存部署。
> 
> 3. 部署一定要先取消，然后去绑定KV变量，AI变量，启动ANALYTICS Engine并绑定变量，完全按照说明文档。第一次我在搭建时候把这几个变量功能在前面部署后再增加的，数据统计功能就没法生效。

整体上还是很喜欢这个项目，只是Home页面没法修改，但能换成自己的博客主页。其它都很好，短链接生成还支持到期时间定义。

> [!TIP]
> 项目的[Demo版](https://sink.cool/)，大家可以访问试用。

