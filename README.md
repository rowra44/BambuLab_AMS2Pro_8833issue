# BambuLab_AMS2Pro_8833issue
I created this repository to share my experience debugging the AMS2 Pro's issue with the 8833 motor driver ICs and how I managed to fix it.

## My story / setup
Last December I got a Bambu Lab P2S combo that comes with an AMS2 Pro. First month I've been using it kind of a lot, as you'd expect, then I had some time off and now I believe these last months I've been using it as a completely normal, expected residential hobbyist. The whole setup right now has **421 hours of runtime** and that includes printing and drying as well. I only ever use higher grade materials, either OE Bambu or Spectrum, Fiberion and such filaments. I am using quite a bit of abrasice GF/CF materials, not that it should matter at all in this scenario as you'll see.

## What happened?
Like I mentioned the printer reports 421 hours of usage as of right now, which regarding feeder units is divded to 6 parts:
* 4 of the AMS2 Pro feeder units
* 1 part AMS HT unit
* 1 part no exterior feeder for TPU and such
Admittedly, these parts are not equal and the vast majority was fed using the AMS2 Pro. Let's settle with **~400 hours**. At this point I got an error message about some communication error with a feeder unit of some sort. I did the usual things but the **affected feeder unit in the AMS wasn't even grabbing material and it wasn't even spinning the motor**, it was down.

## What I tried to identify the faulty part(s)
Following Bambu's own procedures, I disassembled the AMS completely, checked all cable connections, looked for visible damage, contamination, stuck filaments whatsoever. Since there was absolutely nothing and I was already at this disassembled stage I decided to **swap the supposedly broken feeder unit with another**. After reassembly & testing the issue was stuck with the position and not with the feeder unit. Next I **swapped their connecting cables** as well which yielded no other results. At this point it was clear that **all the feeder units themselves are completely functional** and it's some issue before them.
I opened a support ticket and carried on with life using only the other feeder units, obviously. By next day, another one showed the very same symptoms.

## The issue identified
The AMS itself isn't scarily complicated. It has a daughter board at the back that has connectors to the 2 exhaust valves, 2 connectors (power & data) for the actual main motherboard and towards the outside it has the normally visible connectors _(AMS rail connector, aux power, etc.)_. **Since only two of the feeder units were affected it cannot be a general data communication failure, which strongly suggests a failure at the point at which the business logic gets "translated" into actual physical/electrical signals/power/etc. That is the main motherboard or at least parts of it.**

At first I tried checking the motherboard's components during usage with a laser thermometer _(I don't have an actual heat-cam)_ but it showed no real hotspots. However, I noticed there's an electrical whining noise when I tried spinning the faulty feeder positions' feeders _(from the P2S menu)_. Since by now I knew the feeders were fine themselves, I suspected a scenario where they don't get quite enough juice to start moving but they get _some_. I'm no electrical engineer by any means but **to me that mainly pointed to the motor driver ICs.**
Since the board was kind of clearly cooked anyways I decided to dig deeper. I identified 3 dual motor driver ICs identified by **HR8833mte** as can be seen on the following pic:
<img width="1992" height="550" alt="image" src="https://github.com/user-attachments/assets/d9608cb9-59d5-422c-a2ab-a3654b568fa0" />


