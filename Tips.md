

# Can I search the scroll back buffer #
> Yes. Well sort of. The following two macros (bound to Ctrl+Shift+? and Ctrl+/ respectively) will save your scroll back buffer to a file and open it (in a new tab) in less and vim respectively. When opened in less, you can see the same colors as you do on your screen. When opened in Vim, you can edit / cut & paste, but can't see the same colors (unless you write your own syntax file, and use the "conceal" patch by Vince Negri).
```
Mrxvt.macro.Primary+Ctrl+Shift+question: PrintScreen -ps perl -e '$_=join("",<STDIN>); s/\n+$/\n/g; print' > /tmp/scrollback
Mrxvt.macro.Primary+Add+Ctrl+Shift+question: NewTab "(Search)" /bin/sh -c "less -ifLR +G /tmp/scrollback; rm /tmp/scrollback"

Mrxvt.macro.Primary+Ctrl+slash: PrintScreen -s perl -e '$_=join("",<STDIN>); s/\n+$/\n/g; print' > /tmp/scrollback
Mrxvt.macro.Primary+Add+Ctrl+slash: NewTab "(Search)" /bin/sh -c 'view +"syn off|set nospell notitle |normal G" /tmp/scrollback; rm /tmp/scrollback'
```

> NOTE: If you use Vim-6.4, you probably want to remove the "nospell" Vim option. Opening in Vim is especially useful if Vim is compiled with X support. In this case, Vim will synchronize your visual selection to the X clipboard buffer, and you can paste it into other tabs using Ctrl+Shift+v.
> The perl command just strips the trailing new lines from your buffer. If you want them, replace it with "cat". Remember that once you view the scroll back buffer in less / vim, new output in the original tab will not be visible in less / vim. Also the scroll back buffer is in the file /tmp/scrollback. If you are working on some highly secret government project, and don't want any one else to be able to see your scroll back buffer, umask it correctly, or replace /tmp/scrollback to some private location in your home directory.

# Can I set the tab title to the name of the current command #
> Yes. The instructions depend on which shell you use.
> If you use bash version 3.1 or later, then keep in mind that a few distributions set PROMPT\_COMMAND in a manner that destroys the current tab title. You should either unset it, or set it to something useful. The following code snippet sets the tab title to the current command, and sets PROMPT\_COMMAND to set your bash prompt to include a shortened version (3 path components) of the working directory in your shell prompt. Put this somewhere in your ~/.bashrc: (Make sure it is only read by interactive shells. E.g. do something like [[$- != \*i\* ](.md)] && return in the beginning of ~/.bashrc)
```
function set_title()
{
    [[ "$BASH_COMMAND" == "$PROMPT_COMMAND" ]] && return

    echo -ne "\033]0;" > /dev/tty
    #[[ $USER == root ]] && echo -nE "su -- " > /dev/tty
    echo -nE  "$BASH_COMMAND (${PWD/#$HOME/~})" > /dev/tty
    echo -ne "\007" > /dev/tty
}

function set_prompt()
{
    # Also set the command prompt to only expand the last three directories
    local pwdtail=${PWD/#$HOME/\~}
    [[ $pwdtail =~ ^(/[^/]+|~)/.+/([^/]+/[^/]+)$ ]] &&	\
        pwdtail="${BASH_REMATCH[1]}/.../${BASH_REMATCH[2]}"
    PS1="${PS1_HEAD}${pwdtail}${PS1_TAIL}"
}

# Set shell prompt
PS1_HEAD=
if [[ -n $SSH_CONNECTION ]]; then
    PS1_HEAD+='\[\e[33m\]'
    [[ $USER == "root" ]] && PS1_HEAD+="root@"
    PS1_HEAD+='\h:'
else
    [[ $USER == "root" ]] && PS1_HEAD+='\[\e[33m\]root:'
fi
PS1_HEAD+='\[\e[32m\]'
PS1_TAIL='\[\e[0m\]$ '

# Change the window title of X terminals 
if [[ $TERM =~ xterm|rxvt && -z $NO_TITLE ]]; then
    set +o functrace
    trap 'set_title' DEBUG
fi
```

