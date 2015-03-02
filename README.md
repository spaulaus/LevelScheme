Level Scheme Generator
======================
-------------------------------------------------------------------------------

This is a code that one can use to generate publication quality level schemes
fairly easily. There is a little bit of manual setup that needs to be done, but
I am going to work under the assumption that people can follow/read direction.

I should note here that **all energies are in keV**.

The code is heavily commented. This is to both help the user and to help the
coder. I can go quite a number of months between looking at this code (sometimes
years). Some of the order of things may seem a little strange at first.
Postscript programming can be tricky for those that are not used to at stack
style of layout. Those of you who grew up using the old HP calculators will
feel right at home. Basically, it's RPN.

If you have questions or comments please feel free to send them my way.

##Features
Here is a list, in no particular order, of the features.

* Dashed levels/transitions - For tenative tricky types.
* Colored transitions - You can make the new ones pop with red, green, or blue
* Add the Pn, Qbeta, Sn, etc.
* Label levels with spin/parity log(ft) feeding intensity, etc. (or don't
  that's cool too).
* Intensity and energy labels for transitions
* Concidence labeling for transitions.
* Width of transition arrow denotes strength of transition
* All energies (with the current exception of the parent g.s.) are placed
  to scale.

Getting Started
===============
-------------------------------------------------------------------------------
The easiest thing to do to get rolling with this program is to just dive in. We
are going to start at the bottom and work our way up. This is going to be the
easiest due to the RPN style of the stack.

Most all of the placement of the various components will be handled
automatically. There are several aspects that may need to be addressed, but
we'll get to those. I have roughly put the following sections in order of what
you should do to fill out your level scheme.

Any of the function arguments that are in brances "()" are printed text. If
you do not want to use a particular one it may be left blank. This does not mean
that you can remove it from the function call. 

**Remember, you cannot deviate from the order of these function calls due to the
nature of the stack. A missing element from the line can completely break the
program.**

##Inserting Transitions
Now we are ready to start putting in our transitions. These have a good variety
of options (dashed, color, coincidence) that can be set. This provides some good
abilities to make nice level schemes. You'll notice that there's a command that
does a 90 degree rotation. This is so that we plot the labels for the arrows in
the right direction (i.e. parallel to the arrow).

For the transitions, the width of the arrows denotes the relative intensities of
the line. You can set all of them to be the same value (I like 10) if you do not
need this.

The coincidences are denoted by filled circles at the ends of the arrows. We
currently do not suppor the open style of the circles. 

###List of Arguments for "trans" procedure
* **Intensity -** The label for the intensity of the line
* **Energy -** The label for the energy of the transition
* **Efrom  -** The starting Y position of the arrow.
* **Eto -** The position of the tip of the arrow
* **Intensity -** The intensity of the line to be used for the width
* **Option Array -** The array for the options to be used
   + **Index 0 -** Set to be > 1 for dashed arrow
   + **Index 1 -** Set to be 1(=red), 2(=green), 3(=blue)
   + **Index 2 -** Set to be (up),(down),(both),(none) to set the coincidence
   for the line

###Example transition
Here it is.

        ((10)) (218) 314 96 18 [0 0 (none)] trans

This will define a level that has an intensity of 10, an energy of 218,
and is a transition between 314 keV and 96 keV. There is no special markings
used for that line. 

##Inserting levels
The first thing that we should do is insert the levels that we know about. The
levels are going to be positioned automatically. There are a number of optional
features that one can add.

There is a special level called "Mama" this is the level for the parent nucleus
in the event of some sort of decay scheme. You can comment most instances of this
if you just need a clean level scheme that is not going to be used for a decay.

###List of Arguments for the "level" Procedure
* **Spin     -** This denotes the spin/parity of the level that we're looking at
* **log(ft)  -** Sets the log(ft) for the level
* **Feeding  -** Sets the feeding for the level
* **Energy   -** The label for the energy level
* **Energy   -** The placement of the level in the picture.
* **Dunno    -** Honestly, I haven't tracked down the use of this variable yet,
  but removing it seems to break everything.
* **isDashed -** Sets a level to be dashed
* **isMother -** Sets if a level is the g.s. of the mother nucleus

###Example Level
Here is the function call for a level: 

        (5/2-) (5.5) (52(7)) (0.0) 0.0 0 0 0 level

This will set the ground state of the daughter nucleus to be at an energy of
0.0, with some additional labels. The "label" at the end of the line is the
call to the label procedure that actually draws the level.

If we do not need all of the other information we can do the following.

        () () () (195.5)  195.5  0 0 0 level

This will set the first excited state to be at 195.5 keV, and set none of
the other labels that were present before.

##Labels
We are now going to set the different labels of the level scheme. You can
comment out the lines that add these parts without affecting anything but the
look. 

**Take a good look at the various labels that are here. You will have to adjust
the values of some of them for your specific decay. In addition, you may
have to twiddle the positions to your liking.**

###List of (psudo-)optional labels floating around
I the names listed here are the same as the comments labeling the section of
the code. You can just search for this name and find it.

* **log(ft) -** Sets the label for the log(ft)'s associated with the levels
* **Intensity -** Sets the label for the feeding intenstity for the level
* **Pn -** Sets the Pn of the decay
* **Half-Life -** Sets the half-life of the decay
* **Q-value -** Sets the Q-value for the decay
* **Energy Label -** Sets the unit label for the energy levels.
* **Daughter Label -** Prints the label for the daughter nucleus.
* **Decay Arrow -** Prints an arrow denoting the decay.
* **Decay Type Label -** Prints the beta (+,-) associated with the decay.
* **Mother Spin/Parity -** Prints the spin/partiy of the mother nucleus
* **Mother Label -** Prints the label for the mother nucleus

###Example label:

        /Times findfont 20 scalefont setfont
        5 cm momYPos moveto (Q  = 6607(4) keV) show
        /Symbol findfont 14 scalefont setfont 
        5 cm momYPos moveto 14 5 neg rmoveto (b) show

This sets a label at the x-position of 5 cm and y-position at the same height
of the mother level.

##Variable Definitions
There are several variable definitions that you can twiddle. Some of these you
can edit, others you probably shouldn't. Feel free to play with them though.
All of these values are going to be in cm. 

* **/bbStX -** The starting x coordinate of the bounding box
* **/bbStY -** The starting y coordinate of the bounding box
* **/bbEnX -** The ending x coordinate of the bounding box
* **/bbEnY -** The ending y coordinate of the bounding box
* **/MaxY -** The Y position of the daughter ground state
* **/Mama -** The horizontal position of the mother level.
* **/LevelEnd -** The length of the energy levels
* **/ArrowX -** The starting position of the transitions, as taken from
the right
* **/ArrowDist -** The distance between the transitions.

##Bounding Box
Finally, once you get all of the bits and pieces put in place. You should adjust
the size of the bounding box of the eps. This is done at the very top of the
document. The line looks like this

        %%BoundingBox: 0 0 400 600

The values are xmin, ymin, xmax, ymax. I have noticed that you should define
the values here to be slightly larger than the values that you put into the
"/bb*" definitions. Not sure why at the moment, but I have a feeling it's
due to the auto positioning of things. 
