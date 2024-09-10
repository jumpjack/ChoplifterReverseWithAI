  ![image](https://github.com/user-attachments/assets/c202ff63-cc8a-4c65-bc34-3b12b3332be4) ![image](https://github.com/user-attachments/assets/c4b2d949-eb82-44a5-9bb7-de5750280b3f) 




# AI analysis

I will try to analyse through AI tools this reverse-engineered code, to make it visual.

I found that ChatGPT and other can easily analyse short pieces of the code and turn them into call tree in mermaid format, but they can't read the whole code, so I asked them to write a web page which gives same results, but currently I am still getting confused results: hundreds and hundreds of calls make the graph unreadable; I need to find an automated method to group routines ny tasks or something else.

Example of partial result (a .mermaid file viewed in  [https://app.diagrams.net](https://app.diagrams.net) ):

![image](https://github.com/user-attachments/assets/03b81a3d-b1c8-46d3-ad32-aedd54211eef)


Opinions/help/suggestions welcome in [discussions](https://github.com/jumpjack/ChoplifterReverseWithAI/discussions) section!


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

