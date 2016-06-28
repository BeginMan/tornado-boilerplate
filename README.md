tornado-boilerplate -- Tornado应用的一个标准的布局
===============================================================================

## 描述

tornado-boilerplate 建立一个[Tornado](http://www.tornadoweb.org/) 应用布局, 协助编写实用程序来部署这样的应用程序。

这个App布局是[buedafab](https://github.com/bueda/ops)的一个假定。

Tornado v3.2 测试

### 相关项目

[buedafab](https://github.com/bueda/ops)
[django-boilerplate](https://github.com/bueda/django-boilerplate)
[python-webapp-etc](https://github.com/bueda/python-webapp-etc)
[comrade](https://github.com/bueda/django-comrade)

## 目录结构

    tornado-boilerplate/
        handlers/
            foo.py
            base.py
        lib/
        logconfig/
        media/
            css/
                vendor/
            js/
                vendor/
            images/
        requirements/
            common.txt
            dev.txt
            production.txt
        templates/
        vendor/
        environment.py
        fabfile.py
        app.py
        settings.py

### handlers目录

你的所有的 Tornado RequestHandlers 都放在这个目录下,
当`environment.py`被导入时, 该目录下文件被加到环境变量`PYTHONPATH`中。

### lib目录

Python包和模块,并不是真的Tornado请求处理程序。这些只是普通的Python类和方法。
同上也被加载到`PYTHONPATH`环境变量中。

### logconfig目录

来自Mozilla's [zamboni](https://github.com/jbalogh/zamboni)的一个扩展版的[log_settings](https://github.com/jbalogh/zamboni/blob/master/log_settings.py)
模块。

包含一个`initialize_logging` 方法 意味着从项目的`settings.py`中被调用来设置python日志系统.


All of your loggers should be children of your app's root logger (defined in
`settings.py`). This works well at the top of every file that needs logging:

    import logging
    logger = logging.getLogger('five.' + __name__)

### media目录

每个 CSS, Javascript 和 images都对应一个子目录. 第三方文件放在 `vendor/`子文件夹来保持自己的代码分开的。

### requirements

pip requirements 文件, 为每个app环境选择一个。`common.txt` 通用的requirements文件,在每个环境下都会需要。


### templates目录

项目的templates (i.e. those not belonging to any specific app in the
`handlers/` folder). The boilerplate includes a `base.html` template that defines
these blocks:

#### <head>

`title` - Text for the browser title bar. You can set a default here and
append/prepend to it in sub-templates using `{{ super }}`.

`site_css` - Primary CSS files for the site. By default, includes
`media/css/reset.css` and `media/css/base.css`.

`css` - Optional page-specific CSS - empty by default. Use this block if a page
needs an extra CSS file or two, but doesn't want to wipe out the files already
linked via the `site_css` block.

`extra_head` - Any extra content for between the `<head>` tags.

#### <body>

`header` -Top of the body, inside a `div` with the ID `header`.

`content` - After the `header`, inside a `div` with the ID `content`.

`footer` - After `content`, inside a `div` with the ID `footer`.

`site_js` - After all body content, includes site-wide Javascript files. By
default, includes `media/js/application.js` and jQuery. In deployed
environments, links to a copy of jQuery on Google's CDN. If running in solo
development mode, links to a local copy of jQuery from the `media/` directory -
because the best way to fight snakes on a plane is with jQuery on a plane.

`js` - Just like the `css` block, use the `js` block for page-specific
Javascript files when you don't want to wipe out the site-wide defaults in
`site_js`.

#### TODO

This needs to be tested with Tornado's templating language. A quick
look at the documentation indicates that this basic template is compatible, but
none of our Tornado applications are using templates at the moment, so it hasn't
been tested.

### vendor

Python package dependencies loaded as git submodules. pip's support for git
repositories is somewhat unreliable, and if the specific package is your own
code it can be a bit easier to debug if it's all in one place (and not off in a
virtualenv).

At Bueda we collect general webapp helpers and views in the separate package
`comrade` and share it among all of our applications. It is included here as an
example of a Python package as a git submodule (comrade itself should't be
considered part of this boilerplate - while it might be useful, it's much less
generic).

Any directory in `vendor/` is added to the `PYTHONPATH` by `environment.py`. The
packages are *not* installed with pip, however, so if they require any
compilation (e.g. C/C++ extensions) this method will not work.

### Files

#### environment.py

修改`PYTHONPATH` 允许导入`apps/`, `lib/` 和`vendor/` 目录。

这个模块在`settings.py`的顶部被导入,为了确保能够在本地测试环境和生成环境下运行。

#### fabfile.py

使用[Fabric](http://fabfile.org/) 自动化部署。

#### app.py

Tornado的主程序,也是启动程序。

#### settings.py

像Django一样设置一个地方来收集应用程序配置。


## 贡献

如果您有改进或bug修复:

* Fork the repository on GitHub
* File an issue for the bug fix/feature request in GitHub
* Create a topic branch
* Push your modifications to that branch
* Send a pull request

## 作者

* [Bueda Inc.](http://www.bueda.com)
* Christopher Peplin, peplin@bueda.com, @[peplin](http://twitter.com/peplin)
* Aman Guatam