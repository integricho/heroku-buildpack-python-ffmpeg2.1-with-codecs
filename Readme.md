Heroku buildpack: Python-ffmpeg 2.1 with codecs
========================

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) for Python apps. The only difference from the clean Python buildpack is that this one includes a static build of ffmpeg 2.1 with codecs. The static build is simply cloned from [this repo](https://github.com/integricho/ffmpeg2.1-static-with-codecs).

It uses [virtualenv](http://www.virtualenv.org/) and [pip](http://www.pip-installer.org/).

Usage
-----

Example usage:

    $ ls
    gunicorn.py.ini  Procfile  requirements.txt  wsgi.py
    $ git init
    Initialized empty Git repository in /path/to/app/.git/
    $ heroku create --stack cedar --buildpack git://github.com/integricho/heroku-buildpack-python-ffmpeg2.1-with-codecs.git
    Creating random-app-name-1234... done, stack is cedar
    BUILDPACK_URL=git://github.com/integricho/heroku-buildpack-python-ffmpeg2.1-with-codecs.git
    http://random-app-name-1234.herokuapp.com/ | git@heroku.com:random-app-name-1234.git
    Git remote heroku added
    $ git add .
    $ git commit -m "Initial commit."
    [master (root-commit) 16d444a] Initial commit.
     4 files changed, 29 insertions(+)
     create mode 100644 Procfile
     create mode 100644 gunicorn.py.ini
     create mode 100644 requirements.txt
     create mode 100644 wsgi.py
    $ git push heroku master
    Counting objects: 6, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (5/5), done.
    Writing objects: 100% (6/6), 775 bytes, done.
    Total 6 (delta 0), reused 0 (delta 0)

    -----> Fetching custom git buildpack... done
    -----> Python app detected
    -----> Cloning pre-built ffmpeg repository...
    -----> No runtime.txt provided; assuming python-2.7.3.
    -----> Preparing Python runtime (python-2.7.3)
    -----> Installing Distribute (0.6.34)
    -----> Installing Pip (1.2.1)
    -----> Installing dependencies using Pip (1.2.1)
           Downloading/unpacking gevent==0.13.8 (from -r requirements.txt (line 1))
             Running setup.py egg_info for package gevent

           Downloading/unpacking gunicorn==18.0 (from -r requirements.txt (line 2))
             Running setup.py egg_info for package gunicorn

           Downloading/unpacking greenlet (from gevent==0.13.8->-r requirements.txt (line 1))
             Running setup.py egg_info for package greenlet

           Installing collected packages: gevent, gunicorn, greenlet
             Running setup.py install for gevent
               building 'gevent.core' extension
               gcc -pthread -fno-strict-aliasing -g -O2 -DNDEBUG -g -fwrapv -O3 -Wall -Wstrict-prototypes -fPIC -I/app/.heroku/python/include/python2.7 -c gevent/core.c -o build/temp.linux-x86_64-2.7/gevent/core.o
               gcc -pthread -shared build/temp.linux-x86_64-2.7/gevent/core.o -levent -o build/lib.linux-x86_64-2.7/gevent/core.so
               Linking /app/build/gevent/build/lib.linux-x86_64-2.7/gevent/core.so to /app/build/gevent/gevent/core.so

             Running setup.py install for gunicorn

               Installing gunicorn_paster script to /app/.heroku/python/bin
               Installing gunicorn script to /app/.heroku/python/bin
               Installing gunicorn_django script to /app/.heroku/python/bin
             Running setup.py install for greenlet
               gcc -pthread -fno-strict-aliasing -g -O2 -DNDEBUG -g -fwrapv -O3 -Wall -Wstrict-prototypes -fPIC -fno-tree-dominator-opts -I/app/.heroku/python/include/python2.7 -c /tmp/tmp_LRuFq/simple.c -o /tmp/tmp_LRuFq/tmp/tmp_LRuFq/simple.o
               /tmp/tmp_LRuFq/simple.c:1: warning: function declaration isn’t a prototype
               building 'greenlet' extension
               gcc -pthread -fno-strict-aliasing -g -O2 -DNDEBUG -g -fwrapv -O3 -Wall -Wstrict-prototypes -fPIC -fno-tree-dominator-opts -I/app/.heroku/python/include/python2.7 -c greenlet.c -o build/temp.linux-x86_64-2.7/greenlet.o
               In file included from slp_platformselect.h:12,
                                from greenlet.c:416:
               platform/switch_amd64_unix.h:40: warning: ‘noclone’ attribute directive ignored
               platform/switch_amd64_unix.h:43: warning: ‘noclone’ attribute directive ignored
               greenlet.c: In function ‘g_switch’:
               greenlet.c:543: warning: ‘err’ may be used uninitialized in this function
               gcc -pthread -shared build/temp.linux-x86_64-2.7/greenlet.o -o build/lib.linux-x86_64-2.7/greenlet.so
               Linking /app/build/greenlet/build/lib.linux-x86_64-2.7/greenlet.so to /app/build/greenlet/greenlet.so

           Successfully installed gevent gunicorn greenlet
           Cleaning up...

    -----> Discovering process types
           Procfile declares types -> web

    -----> Compiled slug size: 74.3MB
    -----> Launching... done, v5
           http://random-app-name-1234.herokuapp.com deployed to Heroku

    To git@heroku.com:random-app-name-1234.git
     * [new branch]      master -> master
    $ heroku run bash
    Running `bash` attached to terminal... up, run.1627
    ~ $ ffmpeg
    ffmpeg version 2.1-static Copyright (c) 2000-2013 the FFmpeg developers
      built on Nov 18 2013 09:38:34 with gcc 4.6 (Ubuntu/Linaro 4.6.3-1ubuntu5)
      configuration: --prefix=/home/user/dev/tools/ffmpeg-static/target --extra-cflags='-I/home/user/dev/tools/ffmpeg-static/target/include -static' --extra-ldflags='-L/home/user/dev/tools/ffmpeg-static/target/lib -lm -static' --extra-version=static --disable-debug --disable-shared --enable-static --extra-cflags=--static --disable-ffplay --disable-ffserver --disable-doc --enable-gpl --enable-pthreads --enable-postproc --enable-gray --enable-runtime-cpudetect --enable-libfaac --enable-libmp3lame --enable-libtheora --enable-libvorbis --enable-libx264 --enable-libxvid --enable-bzlib --enable-zlib --enable-nonfree --enable-version3 --enable-libvpx --disable-devices
      libavutil      52. 48.100 / 52. 48.100
      libavcodec     55. 39.100 / 55. 39.100
      libavformat    55. 19.104 / 55. 19.104
      libavdevice    55.  5.100 / 55.  5.100
      libavfilter     3. 90.100 /  3. 90.100
      libswscale      2.  5.101 /  2.  5.101
      libswresample   0. 17.104 /  0. 17.104
      libpostproc    52.  3.100 / 52.  3.100
    Hyper fast Audio and Video encoder
    usage: ffmpeg [options] [[infile options] -i infile]... {[outfile options] outfile}...

    Use -h to get full help or, even better, run 'man ffmpeg'
    ~ $ exit

You can also add it to upcoming builds of an existing application:

    $ heroku config:add BUILDPACK_URL=git://github.com/integricho/heroku-buildpack-python-ffmpeg2.1-with-codecs.git

The buildpack will detect your app as Python if it has the file `requirements.txt` in the root. It will detect your app as Python/Django if there is an additional `settings.py` in a project subdirectory.

It will use virtualenv and pip to install your dependencies, vendoring a copy of the Python runtime into your slug.  The `bin/`, `include/` and `lib/` directories will be cached between builds to allow for faster pip install time.

Hacking
-------

To use this buildpack, fork it on Github.  Push up changes to your fork, then create a test app with `--buildpack <your-github-url>` and push to it.

To change the vendored virtualenv, unpack the desired version to the `src/` folder, and update the virtualenv() function in `bin/compile` to prepend the virtualenv module directory to the path. The virtualenv release vendors its own versions of pip and setuptools.
