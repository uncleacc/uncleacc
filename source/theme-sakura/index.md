---
title: theme-sakura
comments: true
date: 2020-03-29 22:53:25
keywords: Hexo 主题 Sakura 🌸
description:
photos: https://cdn.jsdelivr.net/gh/uncleacc/img/custom/5.webp
---
Hexo主题Sakura修改自WordPress主题[Sakura](https://github.com/mashirozx/Sakura/)，感谢原作者[Mashiro](https://2heng.xin/)

hexo-theme-sakura主题 English document

基于WordPress主题Sakura修改成Hexo的主题。

demo预览

正在开发中......
#  交流群 
若你是使用者，加群QQ: 801511924

若你是创作者，加群QQ: 194472590
# 主题特性 

- 首页大屏视频

- 首页随机封面
- 图片懒加载
- valine评论
- fancy-box相册
- pjax支持，音乐不间断
- aplayer音乐播放器
- 多级导航菜单（按现在大部分hexo主题来说，这也算是个特性了）

# 三、引用
在引用的文字前加>即可。引用也可以嵌套，如加两个>>三个>>>

n个...

貌似可以一直加下去，但没神马卵用

#  未完善的使用教程 
那啥？老实说我目前也不是很有条理233333333~
#  1、主题下载安装 
**下载**

hexo-theme-sakura下载压缩包格式，因为除了主题内容还有些source的配置对新手来说比较太麻烦，直接下载解压就省去这些麻烦。

**解压**

下载好后解压到*博客根目录*（不是主题目录哦，重复的选择替换）。

**安装依赖**

接着在命令行（cmd、bash）运行npm i安装依赖。

    $ npm i

 # 2、主题配置 
我们建议你使用专业的编辑器进行修改，例如：vscode
## 博客根目录下的_config配置 
**站点信息**
 
    # Site
    title: 你的站点名
    subtitle:
    description: 站点简介
    keywords:
    author: 作者名
    language: zh-cn
    timezone:
 **部署信息**
 
 这段大概在_config的80行

    deploy:
    type: git
    repo: 
        github: 你的github仓库地址
        # coding: 你的coding仓库地址
    branch: master

备份

（使用hexo b发布备份到远程仓库）

我们为你直接提供了懒人式的备份配置，只需要修改下面的信息，大概在_config的88行

推荐你使用分支的方式进行备份，在你的项目仓库新建一个名为backup的分支即可

    backup:
    type: git
    message: backup my blog of https://honjun.github.io/
    repository:
        # 你的github仓库地址,备份分支名  （建议新建backup分支）
        github: https://github.com/honjun/honjun.github.io.git,backup
        # coding: https://git.coding.net/hojun/hojun.git,backup

主题目录下的_config配置

    注意：标明【改】的是需要修改部分，标明【选】是可改可不改，标明【非】是不用改的部分

**站点名**

# site name
# 站点名 【改】
    prefixName: さくら荘その
    siteName: hojun

标签图标和头像

# favicon and site master avatar
# 站点的favicon和头像 输入图片路径（下面的配置是都是cdn的相对路径，没有cdn请填写完整路径，建议使用jsdeliver搭建一个cdn啦，先去下载我的cdn替换下图片就行了，简单方便~）【改】
    favicon: /images/favicon.ico
    avatar: /img/custom/avatar.jpg

**站点相关**

    注意：如果你不修改你的cdn地址，你将无法修改任何图片

# 站点url 【改】
    url: https://sakura.hojun.cn   

# 站点介绍（或者说是个人签名）【改】
    description: Live your life with passion! With some drive!

# 站点cdn，没有就为空 【改】  若是cdn为空，一些图片地址就要填完整地址了，比如之前avatar就要填https://cdn.jsdelivr.net/gh/honjun/cdn@1.6/img/custom/avatar.jpg
    cdn: https://cdn.jsdelivr.net/gh/honjun/cdn@1.6

# 开启pjax 【选】
    pjax: 1

# 站点首页的公告信息 【改】
    notice: hexo-Sakura主题已经开源，目前正在开发中...

# 懒加载的加载中图片 【选】
    lazyloadImg: https://cdn.jsdelivr.net/gh/honjun/cdn@1.6/img/loader/orange.progress-bar-stripe-loader.svg

菜单配置

    一个正确的格式：

    menus:
    菜单名: { path: url, fa: 图标}
    菜单名: { path: rul, fa: 图标, 
    submenus: { 
        技术: {path: url, fa: 图标 }, 
        生活: {path: url, fa: 图标 }  ##最后一个不能加逗号
    } }
# 图标来自：https://fontawesome.com/icons

# 站点菜单配置 【选】
    menus:
    首页: { path: /, fa: fa-fort-awesome faa-shake }
    归档: { path: /archives, fa: fa-archive faa-shake, submenus: { 
        技术: {path: /categories/技术/, fa: fa-code }, 
        生活: {path: /categories/生活/, fa: fa-file-text-o }, 
        资源: {path: /categories/资源/, fa: fa-cloud-download }, 
        随想: {path: /categories/随想/, fa: fa-commenting-o },
        转载: {path: /categories/转载/, fa: fa-book }
    } }
    清单: { path: javascript:;, fa: fa-list-ul faa-vertical, submenus: { 
        书单: {path: /tags/悦读/, fa: fa-th-list faa-bounce }, 
        番组: {path: /bangumi/, fa: fa-film faa-vertical }, 
        歌单: {path: /music/, fa: fa-headphones },
        图集: {path: /tags/图集/, fa: fa-photo }
    } }
    留言板: { path: /comment/, fa: fa-pencil-square-o faa-tada }
    友人帐: { path: /links/, fa: fa-link faa-shake }
    赞赏: { path: /donate/, fa: fa-heart faa-pulse }
    关于: { path: /, fa: fa-leaf faa-wrench , submenus: { 
        我？: {path: /about/, fa: fa-meetup}, 
        主题: {path: /theme-sakura/, fa: iconfont icon-sakura },
        Lab: {path: /lab/, fa: fa-cogs },
    } }
    客户端: { path: /client/, fa: fa-android faa-vertical }
    RSS: { path: /atom.xml, fa: fa-rss faa-pulse }

**背景配置**

# Home page sort type: -1: newer first，1: older first. 【非】
    homePageSortType: -1

# Home page article shown number) 【非】
    homeArticleShown: 10

# 背景图片 【选】
    bgn: 8

bg:
  - 格式

背景图片样式 空 原图效果 filter-dim 阴影 filter-grid 横条 filter-dot 点点
    bgclass: filter-dot

# startdash面板 url, title, desc img 【改】
    startdash: 
    - {url: /theme-sakura/, title: Sakura, desc: 本站 hexo 主题, img: /img/startdash/sakura.md.png}
    - {url: http://space.bilibili.com/271849279, title: Bilibili, desc: 博主的b站视频, img: /img/startdash/bilibili.jpg}
    - {url: /, title: hojun的万事屋, desc: 技术服务, img: /img/startdash/wangshiwu.jpg}


# your site build time or founded date
# 你的站点建立日期 【改】
    siteBuildingTime: 07/17/2018

**社交配置**

这将会显示在主页上

# 社交按钮(social)  url, img PC端配置 【改】
    social:
    github: {url: http://github.com/honjun, img: /img/social/github.png}
    sina: {url: http://weibo.com/mashirozx?is_all=1, img: /img/social/sina.png}
    wangyiyun: {url: http://weibo.com/mashirozx?is_all=1, img: /img/social/wangyiyun.png}
    zhihu: {url: http://weibo.com/mashirozx?is_all=1, img: /img/social/zhihu.png}
    email: {url: http://weibo.com/mashirozx?is_all=1, img: /img/social/email.svg}
    wechat: {url: /#, qrcode: /img/custom/wechat.jpg, img: /img/social/wechat.png}

# 社交按钮(msocial)  url, img 移动端配置 【改】
    msocial:
    github: {url: http://github.com/honjun, fa: fa-github, color: 333}
    weibo: {url: http://weibo.com/mashirozx?is_all=1, fa: fa-weibo, color: dd4b39}
    qq: {url: https://wpa.qq.com/msgrd?v=3&uin=954655431&site=qq&menu=yes, fa: fa-qq, color: 25c6fe}

**打赏二维码**

# 赞赏二维码（其中wechatSQ是赞赏单页面的赞赏码图片）【改】
    donate:
    alipay: /img/custom/donate/AliPayQR.jpg
    wechat: /img/custom/donate/WeChanQR.jpg
    wechatSQ: /img/custom/donate/WeChanSQ.jpg

首页媒体

# 首页视频地址为https://cdn.jsdelivr.net/gh/honjun/hojun@1.2/Unbroken.mp4，配置如下 【改】
    movies:
    url: https://cdn.jsdelivr.net/gh/honjun/hojun@1.2
# 多个视频用逗号隔开，随机获取。支持的格式目前已知MP4,Flv。其他的可以试下，不保证有用
  name: Unbroken.mp4

# 左下角aplayer播放器配置 主要改id和server这两项，修改详见[aplayer文档] 【改】
    aplayer: 
    id: 2660651585
    server: netease
    type: playlist
    fixed: true
    mini: false
    autoplay: false
    loop: all
    order: random
    preload: auto
    volume: 0.7
    mutex: true

**评论**

    你需要在leancloud注册账号：https://www.leancloud.cn/ 然后获取id和秘钥

# Valine评论配置【改】
    valine: true
    v_appId: GyC3NzMvd0hT9Yyd2hYIC0MN-gzGzoHsz
    v_appKey: mgOpfzbkHYqU92CV4IDlAUHQ

分类页和标签页配置
分类页

分类页 
标签页

image

配置项在\themes\Sakura\languages\zh-cn.yml里。新增一个分类或标签最好加下哦，当然嫌麻烦可以直接使用一张默认图片（可以改主题或者直接把404图片替换下，征求下意见要不要给这个在配置文件中加个开关，可以issue或群里提出来），现在是没设置的话会使用那种倒立小狗404哦。

    #category
# 按分类名创建
    技术:
        #中文标题
        zh: 野生技术协会 
        # 英文标题
        en: Geek – Only for Love
        # 封面图片
        img: https://cdn.jsdelivr.net/gh/honjun/cdn@1.6/img/banner/coding.jpg
    生活:
        zh: 生活
        en: live
        img: https://cdn.jsdelivr.net/gh/honjun/cdn@1.6/img/banner/writing.jpg

    #tag
    # 标签名即是标题
    悦读:
        # 封面图片
        img: https://cdn.jsdelivr.net/gh/honjun/cdn@1.6/img/banner/reading.jpg

单页面封面配置

如留言板页面页面，位于source下的comment下，打开index.md如下：

    ---
    title: comment
    date: 2018-12-20 23:13:48
    keywords: 留言板
    description: 
    comments: true
    # 在这里配置单页面头部图片，自定义替换哦~
    photos: https://cdn.jsdelivr.net/gh/honjun/cdn@1.4/img/banner/comment.jpg
    ---

单页面配置
番组计划页 （请直接在下载后的文件中改，下面的添加了注释可能会有些影响）

image

    ---
    layout: bangumi
    title: bangumi
    comments: false
    date: 2019-02-10 21:32:48
    keywords:
    description:
    bangumis:
    # 番组图片
    - img: https://lain.bgm.tv/pic/cover/l/0e/1e/218971_2y351.jpg
    # 番组名
        title: 朝花夕誓——于离别之朝束起约定之花
    # 追番状态 （追番ing/已追完）
        status: 已追完
    # 追番进度
        progress: 100
    # 番剧日文名称
        jp: さよならの朝に約束の花をかざろう
    # 放送时间
        time: 放送时间: 2018-02-24 SUN.
    # 番剧介绍
        desc:  住在远离尘嚣的土地，一边将每天的事情编织成名为希比欧的布，一边静静生活的伊欧夫人民。在15岁左右外表就停止成长，拥有数百年寿命的他们，被称为“离别的一族”，并被视为活着的传说。没有双亲的伊欧夫少女玛奇亚，过着被伙伴包围的平稳日子，却总感觉“孤身一人”。他们的这种日常，一瞬间就崩溃消失。追求伊欧夫的长寿之血，梅萨蒂军乘坐着名为雷纳特的古代兽发动了进攻。在绝望与混乱之中，伊欧夫的第一美女蕾莉亚被梅萨蒂带走，而玛奇亚暗恋的少年克里姆也失踪了。玛奇亚虽然总算逃脱了，却失去了伙伴和归去之地……。
    - img: https://lain.bgm.tv/pic/cover/l/0e/1e/218971_2y351.jpg
        title: 朝花夕誓——于离别之朝束起约定之花
        status: 已追完
        progress: 50
        jp: さよならの朝に約束の花をかざろう
        time: 放送时间: 2018-02-24 SUN.
        desc: 住在远离尘嚣的土地，一边将每天的事情编织成名为希比欧的布，一边静静生活的伊欧夫人民。在15岁左右外表就停止成长，拥有数百年寿命的他们，被称为“离别的一族”，并被视为活着的传说。没有双亲的伊欧夫少女玛奇亚，过着被伙伴包围的平稳日子，却总感觉“孤身一人”。他们的这种日常，一瞬间就崩溃消失。追求伊欧夫的长寿之血，梅萨蒂军乘坐着名为雷纳特的古代兽发动了进攻。在绝望与混乱之中，伊欧夫的第一美女蕾莉亚被梅萨蒂带走，而玛奇亚暗恋的少年克里姆也失踪了。玛奇亚虽然总算逃脱了，却失去了伙伴和归去之地……。
    ---

友链页

    （请直接在下载后的文件中改，下面的添加了注释可能会有些影响）

    ---
    layout: links
    title: links
    # 创建日期，可以改下
    date: 2018-12-19 23:11:06 
    # 图片上的标题，自定义修改
    keywords: 友人帐 
    description: 
    # true/false 开启/关闭评论
    comments: true 
    # 页面头部图片，自定义修改
    photos: https://cdn.jsdelivr.net/gh/honjun/cdn@1.4/img/banner/links.jpg 
    # 友链配置
    links: 
    # 类型分组
    - group: 个人项目
        # 类型简介
        desc: 充分说明这家伙是条咸鱼 < (￣︶￣)>
        items:
        # 友链链接
        - url: https://shino.cc/fgvf
        # 友链头像
        img: https://cloud.moezx.cc/Picture/svg/landscape/fields.svg
        # 友链站点名
        name: Google
        # 友链介绍  下面雷同
        desc: Google 镜像
        - url: https://shino.cc/fgvf
        img: https://cloud.moezx.cc/Picture/svg/landscape/fields.svg
        name: Google
        desc: Google 镜像
    # 类型分组...
    - group: 小伙伴们
        desc: 欢迎交换友链 ꉂ(ˊᗜˋ)
        items:
        - url: https://shino.cc/fgvf
        img: https://cloud.moezx.cc/Picture/svg/landscape/fields.svg
        name: Google
        desc: Google 镜像
        - url: https://shino.cc/fgvf
        img: https://cloud.moezx.cc/Picture/svg/landscape/fields.svg
        name: Google
        desc: Google 镜像
    ---

写文章配置

主题集成了个人插件hexo-tag-bili和hexo-tag-fancybox_img。其中hexo-tag-bili用来在文章或单页面中插入B站外链视频，使用语法如下：

    {% bili video_id [page] %}

详细使用教程详见hexo-tag-bili。

hexo-tag-fancybox_img用来在文章或单页面中图片，使用语法如下：

    {% fb_img src [caption] %}

详细使用教程详见hexo-tag-fancybox_img
还有啥，一时想不起来......

To be continued...
2019.6.1追加

一直没时间更新readme，六一更新如下
zoom放大图片

关于zoom点击放大图片功能，一直就有，不过readme里头没说明。 修改Sakura\node_modules\marked\lib\marked.js的Renderer.prototype.image方法为

    Renderer.prototype.image = function(href, title, text) {
    if (this.options.baseUrl && !originIndependentUrl.test(href)) {
        href = resolveUrl(this.options.baseUrl, href);
    }
    var out = '<img data-action="zoom" src="' + href + '" alt="' + text + '"';
    if (title) {
        out += ' title="' + title + '"';
    }
    out += this.options.xhtml ? '/>' : '>';
    return out;
    };

即可

**关闭公告**

配置公告为空或false，表示关闭公告 notice: false
动态配置aplayer

    aplayer: 
    id: 2660651585
    server: netease
    type: playlist
    fixed: true
    autoplay: false
    loop: all
    order: random
    preload: auto
    volume: 0.7
    mutex: true

aplayer配置可以自己自定义参数，且都会渲染出来，不局限于以上内容。参考aplayer文档添加参数或拿来实现自己一些特殊功能