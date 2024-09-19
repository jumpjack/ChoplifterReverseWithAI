  ![image](https://github.com/user-attachments/assets/c202ff63-cc8a-4c65-bc34-3b12b3332be4) ![image](https://github.com/user-attachments/assets/c4b2d949-eb82-44a5-9bb7-de5750280b3f) 




# AI analysis

I will try to analyse through AI tools this reverse-engineered code, to make it visual.

I found that ChatGPT and other can easily analyse short pieces of the code and turn them into call tree in mermaid format, but they can't read the whole code, so I asked them to write a web page which gives same results, but currently I am still getting confused results: hundreds and hundreds of calls make the graph unreadable; I need to find an automated method to group routines ny tasks or something else.

Example of partial result (a .mermaid file viewed in  [https://app.diagrams.net](https://app.diagrams.net) ):

![image](https://github.com/user-attachments/assets/03b81a3d-b1c8-46d3-ad32-aedd54211eef)


Opinions/help/suggestions welcome in [discussions](https://github.com/jumpjack/ChoplifterReverseWithAI/discussions) section!

## Simplified diagram

Obtained by manually extracting main game flow from source and feeding it to Claude AI: 

[![Schematic](https://github.com/user-attachments/assets/6aa26c0b-2541-4bcf-96b7-bed7ec5787ef)](https://github.com/user-attachments/assets/c2218d39-15ff-4ffd-a6d1-28b5da8eee2b)


- [Mermaid file on github](https://github.com/jumpjack/ChoplifterReverseWithAI/blob/main/choplifter-simple.mermaid)
- Schematic on draw.io
    -  [PNG](https://drive.google.com/file/d/16fPVil9pn5bM6Kw3KhkibLg-nzIeY2gQ/view?usp=sharing)
    -  [Vector](https://app.diagrams.net/?state=%7B%22ids%22:%5B%2216fPVil9pn5bM6Kw3KhkibLg-nzIeY2gQ%22%5D,%22action%22:%22open%22,%22userId%22:%22117363290921246306004%22,%22resourceKeys%22:%7B%7D%7D)

### chkButtonInput

Flowchart of joustick/keyboard manager; note the "cheat shortcut CTRL+L" to manually change level

[![image](https://github.com/user-attachments/assets/2931e5ff-9bf9-46e6-bdda-3079ec6dda0c)](https://github.com/user-attachments/assets/19c545ef-895a-4456-bc93-cf9109996c3e)




- [On github](https://github.com/jumpjack/ChoplifterReverseWithAI/blob/main/chkButtonInput.mermaid)
- [On Draw.io](https://drive.google.com/file/d/1Pd10MKvkZXtd_itzBqSMYZ5e9S-F1-0q/view?usp=sharing)

## Complex diagram

Game loop diagram with main subroutine calls

![image](https://github.com/user-attachments/assets/a606a744-91dc-4ec6-9c73-5c0ac95a1ac7)

- [on Draw.io](https://drive.google.com/file/d/10Mc4asAWg1l5h7Y6V1Q6bF24I9NsGwWN/view?usp=sharing)
- [On github](https://github.com/jumpjack/ChoplifterReverseWithAI/blob/main/choplifter-all.mermaid)

## mainLoopDemo diagram

[![image](https://github.com/user-attachments/assets/2129ac1b-d652-459f-8efc-49ff0b94b3f8)](https://github.com/user-attachments/assets/d614ce9e-0f55-4745-951a-cbd6c0e9f062)


- [English](https://github.com/jumpjack/ChoplifterReverseWithAI/blob/main/mainLoopDemo-all-eng.mermaid)
- [Italian](https://github.com/jumpjack/ChoplifterReverseWithAI/blob/main/mainLoopDemo-all-ita.mermaid)


# Resources

## Disassemblers
- [DirMaster](https://style64.org/release/dirmaster-v3.1.5-style) (.prg file extractor from .d64 disk images)
- [C64forever](https://www.c64forever.com/) - commercial ($10), free with limitations
- [Regenerator](https://csdb.dk/release/index.php?id=149429)  Multi System
- [C64 Debugger](https://csdb.dk/release/?id=172281) (now Retrodebugger)
- [6502bench](https://6502bench.com/)
- 

## Emulators
### Online
- https://c64.krissz.hu/online-playable-games/
### Offline
- Vice
- FRODO

----------


# Original readme.md

## Choplifter Reverse-Engineer

This is a full reverse engineer of the Apple II game Choplifter, written by Dan Gorlin in 1982. It was done clean-room style beginning only with the binary. I had no additional information about this game other than the disk image.

The source code here is fully documented and will build and run to a version of Choplifter that is binary-identical to the original, except for Dan Gorlin's custom floppy loader. In order to modernize this a bit, I wrote a new loader for it based on ProDOS, and this version boots Choplifter from ProDOS instead. Otherwise, it is identical.

For a full writeup about this reverse-engineer and how it was done (along with lots more information about this source code), see my blog post here:

[https://blondihacks.com/reversing-choplifter](https://blondihacks.com/reversing-choplifter)

A quick note on building this– the makefile ends by executing an AppleScript to launch Virtual II and boot the disk image. This is all obviously very Mac-specific and probably somewhat dependent on my local environment. If the build gets to the end and fails on the AppleScript (V2Make.Scpt) don't worry, the build completed and your disk image is good. You can remove the "emulate" target from the makefile if you're building on another platform, or if the AppleScript doesn't work for you.

This reverse engineer was complete by me, Quinn Dunki, on May 12, 2024, but this is of course still Dan's game and it is a brilliant piece of work. Reverse engineering it only *increased* my admiration of it. I doubt anyone would say that about most of what I've written in my career. 😁

Thanks Dan, for writing one of the best games on the platform, and I hope you don't mind that I did this to it.

