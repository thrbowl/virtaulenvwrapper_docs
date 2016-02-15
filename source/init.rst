初始化配置
==========

在profile.d中添加初始化脚本

.. code-block:: bash

    # Loadd virtualenvwrapper in profile.d
    # /etc/profile.d/virtualenvwrapper.sh
    # http://mkelsey.com/2013/04/30/how-i-setup-virtualenv-and-virtualenvwrapper-on-my-mac/
    export WORKON_HOME=$HOME/.virtualenvs
    export PROJECT_HOME=$HOME/Workspace
    # ensure all new environments are isolated from the site-packages directory
    export VIRTUALENVWRAPPER_VIRTUALENV_ARGS='--no-site-packages'
    # use the same directory for virtualenvs as virtualenvwrapper
    export PIP_VIRTUALENV_BASE=$WORKON_HOME
    # makes pip detect an active virtualenv and install to it
    export PIP_RESPECT_VIRTUALENV=true
    if [[ -r /usr/bin/virtualenvwrapper.sh ]]; then
        source /usr/bin/virtualenvwrapper.sh
    else
        echo "WARNING: Can't find virtualenvwrapper.sh"
    fi
