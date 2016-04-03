
```
--------------------------------------------------------------------------
			Frequently Asked Questions (FAQ) - about mrxvt
--------------------------------------------------------------------------

Q:  I have a question...

A:  Have you read the README, README.configure, INSTALL, FAQ, TIPS, man page...?
    Before asking a question, please read through these documents for potential
    answers.

    Here is a good tutorial to ask questions:

	http://www.catb.org/~esr/faqs/smart-questions.html

-----

Q:  What exactly is the name of this project, materm or mrxvt?

A:  The current official name of this project is mrxvt (I honestly hope Rui
    Carmo will not reclaim the ownership of mrxvt because he is the first person
    who tried to hack a multi-tab rxvt on Cygwin and got this name for his
    project. Here is the link of his work:
    http://the.taoofmac.com/space/Projects/mrxvt).

    As stated in README, in the beginning, the project name was materm - derived
    from the project multi-aterm. And this was the name when I registered the
    project at sourceforge. Unfortunately it is not easy to rename materm to
    mrxvt at sourceforge. This is why we keep materm as the name at sourceforge.

-----

Q:  Why not use GNU screen? It has provided the multi-screen features!

A:  Because I do not like GNU screen. ;-)  People have their own flavors. Plus,
    mrxvt provides some features that GNU screen lacks, e.g., use mouse click to
    switch from different tabs... :p

-----

Q:  Why not use gnome-terminal/konsole? They have provided the functionality of
    mrxvt!

A:  Because they are heavy, slow, and depend on too many libraries. For example,
    gnome-terminal 2.6.1 in the Slackware -current tree depends on
    58 libraries, konsole of KDE 3.3 in the gentoo depends on 41 libraries,
    mrxvt with all features only depends on 19 libraries, and this number can be
    further reduced to 5 (of course you need to strip out some features, like
    background image ;-))!! Since all I need is a fast and lightweight X
    terminal emulator supporting multi-tabs, I decide to create mrxvt by myself.

-----

Q:  Will you rewrite mrxvt using C++? The object oriented feature of C++ can
    make mrxvt much modular.

A:  No! Because I do not like C++. C does not mean to be less modular or
    non-object oriented. It depends on how you do it. If you really like a C++
    implementation of rxvt, check out the rxvt-unicode project.

-----

Q:  Where is the CVS repository of mrxvt?

A:  From mrxvt-0.5.1, we have abandoned CVS, and now exclusively use Subversion.
    To check out from the subversion repository use

	svn checkout https://svn.sourceforge.net/svnroot/materm/mrxvt05b

-----

Q:  I find a bug, or I would like to see a new feature in mrxvt, how to report
    it?

A:  You can go to http://sourceforge.net/projects/materm and report the bug
    using tracker system. Alternately, you can email the mrxvt developer mailing
    list at materm-devel@lists.sourceforge.net. Please use the SourceForge bug
    tracker system for feature requests, or MAJOR bugs. Report minor bugs on the
    materm-devel mailing list. If in doubt, post to the devel mailing list, and
    you might be asked to file a feature request / bug report in addition.

    Please describe the bug as clearly as possible, so that we can replicate it.
    We can not (and WILL not) fix bugs we can not reproduce! Be sure to test if
    your bug is still present in the current version (from Subversion) of mrxvt.
    If the bug is still present in the subversion repository, then please report
    the system you are running mrxvt on, AND steps to reproduce the bug.

    Again, we can and will NOT fix bugs we can not reproduce. Tell us how to
    reproduce the bug, or your report will get (silently) ignored!
```