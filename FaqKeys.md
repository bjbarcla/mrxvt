
```
--------------------------------------------------------------------------
Frequently Asked Questions (FAQ) - keys
--------------------------------------------------------------------------

Q:  Shift+Left/Right do not always work. Ctrl+Shift+6 does not do anything.
    Some other keyboard shortcut do not work either.

A:  Do not report these issues as bugs. They most certainly are NOT. As of
    mrxvt-0.5.0, a lot have changed, this includes a few default keyboard
    shortcut sequences.

    Shift+Left/Right by default is only bound when you are in the primary
    screen. This is because good editors (i.e. Vim) use this keyboard
    combination to move between words. This of course can be changed by adding
    the following lines to your ~/.mrxvtrc

	Mrxvt.macro.Shift+Left:		GotoTab -1
	Mrxvt.macro.Shift+Right:	GotoTab +1

    but we recommend you use Ctrl+PgUP/PgDn or Ctrl+Shift+h/l to switch tabs
    instead.

    About Ctrl+Shift+6: Vim uses this sequence to switch to the "alternate"
    file. However when you press it in mrxvt, it seems to do nothing. This is
    because pressing Ctrl+Shift+6 by default moves the current tab to position
    6.

    Most terminal applications we know of so far can NOT tell the difference
    between Ctrl+Shift+number and Ctrl+number. Thus pressing Ctrl+6 in Vim will
    switch to the alternate buffer, so mrxvt can safely swallow the Ctrl+Shift+6
    keystroke.

----

Q:  My Control-Left / Right and Home / End keys do not work properly in some
    applications.

A:  To get your modified cursor keys working properly in Vim, just upgrade to
    Vim 7. The new behaviour in mrxvt is designed to work "out of the box" with
    Vim 7.

    To get your modified cursor keys working in bash / readline based
    applications put the following in your ~/.inputrc:

	# Cursor movement for mrxvt-0.5.x / xterm
	"\e[1;5C": forward-word
	"\e[1;5D": backward-word

	"\e[7~": beginning-of-line
	"\e[8~": end-of-line

    As of 0.5.0 the escape sequences mrxvt produces for modified cursor keys is
    the same as those produced by Xterm. If you do not like this behaviour,
    undefine the XTERM_KEYS macro in src/feature.h.

    Alternately you can use the macro feature of mrxvt to make the cursor keys
    produce the string that is expected by your application (explained later on
    in this FAQ)

-----

Q:  Mrxvt default hotkey ctrl+shift+minus conflicts with Emacs. How can I
    disable this mrxvt default hotkey?

A:  Put the following two lines into your mrxvt configuration file, usually
    ~/.mrxvtrc:

	Mrxvt.macro.Ctrl+Shift+underscore:	Dummy
	Mrxvt.macro.Ctrl+Shift+plus:	Dummy

-----

Q:  What's with the strange Backspace/Delete key behaviour?

A:  Assuming that the physical Backspace key corresponds to the BackSpace keysym
    (not likely for Linux ... see the following question) there are two standard
    values that can be used for Backspace: ^H and ^?.

    Mrxvt tries to inherit the current stty settings and uses the value of
    `erase' to guess the value for backspace.  If mrxvt wasn't started from a
    terminal (say, from a menu or by remote shell), then the system value of
    `erase', which corresponds to CERASE in <termios.h>, will be used (which may
    not be the same as your stty setting).

    For starting a new mrxvt:

	use Backspace = ^H
	$ stty erase ^H
	$ mrxvt
	or
	$ mrxvt --backspacekey ^H -e bash

	use Backspace = ^?
	$ stty erase ^?
	$ mrxvt
	$ mrxvt --backspacekey ^? -e bash

    NB: generate either value with BackSpace and Ctrl/Shift-BackSpace. Toggle
	with "ESC[36h" / "ESC[36l" as documented in "doc/refer.txt"

    For an existing mrxvt:
	use Backspace = ^H
	    $ stty erase ^H
	    $ echo -n "^[[36h"

	use Backspace = ^?
	    $ stty erase ^?
	    $ echo -n "^[[36l"

    This helps satisfy some of the Backspace discrepancies that occur, but if
    you use Backspace = ^?, make sure that the termcap/terminfo value properly
    reflects that.

    The Delete key (which one would expect to emit ^?) is a another casualty of
    the ill-defined Backspace problem.  To avoid confusion between the Backspace
    and Delete keys, the Delete key has been assigned an escape sequence to
    match the vt100 for Execute (ESC[3~) and is in the supplied
    termcap/terminfo.

    Some other Backspace problems:
	some editors use termcap/terminfo,
	some editors (vim I'm told) expect Backspace = ^H,
	GNU Emacs (and Emacs-like editors) use ^H for help.

    Perhaps someday this will all be resolved in a consistent manner ... and
    maybe xterm will have Home/End values too!

-----

Q:  Why doesn't the Backspace key work on my Linux machine?

A:  The XFree86 server has a notorious problem of mapping the Backspace key as
    Delete in order to match the Linux console.

    The correct way to fix this:

    0 - Complain to your Linux distributer and the XFree86 team, maybe they'll
	fix it.

    1 - Use xmodmap to correct the Backspace mapping

	! ~/.Xmodmap

	! a correctly-mapped BackSpace
	keycode 22 = BackSpace

	*** Make sure the keycode above matches the physical Backspace key on
	your machine!! (use xev) ***

    This will also fix the BackSpace problem with Motif applications, such as
    ``why doesn't Backspace work for Netscape?''

    Finally, you can also remap the mrxvt key-binding at run-time (next
    question).

-----

Q:  I don't like the key-bindings.  How do I change them?

A:  You can use the "macro" feature of mrxvt (available from version 0.5.0
    onwards). See the man page for details. For instance, if you want the
    application keypad keys to work like the regular cursor / home / end keys,
    then you can use something like:

	Mrxvt.macro.KP_Left:	Str \eOD
	Mrxvt.macro.KP_Right:	Str \eOC
	Mrxvt.macro.KP_Up:	Str \eOA
	Mrxvt.macro.KP_Down:	Str \eOB

	Mrxvt.macro.KP_Prior:	Str \e[5~
	Mrxvt.macro.KP_Next:	Str \e[6~
	Mrxvt.macro.KP_Home:	Str \e[7~
	Mrxvt.macro.KP_End:	Str \e[8~

	Mrxvt.macro.KP_Insert:	Str \e[2~
	Mrxvt.macro.KP_Delete:	Str \e[3~

    You can also use the macro feature to correct the notorious backspace
    problem, but it is better you fix that by complaining to your Linux vendor
    :).

-----

Q:  I'm using keyboard model XXX that has extra Prior/Next/Insert keys.  How do
    I make use of them?  For example, the Sun Keyboard type 4 has the following
    mappings that mrxvt doesn't recognize.

	KP_Insert == Insert
	F22 == Print
	F27 == Home
	F29 == Prior
	F33 == End
	F35 == Next

A:  Rather than have mrxvt try to accomodate all the various possible keyboard
    mappings, it is better to use `xmodmap' to remap the keys as required for
    your particular machine, or the use "macro" feature of mrxvt in your
    ~/.mrxvtrc.

