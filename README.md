# magic-carpet-vsync
Activates VSYNC in Magic Carpet

## The Problem
*Magic Carpet* for DOS runs at an uncapped framerate. Unlike modern game engines, it does not interpolate each frame by time; instead, it uses a fixed update rate.  
As a result, on a fast CPU the game runs at an unplayable speed. On the original hardware this wasn’t an issue, since it naturally ran at approximately the intended speed.

When emulating with DOSBox, you can fix this by limiting the CPU cycles to slow the game down to a reasonable rate.  

The issue with limiting CPU cycles in games like *Magic Carpet* is that the game can suffer severe slowdowns whenever too much is happening on screen.

## The Solution
Apart from rewriting the game engine to support variable framerates, another solution is to let each frame use as many CPU cycles as needed, but limit the number of frames displayed per second.  

The simplest way to do this is to wait for the monitor’s VSync before copying the framebuffer to video memory.

## The Method
During disassembly I found locations where the game writes to video memory, and I also discovered an existing (but incomplete and possibly unused) VSync routine in the original code.  

I updated that routine and added two consecutive calls to it after each frame. This limits the game to roughly **35 FPS**, which feels like a playable speed.

## The Patch
Currently this patch only works with the *Magic Carpet Plus* version dated: "Jun 05 1995.16:26:18"

I created a **BPS patch** that applies to the `CARPET.EXE` file in the game’s `CARPET` subdirectory, e.g.:

`C:\GAMES\CARPET\CARPET\CARPET.EXE`

I use BPS so that file checksums must match; the patch will not apply to the wrong version of the game.

## How to Apply
1. use any **BPS patching tool**: (online examples below)  
   - [Online BPS Patcher](https://www.marcrobledo.com/RomPatcher.js/)
   - [Hack64 Web Patcher](https://hack64.net/tools/patcher.php/)

2. Make a backup copy of your original `CARPET.EXE`.

3. Apply the patch:  
   - Open the patching tool.  
   - Select the provided `.bps` patch file.  
   - Select your original `CARPET.EXE` (from the correct version).  
   - Save the patched output (usually it will overwrite `CARPET.EXE`, so keep your backup safe).

4. Run the game as usual in DOSBox (or real hardware). The framerate should now be capped at ~35 FPS.

## Other Versions
It should be possible to create patches for other versions, but I don’t currently have access to them.  

A patch script could be written to search for the correct patch locations automatically. If there’s interest, I can add such a script or app to this repo.