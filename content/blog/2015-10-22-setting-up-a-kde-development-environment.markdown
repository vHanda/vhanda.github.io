---
date: "2015-10-22T00:13:11Z"
tags:
- planetkde
title: Setting up a KDE Development Environment
---

[Season of KDE](https://dot.kde.org/2015/10/08/season-kde-2015-now-open) is about to start, and I've been getting a number of emails with students struggling to setup a development environment. For KDE, it isn't quite trivial.

While there are multiple ways to setup a development environment, I'm going to go through how mine functions. I know many developers who have a similar setup.

### Step 1 - Choose your distribution

I develop exclusively on Linux, so do most KDE developers. While some parts of KDE do function on OSX and Windows, that's not what most of us use. So pick your favorite Linux distribution.

In general, I find distributions which follow a rolling release to be easier to work with. My favorite is [Arch Linux](https://www.archlinux.org/). If you're new to Linux, it can be a little daunting, but I do recommend it for development.

### Step 2 - Install basic tools

In order to work on most KDE projects you need the following tools -

* A C++ compiler (Clang is awesome)
* CMake
* Ninja
* Git

Install these using your package manager. Ninja isn't required. But my scripts use it, and I like ninja.

### Step 3 - The basic environment

Before I start with how my environment works, here is a little mumbo jumbo which you can skip if you don't care. Why does one need a build environment? You can just compile the project you want, install it as root and run it. The reason is so that your main system is isolated from your root system. It makes development easier as your environment can have a different version than your distribution, and it's much safer, since it is harder to nuke your system.

I like to use my main user and skip creating a special user for development. For this blog post we shall call him Bob. Bob is cool. On my system he is called Vishesh.

    $ mkdir ~/kde

Everything goes in this `~/kde` directory. This is where our entire environment will reside.

    $ mkdir ~/kde/.setup

The `.setup` folder is where the code for which sets up the environment goes. I split my environment based on environment variables and convenience functions.

Create the first file `~/kde/.setup/env`

    #!/bin/bash

    export KF5=~/kde
    export KDEDIR=~/kde
    export KDE_SESSION_VERSION=5

    export XDG_DATA_DIRS=$KF5/share:/usr/share
    export XDG_CONFIG_DIRS=$KF5/etc/xdg:/etc/xdg

    export PATH=$KF5/bin:$PATH
    export QT_PLUGIN_PATH=$KF5/lib/plugins:$KF5/lib/x86_64-linux-gnu/plugins:$QT_PLUGIN_PATH

    export QML2_IMPORT_PATH=$KF5/lib/qml:$KF5/qml:$KF5/lib/x86_64-linux-gnu/qml:$QTDIR/qml

    export CMAKE_PREFIX_PATH=$KF5
    export PKG_CONFIG_PATH=$KDEDIR/lib/pkgconfig:/usr/lib/pkgconfig
    export LD_LIBRARY_PATH=$KDEDIR/lib:/usr/lib

Create the second file `~/kde/.setup/functions`

    #!/bin/bash
    export KDE_BUILD=$KDEDIR/build
    export KDE_SRC=$KDEDIR/src


    # Make sure build directory exists.
    mkdir -p "$KDE_BUILD"
    mkdir -p "$KDE_SRC"

    function ck {
            local srcFolder current

            srcFolder=`pwd | sed -e s,$KDE_BUILD,$KDE_SRC,`
            current=`pwd`
            if [ "$srcFolder" = "$current" ]; then
                    cb
            fi

            cmake "$srcFolder" -DCMAKE_INSTALL_PREFIX=$KDEDIR \
                -G Ninja \
                -DPYTHON_SITE_PACKAGES_DIR:PATH=~/.local/lib/python2.6/site-packages \
                -DCMAKE_BUILD_TYPE="Debug" \
                -DPYTHON_EXECUTABLE:FILEPATH="/usr/bin/python2" \
                -DCMAKE_INSTALL_PREFIX=$KDEDIR \
                -DCMAKE_C_COMPILER=/usr/bin/clang -DCMAKE_CXX_COMPILER=/usr/bin/clang++ \
                -DCMAKE_EXPORT_COMPILE_COMMANDS=ON \
                -DCMAKE_CXX_FLAGS="-Weverything -Wno-c++98-compat -Wno-c++98-compat-pedantic -Wno-exit-time-destructors -Wno-padded -Wno-sign-conversion -Wno-documentation -Wno-undefined-reinterpret-cast -fcolor-diagnostics -Wno-shorten-64-to-32 -Wno-conversion -Wno-gnu-zero-variadic-macro-arguments -Wno-shadow"
                # C++ 98 - It was about 17 years ago
                # no-undefined-reinterpret-cast -> This is because moc files typically use reinterpret casts
                # Padded - Nothing much that can be done. Also, it's annoying
                # Documentation - Maybe we should care about this one?
                # Sign Conversion - It's annoying, but maybe we should care about this.
                # -fcolor-diagnostics - For ninja coloured output
                # Conversion + shorten -> Cause they are annoying and I just ignore them. Bad Vishesh!
                # gnu-zero-variadic-macro -> When using qCDebug()
                # -Wno-shadow -> Just too many warnings

            ninja && ninja install
            RETURN=$?
            cs
            return ${RETURN}
    }

    function ckgcc {
            local srcFolder current

            srcFolder=`pwd | sed -e s,$KDE_BUILD,$KDE_SRC,`
            current=`pwd`
            if [ "$srcFolder" = "$current" ]; then
                    cb
            fi

            cmake "$srcFolder" -DCMAKE_INSTALL_PREFIX=$KDEDIR \
                -G Ninja \
                -DPYTHON_SITE_PACKAGES_DIR:PATH=~/.local/lib/python2.6/site-packages \
                -DCMAKE_BUILD_TYPE="Debug" \
                -DPYTHON_EXECUTABLE:FILEPATH="/usr/bin/python2" \
                -DCMAKE_INSTALL_PREFIX=$KDEDIR \

            # nice make -j4 && make install
            ninja && ninja install
            RETURN=$?
            cs
            return ${RETURN}
    }

    function cb {
            local dest
            dest=`pwd | sed -e s,$KDE_SRC,$KDE_BUILD,`
            mkdir -p "$dest"
            cd "$dest"
    }

    function cs {
            cd `pwd | sed -e s,$KDE_BUILD,$KDE_SRC,`
    }

### Step 4 - Get the source

All my source code goes in `~/kde/src/`. Look for the project you want to contribute to either on [projects.kde.org](https://projects.kde.org/). The code is also on [github](https://github.com/KDE/). This is a read only mirror so the typical github workflow will not work. I will still look at your github pull request, but other KDE developers might not.

Lets take the example of Baloo.

    $ mkdir -p ~/kde/src
    $ cd ~/kde/src
    $ git clone git://anongit.kde.org/baloo

### Step 5 - Activate the Environment

The environment can be activated by sourcing those two special files we created.

    $ source ~/kde/.setup/env
    $ source ~/kde/.setup/functions

This needs to be done each time you open a new terminal session. Later we'll see how we can make this even simpler.

### Step 6 - Project Dependencies

Every project has its own list of dependencies. This part is not easy to automate and requires you to install all of them. You can either compile some of them yourselves, or install them through your package manager. As a starting point I would recommend just using your package manager. Do not compile Qt.

Most distros split up packages into the normal package and `-devel`. You need to install the `-devel` version of the dependency. Arch Linux is awesome, and does not split them.

### Step 7 - Lets build it!

    $ cd ~/kde/src/baloo
    $ ck

That's it. This will run 3 steps - `cmake`, `ninja` and `ninja install`. If you insist on using gcc instead of clang then run `ckgcc` instead. Let me explain each of these steps.

`cmake` is a build system which we typically use. This means we use it to figure out what all needs to be compiled, what flags should be used, where the dependencies are, etc. With our scripts its output goes in a special build directory `~/kde/build/baloo`.

`ninja` (or make): This command typically looks for a Ninja file (or MakeFile). It is the actual step which compiles your entire application. It can be run by going into the build directory and running `ninja`. The `ck` command automatically does this for you. If you get a compile error, you should run this command instead of running `ck` and starting from scratch.

`ninja install`: This command installs all the files in the correct location. For us the location is `~/kde`. We do not want to mess up our root environment. This command is also run in the build directory.

### Step 8 - Running the project

If this all worked out, the project should be compiled and installed. You can run the project manually by calling the correct executable. This executable is in your `~/kde/bin` directory. This directory is in your `PATH` environment variable, so just running the executable will find it.

### Step 9 - Jumping around

The `functions` file has a few convenience methods. `cb` and `cs`. These stand for 'change to build` and 'change to source'. They can be run at any point of jump to the build and source directory respectively. It's quite handy since certain commands need for you to be in certain directories.


That's about it. I could write more on how to activate the environment more easily and how to make sure your IDE uses this environment as well, but I'm tired, and sleepy. It is 2 AM. Good Night.

Here is a more detailed link - https://techbase.kde.org/Getting_Started/Build/Environment

