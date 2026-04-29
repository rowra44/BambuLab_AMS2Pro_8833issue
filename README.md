# BambuLab_AMS2Pro_8833issue
I created this repository to share my experience debugging my AMS2 Pro's issues with the feeders how I managed to fix it.

## When should you try looking into this / keep reading?
* in case of one or more **feeder units don't work**, don't spin at all or spin weakly. _(Basically any motoric issue generally caused by driver faults as you'll see)_
* my testing showed the **exhaust vent valves** could be related as well _(are those even motors?)_, so in case of their failures
* especially if your mentioned **issues** seem isolated / not global and **don't respond to swapping around identical parts, stay with the position and not the part**!

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

I opened a support ticket and carried on with life using only the other feeder units, obviously. By next day, **another feeder position showed the very same symptoms.**

## The issue identified
The AMS itself isn't scarily complicated. It has a daughter board at the back that has connectors to the 2 exhaust valves, 2 connectors (power & data) for the actual main motherboard and towards the outside it has the normally visible connectors _(AMS rail connector, aux power, etc.)_. **Since only two of the feeder units were affected it cannot be a general data communication failure, which strongly suggests a failure at the point at which the business logic gets "translated" into actual physical/electrical signals/power/etc. That is the main motherboard or at least parts of it.**

At first I tried checking the motherboard's components during usage with a laser thermometer _(I don't have an actual heat-cam)_ but it showed no real hotspots. However, I noticed there's an electrical whining noise when I tried spinning the faulty feeder positions' feeders _(from the P2S menu)_. Since by now I knew the feeders were fine themselves, I suspected a scenario where they don't get quite enough juice to start moving but they get _some_. I'm no electrical engineer by any means but **to me that mainly pointed to the motor driver ICs.**
Since the board was kind of clearly cooked anyways I decided to dig deeper. I identified 3 dual motor driver ICs identified by **HR8833mte** as can be seen on the following pic:
<img width="1992" height="550" alt="image" src="https://github.com/user-attachments/assets/d9608cb9-59d5-422c-a2ab-a3654b568fa0" />

Since the board was kind of clearly cooked by now and I have _some_ experience with hot air soldering ICs I decided to swap two of them around. By simply guessing their relative positions to the connectors of the affected feeder units' connectors, I decided to **swap the middle one with the one on the right** _(as can be seen on the picture above)_. This **resulted in some "dead" feeder unit positions returning to life, another going down and strangely enough, one of the exhaust vent valves reported faulty.** Now this fortified my reasoning behind that it _may_ worth looking into the driver ICs and replacing them.

## Replacements & results
I couldn't really find an exact replacement IC. I could find some HR8833 ICs but somewhat different batches and only shipped from China. I thought I'd look at electrical suppliers locally and I found Texas Instruments DRV8833 ones. At this point reading up on it I realized that the **HR8833 is a chinese knock-off of the TI DRV88833.** It should've been pin/drop-in compatible so I was like why the hell not I got some TI DRV8833 ones and at this point I felt like going all-in and **replaced all 3x HR8833 with 3x TI DRV8833.** Immediately, **every feeder unit**, vent and just about everything **was perfectly fine & back to life**, normal operation.

For the record here's the exact IC I used as replacements:
<img width="1069" height="1313" alt="image" src="https://github.com/user-attachments/assets/67e2d1d1-e0c1-44f5-a340-a1553e8f3a48" />


## Conclusion
I'm no electrical engineer, let alone experience PCB designer or expert at choosing the right IC. Take anything I say with a grain of salt. I know there are thousands of units out there doing absolutely happily & fine for years and thousands of hours of usage. I'm just saying mine was cooked in just under a few months and few hundreds hours of home / hobbyist usage and was simply fixed by swapping the ICs with the real deal. Would it have been fixed by using the same cheapo chinesium knock-offs? Absolutely. Will it last for years now with the OG TI ones? I wouldn't know, time will tell. Especially that the HR ones seem to have a higher supply voltage tolerance, actually _(12.8V instead of 10.8V of the TI)_.

Good luck everyone not need this at all and/or fixing it if you happen to need to!

I didn't do actual reverse-engineering of any sort but a kind _(and at this time an actually qualified electrical engineer)_ redditer **(@jmdejoanelli)** pointed out the feeders may be using the main 12V rail and thus even the HR8833 might've been pushed to kind of its limits. If that's true I may be actually overdriving the IT ones. We'll see as I don't really feel like messing around again with it till I absolutely need to.

## Warranty, scale of the issue
I'm not saying this is a global issue but trying to search for similar ones, you can find quite a few. Regarding warranty, Bambu's support team responds pretty quickly _(within hours / less than a day)_ and while they're not really keen on immediately sending out each and every electrical part's replacements to you with no questions asked, they're generally helpful and positive about providing replacement parts after some questions answered, video proofs sent, testing done etc.

I'm still hoping I'll receive a replacement motherboard under warranty, I didn't just yet.

## What will you need if you decide to do the same?
A hot-air soldering station capable of ~400C _(kinda scary but that was the point at which I was effectively & quickly melting the oe solder without heating absolutely everything around it up too much)_, lots of damn flux, pair of tweezers, usual SMD IC soldering stuff :] 
