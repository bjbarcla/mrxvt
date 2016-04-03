
```
--------------------------------------------------------------------------
Frequently Asked Questions (FAQ) - colors & effects
--------------------------------------------------------------------------

Q:  I run mrxvt with `-o 25' option, but the window is not translucent!

A:  First make sure your X server support translucent, such as Xorg 6.8.1. Next,
    make sure the translucent extension is enabled in the X server. You can use
    command `xdpyinfo' to check whether the Composite extension is in the
    extension list. Then make sure you have run the program `xcompmgr' to enable
    translucent.

-----

Q:  When I run mrxvt in transparent mode or background image mode, can I make
    the background a little darker?

A:  Yes, you can. Just tint the background to black using the --shading and
    --tintColor options:

	Mrxvt.transparent:		True
	Mrxvt.transparentScrollbar:	True
	Mrxvt.transparentTabbar:	True
	Mrxvt.tintColor:		#000000
	Mrxvt.shading:		85
    
    You can use Ctrl+Shift+J / K to change the shading amount when mrxvt is
    running.

-----

Q:  Why the tinting does not work after I set the tint color in command line or
    ~/.mrxvtrc?

A:  First, tinting only works for user supplied background image or transparent
    background. Secondly, you have to set the shade option as well.

-----

Q:  I am tired with the current tinting color, can I change the color without
    restart mrxvt?

A:  Yes, you can. Control-RightClick on the terminal window. Select the
    "Transparency / Tint Background / <favourite color>" menu options.
    Alternately you can run use an escape sequence as follows:

	$ echo "\e]66;red\a"

    You can have macros / menu entries to do the same. See also the program
    settitle.c and mrxvtset.pl supplied with the source, and mrxvt_seq.txt.bz2
    for a complete list of escape sequences supplied by mrxvt.

-----

Q:  What's with this bold/blink stuff? I can never get blinking text!

A:  It is not possible, and likely will never be, for mrxvt to have actual
    blinking text. Instead (if mrxvt was compiled without NO_BOLDCOLOR),
    bold/blink attributes are used to set high-intensity foreground or
    background colors ... like what you'd see on a PC video adapter.  There are
    programs, notably John Davis' SLang-based ones
    ftp://space.mit.edu/pub/davis, that use bold/blink attributes to permit 16
    colors.

	color0-7	are the low-intensity colors.
	color8-15	are the corresponding high-intensity colors.

    A side issue of this bold/blink stuff is the question of how the normal
    default foreground/background colors are to be treated.  If the default
    foreground/background match one of the low-intensity colors (color0-7), the
    bold/blink attribute will invoke the appropriate high-intensity color
    (color8-15).

    In the case that the default foreground doesn't match one of the
    low-intensity colors, the bold attribute will use an `overstrike' to
    simulate a bold font. But note this leaves pixel-droppings and so, rather
    than wasting an inordinate amounts of energy to fix it, its use is simply
    deprecated.

    In the case that the default background doesn't match one of the
    low-intensity colors, the blink attribute is simply ignored (rather than
    representing it as bold as xterm does).

-----

Q:  I don't like the screen colors.  How do I change them?

A:  You can change the screen colors at run-time using ~/.mrxvtrc resources (or
    as long-options) ... see the man-page.

    Here are values that are supposed to resemble a VGA screen, including the
    murky brown that passes for low-intensity yellow:

	Mrxvt.color0:   #000000
	Mrxvt.color1:   #A80000
	Mrxvt.color2:   #00A800
	Mrxvt.color3:   #A8A800
	Mrxvt.color4:   #0000A8
	Mrxvt.color5:   #A800A8
	Mrxvt.color6:   #00A8A8
	Mrxvt.color7:   #A8A8A8

	Mrxvt.color8:   #000054
	Mrxvt.color9:   #FF0054
	Mrxvt.color10:  #00FF54
	Mrxvt.color11:  #FFFF54
	Mrxvt.color12:  #0000FF
	Mrxvt.color13:  #FF00FF
	Mrxvt.color14:  #00FFFF
	Mrxvt.color15:  #FFFFFF

-----
```