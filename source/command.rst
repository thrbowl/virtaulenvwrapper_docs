命令列表
========


虚拟环境管理
------------

mkvirtualenv
^^^^^^^^^^^^^

    在 ``WORKON_HOME`` 目录下，创建一个新的虚环境

    语法::

        mkvirtualenv [-a project_path] [-i package] [-r requirements_file] [virtualenv options] ENVNAME

    除了 ``-a`` , ``-i`` , ``-r`` 和 ``-h`` , 所有的其它选项都直接传递给 ``virtualenv`` , 同时虚环境会在成功初始化后自动激活。

    .. code-block:: bash

        $ workon
        $ mkvirtualenv mynewenv
        New python executable in mynewenv/bin/python
        Installing setuptools.............................................
        ..................................................................
        ..................................................................
        done.
        (mynewenv)$ workon
        mynewenv
        (mynewenv)$

    ``-a`` : 把新创建的虚环境关联到一个已存在的项目路径。

    ``-i`` : 在命令行中指定虚环境需要安装的包,安装多个包使用 ``-ipackage1 -ipackage2`` 方式。

    ``-r`` : 指定一个包含所有需要安装包的列表文件, 传递给 ``pip install -r``。


mktmpenv
^^^^^^^^^

    在 ``WORKON_HOME`` 目录下，创建一个新的虚环境

    语法::

        mktmpenv [(-c|--cd)|(-n|--no-cd)] [VIRTUALENV_OPTIONS]

    创建一个拥有唯一名的虚环境

    如果使用 ``-c`` / ``--cd``，会在 ``post-activate`` 阶段切换工作目录到虚环境目录，忽略 ``VIRTUALENVWRAPPER_WORKON_CD`` 。

    如果使用 ``-n`` / ``--no-cd`` ，不会在 ``post-activate`` 阶段切换工作目录到虚环境目录，忽略 ``VIRTUALENVWRAPPER_WORKON_CD`` 。

    .. code-block:: bash

        $ mktmpenv
        Using real prefix '/Library/Frameworks/Python.framework/Versions/2.7'
        New python executable in 1e513ac6-616e-4d56-9aa5-9d0a3b305e20/bin/python
        Overwriting 1e513ac6-616e-4d56-9aa5-9d0a3b305e20/lib/python2.7/distutils/__init__.py
        with new content
        Installing setuptools...............................................
        ....................................................................
        .................................................................done.
        This is a temporary environment. It will be deleted when deactivated.
        (1e513ac6-616e-4d56-9aa5-9d0a3b305e20) $


lsvirtualenv
^^^^^^^^^^^^^

    列出所有的虚环境

    语法::

        lsvirtualenv [-b] [-l] [-h]

    ==== ================================
    -b   只输出简单信息
    -l   输出完整的信息(默认)
    -h   打印 ``lsvirtualenv`` 的使用帮助
    ==== ================================


showvirtualenv
^^^^^^^^^^^^^^^

    显示指定虚环境的详细信息

    语法::

        showvirtualenv [env]


rmvirtualenv
^^^^^^^^^^^^^

    删除指定的虚环境（在 ``WORKON_HOME`` 目录下）

    语法::

        rmvirtualenv ENVNAME

    .. warning::

        如果要删除当前虚环境，必须先 ``deactivate`` 退出当前环境。

    .. code-block:: bash

        (mynewenv)$ deactivate
        $ rmvirtualenv mynewenv
        $ workon
        $


