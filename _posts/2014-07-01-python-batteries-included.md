---
layout: post
title: "Python: Batteries Included"
description: "Little nuggets of functionality that come with python out of the box"
category: articles
tags: [python, tools]
image:
  feature:
  credit:
  creditlink:
---
<section id="table-of-contents" class="toc">
  <header>
    <h3>Contents</h3>
  </header>
<div id="drawer" markdown="1">
*  Auto generated table of contents
{:toc}
</div>
</section><!-- /#table-of-contents -->

Attending this year's PyCon made me reflect on the versatility of Python. It is remarkable what can be accomplished by writing only a few lines of code. What I didn't know was how much functionality it comes with out of the box.

## Executable modules

A lesser known feature of the Python interpreter is the capacity to run a module as a script by passing it the `-m` flag. Using this flag a fair amount of modules in the standard libraries expose functionality by having a `if __name__ == '__main__'` line at the end of the file. Unfortunately, as far as I know, there is little documentation on what modules offer functionality this way, resulting in very few people knowing the full extent of the functionality that the python standard library ships with. In this post I would like to highlight some of the modules I have found most useful and/or entertaining. Some of the modules described provide a substantial amount of functionality and could do with a post of their own to explain the full extent of their features.

###Disclaimer
In this post I assume python 3.4, however most of the modules described are available in older versions of python, perhaps under slightly different names (e.g. http.server used to be SimpleHTTPServer).

## Virtual Environments: venv

Starting with python 3.3 Python now ships with pip and its own implementation of virtual environments. Creating a new virtual environment is as easy as

{% highlight bash %}
$ python -m venv /path/to/<my_venv>
$ source /path/to/<my_venv>/bin/activate
{% endhighlight%}

That's it! Now you can go ahead and install any packages without the fear of polluting your global environment. As far as I'm aware this is not yet integrated with virtualenvwrapper, but doing so should be fairly straightforward.

## Site information

If you are ever wondering what your PYTHONPATH looks like, or where are your python packages being installed, simply do

{% highlight bash %}
$ python -m site
sys.path = [
    '/home/leo/code/projects/python',
    '/home/leo/.virtualenvs/python3.4/lib/python34.zip',
    '/home/leo/.virtualenvs/python3.4/lib/python3.4',
    '/home/leo/.virtualenvs/python3.4/lib/python3.4/plat-linux',
    '/home/leo/.virtualenvs/python3.4/lib/python3.4/lib-dynload',
    '/opt/python3.4.0/lib/python3.4',
    '/opt/python3.4.0/lib/python3.4/plat-linux',
    '/home/leo/.virtualenvs/python3.4/lib/python3.4/site-packages',
]
USER_BASE: '/home/leo/.local' (exists)
USER_SITE: '/home/leo/.local/lib/python3.4/site-packages' (exists)
ENABLE_USER_SITE: False
{% endhighlight%}

## HTTP Server

Having a lightweight HTTP server comes in handy when developing static sites. I have also used this to quickly share files with other people in the same network, just give them your IP address, fire the server up and you are good to go:

{% highlight bash %}
$ python -m http.server [port]
{% endhighlight %}

## Pretty printing JSON

If you do web development, you have probably come across JSON files with no whitespace making it very hard to read. There are plenty of tools and browser plugins to make reading these easier. However, if all you want is to pretty print a JSON file from the command, `python -m json.tool` reads a JSON from stdin and pretty prints it to standard out:

{% highlight bash %}
$ cat my_file.json | python -m json.tool
{% endhighlight%}

## Base64 encoding/decoding

This can come in handy when dealing with MIME data, and is as simple as

{% highlight bash %}
$ echo 'Hello World' | python -m base64 -e
SGVsbG8gV29ybGQK
$ echo 'Hello World' | python -m base64 -e | python -m base64 -d
Hello World
{% endhighlight%}

## Debugging with pdb

Running a python script with the `-m pdb` flag will drop you into the pdb prompt, let you set breakpoints, and let you debug your program with an interface very similar to gdb's. You can also install ipdb, which requires ipython and has some extra features. For more information about debugging python programs, check out the resources listed [here](http://stackoverflow.com/questions/4228637/getting-started-with-the-python-debugger-pdb).


## Measuring performance with profile

The python interpreter comes with a profiler bundled in, and can be used via the `-m profile`. Below is a naive (and extremely inefficient) implementation of the fibonacci sequence.

{% highlight python %}
def main():
    print(fib(25))

def fib(n):
    if n < 2:
        return 1
    return fib(n-1) + fib(n-2)

if __name__ == '__main__':
    main()
{% endhighlight %}

When profiled we get the following output

