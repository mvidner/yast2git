What
----

This is the harness used to convert [YaST][1]
[Subversion repository][2] to Git.

The plan to do it is outlined in [this message][3].

[1]: http://en.opensuse.org/YaST
[2]: http://svn.opensuse.org/viewvc/yast/trunk/ 
[3]: http://lists.opensuse.org/yast-devel/2011-03/msg00005.html

How
---

You need a local copy of the subversion repository. It takes about 1.5GiB.

    # use svnsync...

The tool used is [svn2git from KDE][4]. To compile that:

    zypper install libqt4-devel subversion-devel
    qmake
    make

The conversion is run by `rake`. On my machine the 64000 revisions take about
35 minutes. The resulting bare repository has 1.1GiB, which can be packed
further to 700MiB. A clone with the trunk working copy needs additional
140MiB.

Each rake invocation assumes that you changed the config and want to compare
the results to the previous try, so it creates a timestamp-named directory and
works inside that.

[4]: http://gitorious.org/svn2git/svn2git