-----

Q:  Sometimes you need to log into a remote system which lacks a decent rxvt 
    terminfo entry. The recommedation here is to set TERM=xterm (or to use the 
    option '-tn xterm') to get something close. But then Home and End keys don't 
    work as expected.

A:  Add a mapping in ~/.mrxvtrc:

Mrxvt.macro.Home:             Str \eOH
Mrxvt.macro.End:              Str \eOF

    These additional mappings may also help:

Mrxvt.macro.Shift+Home:       Str \e[1;2H
Mrxvt.macro.Shift+End:        Str \e[1;2F
Mrxvt.macro.Shift+Ctrl+Left:  Str \e[1;6D
Mrxvt.macro.Shift+Ctrl+Right: Str \e[1;6C

-----

Q:  Alt-1 (and Alt-2, ... etc ) don't work in readline (bash).

A:  Put these in your ~/.mrxvtrc file to let bash see the keystrokes which are 
    otherwise trapped by mrxvt and mapped to "GotoTab n":

Mrxvt.macro.Alt+1:           Dummy
Mrxvt.macro.Alt+2:           Dummy
Mrxvt.macro.Alt+3:           Dummy
Mrxvt.macro.Alt+4:           Dummy
Mrxvt.macro.Alt+5:           Dummy
Mrxvt.macro.Alt+6:           Dummy
Mrxvt.macro.Alt+7:           Dummy
Mrxvt.macro.Alt+8:           Dummy
Mrxvt.macro.Alt+9:           Dummy
Mrxvt.macro.Alt+0:           Dummy

-----

```