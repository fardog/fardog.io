---
title: Fixing a Famicom Disk System
date: 2010-03-21T12:00:00-07:00
categories:
  - Gaming
tags:
  - Famicom
  - Famicom Disk System
  - FDS
  - Gaming
  - NES
  - Nintendo
  - repair
  - Sharp Twin Famicom
  - Twin Famicom
---

_**Update 2013-10-08:** There is now a
[great article available](http://www.famicomdisksystem.com/tutorials/fds-repair-mod/belt-replacement-adjustment/)
at the [Famicom Disk System](http://www.famicomdisksystem.com/) site, which I'd
highly recommend. See
[my addendum](http://milkandtang.com/blog/2013/10/08/an-addendum-to-fixing-a-famicom-disk-system/)
for more info._

This isn’t the usual fare from this blog, but this is good information that took
me quite some time to find. I’m also an avid video game collector, and i
recently picked up a [Sharp Twin Famicom][1] and a few [Famicom Disk System][2]
games; notably Kid Icarus and the infamous [Doki Doki Panic!][3], which later
became the USA/World _Super Mario Brothers 2_.

[1]: http://en.wikipedia.org/wiki/Twin_Famicom
[2]: http://en.wikipedia.org/wiki/Family_Computer_Disk_System
[3]: http://en.wikipedia.org/wiki/Doki_Doki_Panic

Anyhow, my Twin Famicom came with Super Mario Brothers 2 (known as the Lost
Worlds outside of Japan) which works perfectly, but the game on the back (some
sort of Tennis game) wouldn’t ever work. I didn’t think much of it, until both
of my new games didn’t work either. I would receive an Err.21 or Err.22 message
each time I attempted to load the games. The drive would attempt to read, come
to a full stop (**note:** this is important!) and then attempt to read again,
shortly before throwing the error message.

So, my Twin Famicom came with a broken belt, which is very common, and the
seller shipped the system with a new belt that would have to be [installed][4].
This part went swell, but apparently there’s more to be done. There’s a lot of
[random forum posts][5] out there, telling you to adjust the potentiometer on
the motor to adjust the drive speed, or the [read head][6] on the disk drive.
Adjusting the pot was simple, but didn’t give me any results, and the read head
is factory set (and thoroughly waxed/glued in) and seemed a little too dangerous
to adjust, so I didn’t want to try.

[4]:
  http://www.pinkgorillagames.com/retro_reviews/simply_ness_guide_to_famicom_d.php
[5]: http://www.digitpress.com/forum/showthread.php?t=140944
[6]: http://www.famicomworld.com/forum/index.php?topic=392.0

I found a lot of passing mentions to aligning the spindle, but no real good
information on how to do so, until I came across this [forum post][7] by “Zach”:

[7]: http://digitpress.com/forum/showpost.php?p=364900&postcount=97

> The tough part (for me, at least) comes when you have to phyiscally align the
> drive. The first time I replaced a belt, the drive worked (without any
> alignment) for most of my games, but not some others.  
> [...]  
> So what you have to do is reach in from the front of the drive, loosen the
> SMALL black allen head set screw that holds the grey metal block to the
> spindle, then rotate the big gear with the belt slot around until it clicks
> (you’ll see the spring-loaded piece snap into place, dropping down a steep
> notch). At that point, you have to rotate the big gear another quarter turn.
> This is the position where you want the assembly to be when you have the
> little, black set screw pointing straight out the front of the drive, so line
> up the smal, grey metal disk pickup block (which should be free to spin around
> the spindle since the set screw is loose) , and tighten the set screw with
> your little allen wrench. Try it out and see if more disks are loading (or see
> if you’ve messed up the drive even more!)

I followed his instructions by performing the following:

1.  Remove the FDS drive from the system, and disassemble the following:
    - Front faceplate and drive door
    - Bottom Plate
2.  Look inside the drive from the front (where the drive door was), and spin
    the spindle (from the gear at the bottom of the drive) until the small allen
    screw faces you.
3.  Loosen the screw, and keep the allen key in the screw to hold the spindle
    still.
4.  Spin the drive pulley clockwise until you hear a small click (clockwise
    should be observed by looking at the bottom of the drive. The spindle (if
    you were looking straight down at the top, would be moving
    counter-clockwise).
5.  Once you hear the small click, spin one quarter turn clockwise, and tighten
    down the spindle (allen screw).

I half reassembled things to see if the trick worked, and I was getting a new
behavior! Now, the drive would spin up, attempt to read, and go to a black
screen! The drive would continue spinning (and wouldn’t come to a full stop mid
read, very important!), and then eventually the Famicom BIOS screen would
return, with Err.27. I was on the way!

From here, I did the following:

1.  Loosen spindle, and turn the pulley 5 degrees or so clockwise, then
    retighten the spindle.
2.  Try a disk again.

I repeated those steps until the drive began exhibiting somewhat similar
behavior as before (coming to a full stop during the read operation). Then I did
the same, only moving 5 degrees counter-clockwise instead; and you know what?

**IT WORKED.**

Stupid thing loaded. It turns out that the systems are aware of the spindle
position, and if it isn’t correct it will cause them to read the header data
incorrectly, causing the games to fail. Afterwards, I reassembled the system and
all of my games have been loading flawlessly.

Someday in the future maybe I’ll turn this into a guide with pretty pictures and
everything, but for now hopefully this’ll help someone get their FDS working.

For reference, here’s a list of FDS Error Codes:

> ERROR 01 Disk not correctly inserted. (No Disk Card)  
> ERROR 02 Battery error. Check power adaptor or batteries.  
> ERROR 03 Broken prong on disk card.  
> ERROR 04 Wrong gamemaker ID.  
> ERROR 05 Wrong game name.  
> ERROR 06 Wrong version name.  
> ERROR 07 A, B side error (eject disk, turn and insert disk again).  
> ERROR 08 Disk #1 wrong.  
> ERROR 09 Disk #2 wrong.  
> ERROR 10 Disk #3 wrong.  
> ERROR 20 screen data differs.  
> ERROR 21 Disk header block(_NINTENDO-HVC_) part is wrong.  
> ERROR 22 Disk header block reecognition #$01 isn’t read and cant be ignored.  
> ERROR 23 File recognition block #$02 can’t read for several reasons and cant
> be ignored.  
> ERROR 24 File header block recognition #$03 can’t read and cant be ignored.  
> ERROR 25 File data block recognition #$04 can’t read and cant be ignored.  
> ERROR 26 Can’t save properly to disk card.  
> ERROR 27 Block end mark seen and ends prematurely.  
> ERROR 28 The disk unit and the same period can’t take it.  
> ERROR 29 The disk unit and the same period can’t take it.  
> ERROR 30 Disk card too full to save.  
> ERROR 31 Data number of a disk card doesn’t match up.

Then, a commenter named "Jon" posted the following:

> Thanks for posting this! When I got a “broken” FDS recently this helped direct
> me towards the cure.
>
> I did find a much more direct & accurate way to align these though. It can be
> done during belt replacement & should eliminate the need for fine-tuning
> afterward.
>
> How? When the gear assembly retainer (triangular piece) is removed for belt
> replacement, simply point the rectangular notch in the large white gear to the
> largest notch in the metal cam below it.
>
> Maintain those positions during reinstall & you should be good to go. No need
> to hunt for a hex key (1.5mm BTW) to adjust the disk catch on the spindle &
> all positioning remains at original factory settings.