> If you use tcsh, then add the following to your ~/.tcshrc:
```
# Change the window title of X terminals
set backslash_quote
if ( $TERM =~ xterm* || $TERM =~ rxvt* ) then
    alias jobcmd echo -n '"\e]0;"\!#:q "($cwd)\a"' \> /dev/tty
    alias cwdcmd echo -n '"\e]0;"\!#:q "($cwd)\a"' \> /dev/tty
endif
unset backslash_quote
```

> If you use zsh, then add the following to your ~/.zshrc:
```
# Change the window title of X terminals
if [[ $TERM =~ '^(xterm|m?rxvt)' ]]; then
    function preexec()
    {
	typeset -g prevcmd;

	[[ -n $2 ]] && prevcmd=$2
	{ print -n "\e]0;"; print -Rn $prevcmd; print -Pn " (%~)\a" } > /dev/tty
    }

    # Add [done] to title when command completes.
    function precmd()
    {
	[[ -n $prevcmd ]] && preexec "" "$prevcmd [done]"
    }

elif [[ $TERM == screen* ]]; then
    function preexec()
    {
	echo -n "\ek${2%% *}\e\\" > /dev/tty
    }

fi
[[ -n $functions[preexec] ]] && \
    function chpwd()
    {
	preexec
    }
```

> NOTE: The above will only work starting from mrxvt-0.5.1 and up. On previous versions replace ]0 with ]2, ]61, or ]62.
> If you also want the window title to reflect the current command, then in addition to one (or more) of the above, add the following line to your ~/.mrxvtrc:
```
Mrxvt.syncTabTitle:    True
```

# Can I set the mini-icon of the mrxvt window to reflect the command I am running? #
> Not through mrxvt directly. But, if you use Fvwm the you can do this as described in [this page](FvwmMiniicon.md).

# Can I have a fullscreen mrxvt window? #
> Of course! Start mrxvt with mrxvt --smoothResize --fullScreen to get a full screen window. You can also use the Ctrl+Shift+F keystroke to switch between a full screen and windowed terminal. Keep in mind though, that unless the --smoothResize option is enabled, your full screen terminal window might have a small border.

# Can I use Dtach (a screen manager similar to the "GNU Screen" project) with Mrxvt? #
> Yes you can! Instructions can be found [here](http://www.eskimo.com/~roger/programming/mrxvtanddtach.html)!

# Can I make the tabbar transparent, without the white casting color, like in mrxvt 0.4.2? #
> Yes you can! This is achieved by using the '-itabbg black' switch. An example of this would be:
```
mrxvt -tr -trt -itabbg black
```

# Can I make the scrollbar transparent, without the white casting color, like in mrxvt 0.4.2? #
> Unfortunately you can only do this if you set your scrollbar style to "plain", currently, and then use the "-troughColor black" switch.
> Work is currently being done so that the -troughColor switch has some effect on your scrollbar background regardless of what scrollbar you choose to use.
> An example command to set transparency (without the white color cast) for your scrollbar in the 0.5.x series would be:
```
mrxvt -tr -trs -ss plain -troughColor black
```

# How do I take full advantage of the 256 colors? #
> See [here](Colors.md) for a full page of tips (shell, vim, mutt, and possibly more)

# How to made tabs squared? #
> Starting from version 0.5.1 mrxvt has rounded tabs, but if you want to bring back old style of tabs, you must configure mrxvt with --with-tab-radius=0

# Why do I have odd looking characters at times (like when I'm viewing a man page)? #
> This may occur if you are using a UTF-8 locale. You can check this by typing 'locale' on the command line. To see a list of alternate locales type 'locale -a' at the command line. See [http://www.madboa.com/geek/utf8/](http://www.madboa.com/geek/utf8/) for more details on locales.

# Does mrxvt support UTF-8? #
> At the current time mrxvt does not fully support UTF-8, though Jimmy (a.k.a Terminator) is working on this. If you would like to work on this please feel free to, and submit a patch to the mailing list.