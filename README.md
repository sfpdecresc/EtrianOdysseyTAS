# EtrianOdysseyTAS  
Programmable Etrian Odyssey TAS  
  
## What Is It?  
This is a simple, programming language that compiles to DeSmuME's .dsm file. You write some code in this language, then compile it with the .html[javascript] interface, saving the output as a .dsm file that you then load into DeSmuME with Etrian Odyssey running.  
  
## Why Is It?  
Complete RNG manipulation for Etrian Odyssey has been discovered. RTA notes end up looking, more-or-less, like a TAS, since this game is mostly grid-walking and menuing. Since some of us already have TAS-like RTA notes, it would be nice to be able to just hand these notes to the emulator and have it run them for us. Because notes are slightly too complex, some compromises had to be made - but code still looks decently readable, so we can still use sort-of-human-written-looking notes as TAS input.  
  
## Notes And Warnings  
• Debugging a TAS in DeSmuME can be difficult. On the Apple OS versions, there's a handy little Execution Control window. There's a command called "Jump to frame number:". Its name is misleading. Think of it as "Fast-forward up until just before frame number:". If you load a TAS and then jump 1000 frames, DeSmuME will run the first 1000 input frames as fast as possible and then drop you off immediately before frame 1000. Use the #lbl instruction in your code to figure out where your breakpoints will be, pause the emulator, jump to that spot, and start debugging.  
• The compiler will not help you very much regarding syntax errors, unless you open up your browser's JavaScript console and read through the intermediate JavaScript to see where the error occurred. Both this language and its intended uses are so simple that you shouldn't run into very many issues.  
  
## Language Specification  
This is how the language should behave, not necessarily how it actually behaves.  
  
---  
  
### Short Instructions  
  
#### Button Push[es] On A Frame  
Example : LRSs  
Example : r  
A grouping of one or more of consecutive letters, from the following : urdlLRABXYTSG.  
• urdl : up,right,down,left on directional pad  
• LR : left,right triggers  
• ABXY : four thumb buttons  
• T : s[T]art  
• S : [S]elect  
• G : debu[G]  
Each mentioned button will be pushed at the same time.  
  
#### Wait Instructions  
Example : _  
Example : __  
One or two underscores.  
• _ : wait for one frame, not sending any inputs whatsoever  
• __ : wait for preset amount of frames, defaults to 1 until set with the #wst long instruction  
  
---  
  
### Long Instructions  
  
#### Draw A Line On The Touchsreen  
Example : @lin [68,68] [158,68]  
Draws a line from the start [x:0-255,y:0-191] to the end [x:0-255,y:0-191]. Northwest corner of the touchscreen is [0,0]. Buffers wait frames before and after the drawing instructions. No intermediate frames - starts at first point, immediately jumps to last point.  
  
#### Tap The Touchscreen  
Example : @tap [60,60]  
Taps the touchscreen at the [x:0-255,y:0-191] point. Northwest corner of the touchscreen is [0,0]. Buffers wait frames before and after the drawing instructions.  
  
#### Stylus Up At Specified Position  
Example : @sup [60,60]  
Send a raw stylus-up command for the touchscreen at the [x:0-255,y:0-191] point. Northwest corner of the touchscreen is [0,0]. No buffer frames, literally generates one command - used for TAS optimizations.  
  
#### Stylus Down At Specified Position  
Example : @sdn [60,60]  
Send a raw stylus-down command for the touchscreen at the [x:0-255,y:0-191] point. Northwest corner of the touchscreen is [0,0]. No buffer frames, literally generates one command - used for TAS optimizations.  
  
#### Input A Raw .dsm Command  
Example : @raw |0|.......A.....147 065 1|  
Send a raw command, in .dsm format.  
  
#### Reset The Console  
Example : @rst  
Reset the console. [!!!] Not sure whether this is a soft or hard reset.  
  
#### Set The Long-Wait Frame Count  
Example : #wst 15  
Sets the internal variable that controls how long the Long-Wait __ command waits, in frames. This variable defaults to 1 until set. Can be set any number of times, effects only the Long-Wait instructions following it.  
  
#### Breakpoint Labels  
Example : #lbl B1F_Encounter_23_START  
Sets an internal variable to mark this point in code. After compiling, the tool will inform you of the frame number that each label exists on. The compiler will automatically insert a START and END breakpoint for your convenience. Label names cannot contain spaces nor double quote marks.  
  
---  
  
### Loops  
Example : 60{A _}  
Example : 60{10{A _} _}  
Runs whatever is inside of the curly braces the specified number of times.  
  
---  
  
### Comments  
Example : // move cursor to Ambush, use it, exit menu  
Used to make the TAS code more readable, has no effect on code other than the // and everything after it being removed at the start of compilation.  
  