{% highlight bash %}
$ python -m profile fib.py
121393
         242791 function calls (7 primitive calls) in 1.876 seconds

   Ordered by: standard name

   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
        1    0.000    0.000    1.874    1.874 :0(exec)
        1    0.000    0.000    0.000    0.000 :0(print)
        1    0.003    0.003    0.003    0.003 :0(setprofile)
        1    0.000    0.000    1.874    1.874 fib.py:1(<module>)
        1    0.000    0.000    1.874    1.874 fib.py:1(main)
 242785/1    1.873    0.000    1.873    1.873 fib.py:4(fib)
        1    0.000    0.000    1.876    1.876 profile:0(<code object <module> at 0x7f9c1d3d18a0, file "fib.py", line 1>)
        0    0.000             0.000          profile:0(profiler)
{% endhighlight%}

Which could perhaps help us figure out the outrageous number of calls being made to compute fib(25).

## Docs with pydoc

Accessing the built-in python documentation for a specific module is as easy as typing `python -m pydoc my.module`. Also, running `python -m pydoc -b` will start a webserver serving the docs and open a browser window pointing to it. The web version of the docs could use a face lift, but nonetheless this can come in handy, specially when developing without an internet connection, traveling, etc.

## Calendar

This one is simple. Just print a nicely formatted calendar to stdout:

{% highlight bash %}
$ python -m calendar
                                  2014

      January                   February                   March
Mo Tu We Th Fr Sa Su      Mo Tu We Th Fr Sa Su      Mo Tu We Th Fr Sa Su
       1  2  3  4  5                      1  2                      1  2
 6  7  8  9 10 11 12       3  4  5  6  7  8  9       3  4  5  6  7  8  9
13 14 15 16 17 18 19      10 11 12 13 14 15 16      10 11 12 13 14 15 16
20 21 22 23 24 25 26      17 18 19 20 21 22 23      17 18 19 20 21 22 23
27 28 29 30 31            24 25 26 27 28            24 25 26 27 28 29 30
                                                    31

       April                      May                       June
Mo Tu We Th Fr Sa Su      Mo Tu We Th Fr Sa Su      Mo Tu We Th Fr Sa Su
    1  2  3  4  5  6                1  2  3  4                         1
 7  8  9 10 11 12 13       5  6  7  8  9 10 11       2  3  4  5  6  7  8
14 15 16 17 18 19 20      12 13 14 15 16 17 18       9 10 11 12 13 14 15
21 22 23 24 25 26 27      19 20 21 22 23 24 25      16 17 18 19 20 21 22
28 29 30                  26 27 28 29 30 31         23 24 25 26 27 28 29
                                                    30

        July                     August                  September
Mo Tu We Th Fr Sa Su      Mo Tu We Th Fr Sa Su      Mo Tu We Th Fr Sa Su
    1  2  3  4  5  6                   1  2  3       1  2  3  4  5  6  7
 7  8  9 10 11 12 13       4  5  6  7  8  9 10       8  9 10 11 12 13 14
14 15 16 17 18 19 20      11 12 13 14 15 16 17      15 16 17 18 19 20 21
21 22 23 24 25 26 27      18 19 20 21 22 23 24      22 23 24 25 26 27 28
28 29 30 31               25 26 27 28 29 30 31      29 30

      October                   November                  December
Mo Tu We Th Fr Sa Su      Mo Tu We Th Fr Sa Su      Mo Tu We Th Fr Sa Su
       1  2  3  4  5                      1  2       1  2  3  4  5  6  7
 6  7  8  9 10 11 12       3  4  5  6  7  8  9       8  9 10 11 12 13 14
13 14 15 16 17 18 19      10 11 12 13 14 15 16      15 16 17 18 19 20 21
20 21 22 23 24 25 26      17 18 19 20 21 22 23      22 23 24 25 26 27 28
27 28 29 30 31            24 25 26 27 28 29 30      29 30 31
{% endhighlight %}

## Turtledemo

Last but not least, the turtle module comes with a bundled GUI demo, and examples showcasing what turtle can do. It can be accessed via `python -m turtledemo`, below is a screenshot of one of the examples:

<figure>
    <a href="https://s3.amazonaws.com/com.leourbina.public/turtledemo.png"><img src="https://s3.amazonaws.com/com.leourbina.public/turtledemo.png"></a>
</figure>

# Shortcuts
There are several more commands that are available as executable modules, including tabnanny, timeit, trace, among others. And it seems like every new version of Python includes more of these. The only downside is that there seems to be no easy way to find these (I had to grep through the source of CPython to some of the more obscure ones presented here). Regardless, its amazing how much functionality comes bundled in in the Python standard library, and its all one command away. To ease typing I have the following aliases defined in my .bashrc:

{% highlight bash %}
alias venv='python -m venv'
alias serve='python -m htt.server'
alias pydoc='python -m pydoc'
alias pdb='python -m ipdb'
alias pytime='python -m timeit'
alias pyprof='python -m profile'
alias jcat='python -m json.tool'
alias cal='python -m calendar'
{% endhighlight %}

Hope you find these useful.