cpvirtualenv
^^^^^^^^^^^^^

    复制一个存在的（通过 ``virtualenvwrapper`` 管理或者外部直接创建）虚环境。

    .. warning::

        直接拷贝对于虚环境支持的不是很好。在虚环境中会有路径信息硬编码到文件里，直接拷贝不会修改那些文件。

    语法::

        cpvirtualenv ENVNAME [TARGETENVNAME]

    .. note::

        复制虚环境, ``ENVNAME`` 参数是必须的。如果复制的是外部创建的虚环境，
        没有指定 ``TARGETENVNAME`` 参数，将直接使用原来的名称。

    .. code-block:: bash

        $ workon
        $ mkvirtualenv source
        New python executable in source/bin/python
        Installing setuptools.............................................
        ..................................................................
        ..................................................................
        done.
        (source)$ cpvirtualenv source dest
        Making script /Users/dhellmann/Devel/virtualenvwrapper/tmp/dest/bin/easy_install relative
        Making script /Users/dhellmann/Devel/virtualenvwrapper/tmp/dest/bin/easy_install-2.6 relative
        Making script /Users/dhellmann/Devel/virtualenvwrapper/tmp/dest/bin/pip relative
        Script /Users/dhellmann/Devel/virtualenvwrapper/tmp/dest/bin/postactivate cannot be made relative (it's not a normal script that starts with #!/Users/dhellmann/Devel/virtualenvwrapper/tmp/dest/bin/python)
        Script /Users/dhellmann/Devel/virtualenvwrapper/tmp/dest/bin/postdeactivate cannot be made relative (it's not a normal script that starts with #!/Users/dhellmann/Devel/virtualenvwrapper/tmp/dest/bin/python)
        Script /Users/dhellmann/Devel/virtualenvwrapper/tmp/dest/bin/preactivate cannot be made relative (it's not a normal script that starts with #!/Users/dhellmann/Devel/virtualenvwrapper/tmp/dest/bin/python)
        Script /Users/dhellmann/Devel/virtualenvwrapper/tmp/dest/bin/predeactivate cannot be made relative (it's not a normal script that starts with #!/Users/dhellmann/Devel/virtualenvwrapper/tmp/dest/bin/python)
        (dest)$ workon
        dest
        source
        (dest)$


allvirtualenv
^^^^^^^^^^^^^^

    让 ``WORKON_HOME`` 目录下的所有虚环境运行一个命令

    语法::

        allvirtualenv command with arguments

    *绕过钩子*，每个虚环境会被激活，当前的工作目录会切换到虚环境目录，同时运行要执行的命令。
    **命令不能修改当前shell状态，但是可以修改虚环境状态** 。

    .. code-block:: bash

        $ allvirtualenv pip install -U pip


控制虚环境
----------

workon
^^^^^^

    列出或者切换虚环境

    语法::

        workon [(-c|--cd)|(-n|--no-cd)] [environment_name|"."]

    如果没有提供名称，将会列出所有的可用虚环境。

    如果指定了 ``-c`` / ``--cd`` ，工作目录会在 ``post-activate`` 阶段中切换到项目目录，忽略 ``VIRTUALENVWRAPPER_WORKON_CD`` 。

    如果指定了 ``-c`` / ``--cd``，工作目录不会在 ``post-activate`` 阶段切换到项目目录中，忽略 ``VIRTUALENVWRAPPER_WORKON_CD`` 。

    如果"."被作为名称传入，将会根据当前的工作目录查找对应的虚环境。

    .. code-block:: bash

        $ workon
        $ mkvirtualenv env1
          New python executable in env1/bin/python
        Installing setuptools.............................................
        ..................................................................
        ..................................................................
        done.
        (env1)$ mkvirtualenv env2
        New python executable in env2/bin/python
        Installing setuptools.............................................
        ..................................................................
        ..................................................................
        done.
        (env2)$ workon
        env1
        env2
        (env2)$ workon env1
        (env1)$ echo $VIRTUAL_ENV
        /Users/dhellmann/Devel/virtualenvwrapper/tmp/env1
        (env1)$ workon env2
        (env2)$ echo $VIRTUAL_ENV
        /Users/dhellmann/Devel/virtualenvwrapper/tmp/env2
        (env2)$


deactivate
^^^^^^^^^^^

    退出当前虚环境

    语法::

        deactivate

    .. note::

        这个命令是 ``virtualenv`` 的一个命令，但是像 ``activate`` 一样， 被封装提供了之前或之后的钩子来处理额外的操作。

    .. code-block:: bash

        $ workon
        $ echo $VIRTUAL_ENV

        $ mkvirtualenv env1
        New python executable in env1/bin/python
        Installing setuptools.............................................
        ..................................................................
        ..................................................................
        done.
        (env1)$ echo $VIRTUAL_ENV
        /Users/dhellmann/Devel/virtualenvwrapper/tmp/env1
        (env1)$ deactivate
        $ echo $VIRTUAL_ENV

        $


快速定位到虚环境
----------------


cdvirtualenv
^^^^^^^^^^^^^

    改变当前的工作目录到虚环境目录( ``VIRTUAL_ENV`` )

    语法::

        cdvirtualenv [subdir]

    调用 ``cdvirtualenv`` 改变当前的工作目录到虚环境目录，也可以指定到虚环境目录中的子目录。

    .. code-block:: bash

        $ mkvirtualenv env1
        New python executable in env1/bin/python
        Installing setuptools.............................................
        ..................................................................
        ..................................................................
        done.
        (env1)$ echo $VIRTUAL_ENV
        /Users/dhellmann/Devel/virtualenvwrapper/tmp/env1
        (env1)$ cdvirtualenv
        (env1)$ pwd
        /Users/dhellmann/Devel/virtualenvwrapper/tmp/env1
        (env1)$ cdvirtualenv bin
        (env1)$ pwd
        /Users/dhellmann/Devel/virtualenvwrapper/tmp/env1/bin


cdsitepackages
^^^^^^^^^^^^^^^

    改变当前的工作目录到虚环境目录( ``VIRTUAL_ENV`` )中的 ``site-packages`` 目录。

    语法::

        cdsitepackages [subdir]

    因为 ``site-packages`` 的精确路径依赖于Python版本，
    ``cdsitepackages`` 提供了一个快捷切换 ``cdvirtualenv lib/python${pyvers}/site-packages`` 的方式。
    也可以指定一个 ``site-packages`` 中的子目录作为参数。

    .. code-block:: bash

        $ mkvirtualenv env1
        New python executable in env1/bin/python
        Installing setuptools.............................................
        ..................................................................
        ..................................................................
        done.
        (env1)$ echo $VIRTUAL_ENV
        /Users/dhellmann/Devel/virtualenvwrapper/tmp/env1
        (env1)$ cdsitepackages PyMOTW/bisect/
        (env1)$ pwd
        /Users/dhellmann/Devel/virtualenvwrapper/tmp/env1/lib/python2.6/site-packages/PyMOTW/bisect


lssitepackages
^^^^^^^^^^^^^^^

    列出当前虚环境 ``site-packages`` 目录中的内容

    语法::

        lssitepackages

    .. code-block:: bash

        $ mkvirtualenv env1
        New python executable in env1/bin/python
        Installing setuptools.............................................
        ..................................................................
        ..................................................................
        done.
        (env1)$ $ workon env1
        (env1)$ lssitepackages
        setuptools-0.6.10-py2.6.egg     pip-0.6.3-py2.6.egg
        easy-install.pth                setuptools.pth


路径管理
--------


add2virtualenv
^^^^^^^^^^^^^^^

    把目录添加到当前虚环境的Python PATH中

    语法::

        add2virtualenv directory1 directory2 ...

    有时需要共享不在系统 ``site-packages`` 目录中的第三方库，可能需要在每个虚环境中安装同样的库。
    一个解决办法是做软链接到虚环境的 ``site-packages`` 目录，但是另一种推荐的方法是通过.pth文件，
    添加额外的目录到 ``PYTHONPATH`` 中。

    1. 从项目中抽取源码，例如Django
    #. 运行 ``add2virtualenv path_to_source``
    #. 运行 ``add2virtualenv.``
    #. 打印出使用信息和外部路径

    目录列表被添加到 ``site-packages`` 中的 ``_virtualenv_path_extensions.pth`` 文件。


toggleglobalsitepackages
^^^^^^^^^^^^^^^^^^^^^^^^^^

    控制当前虚环境是否可以访问全局Python ``site-packages`` 目录中的第三方库

    语法::

        toggleglobalsitepackages [-q]

    默认输出虚环境的新状态，使用 ``-q`` 关闭输出.


项目路径管理
-------------


mkproject
^^^^^^^^^^

    在 ``WORKON_HOME`` 中创建虚环境，在 ``PROJECT_HOME`` 中创建项目目录

    语法::

        mkproject [-f|--force] [-t template] [virtualenv_options] ENVNAME

    ``-f`` ``--force`` 即使项目目录存在，也创建虚环境

    在创建新项目时，``template`` 选项可以重复多个。多个模板按照命令行中的顺序被应用。所有其他的可选项都会被传递给 ``mkvirtualenv``。

    .. code-block:: bash

        $ mkproject myproj
        New python executable in myproj/bin/python
        Installing setuptools.............................................
        ..................................................................
        ..................................................................
        done.
        Creating /Users/dhellmann/Devel/myproj
        (myproj)$ pwd
        /Users/dhellmann/Devel/myproj
        (myproj)$ echo $VIRTUAL_ENV
        /Users/dhellmann/Envs/myproj
        (myproj)$


setvirtualenvproject
^^^^^^^^^^^^^^^^^^^^^^

    绑定一个已有的虚环境到一个已有的项目。

    语法::

        setvirtualenvproject [virtualenv_path project_path]

    ``setvirtualenvproject`` 的参数是虚环境和项目的完整路径。如果关联成功，对应的虚环境也会被激活。

    .. code-block:: bash

        $ mkproject myproj
        New python executable in myproj/bin/python
        Installing setuptools.............................................
        ..................................................................
        ..................................................................
        done.
        Creating /Users/dhellmann/Devel/myproj
        (myproj)$ mkvirtualenv myproj_new_libs
        New python executable in myproj/bin/python
        Installing setuptools.............................................
        ..................................................................
        ..................................................................
        done.
        Creating /Users/dhellmann/Devel/myproj
        (myproj_new_libs)$ setvirtualenvproject $VIRTUAL_ENV $(pwd)

    如果没有参数，当前的虚环境和工作目录被使用。

    一个项目可以被多个虚环境绑定。这样可以方便的测试不同版本的python和第三方依赖库对项目的影响。


cdproject
^^^^^^^^^^

    改变当前工作目录到当前虚环境绑定的项目目录

    语法::

        cdproject


管理安装包
-----------


wipeenv
^^^^^^^^

    删除当前虚环境中的所有已安装的第三方依赖包

    语法::

        wipeenv


其它命令
--------


virtualenvwrapper
^^^^^^^^^^^^^^^^^^^

    打印命令列表和使用方法

    语法::

        virtualenvwrapper

