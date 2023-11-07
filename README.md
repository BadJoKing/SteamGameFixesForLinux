# SteamGameFixesForLinux
Just some fixes for problems I came across while trying to run Steam games on Linux
These may or may not work for you. They did for me (on Linux Mint 21.2 Cinnamon, at the moment EndeavourOS), but depending on your distro and general setup your mileage may vary. 

## Skyrim Special Edition (with AE DLC)
The Problems:
* The game would initially not launch at all, the fixes I found for that are:
    * Don't make your Steam Library on an NTFS-formatted Drive. **Use ext4**.
    * I also didn't find exfat (for compatibility with Windows) to be a viable option since the same "Not-Starting" problem would reappear.
    * Also, I added the following Launch Options
    ```
    WINEDLLOVERRIDES="xaudio2_7=n,b" PULSE_LATENCY_MSEC=90 mesa_glthread=true %command%
    ```
    * Now, when you do your own research, maybe on [Skyrim SE's protondb](https://www.protondb.com/app/489830), you might find that some people added a ```PROTON_USE_WINED3D=1 %command%``` option to their game. I didn't, because for me it would cause the performance to be abysmal. We're talking 1 FPS **at most** throughout the opening scene, average was more like 5 SPF. Your mileage may vary though, so you might wanna try it out, just to see if it works for you or not.
    * You might also want to try **deleting the wineprefix** for Skyrim SE (located somewhere around /path/to/steamapps/compatdata/489830/pfx) and relaunching the game.
* Skyrim runs now, but there's some very annoying **hitching** happening
    * I'm on Linux Mint 21.2 cinnamon right now and cinnamon has this annoying quirk where it doesn' disable compositing for fullscreen apps by default, so the way to do exactly that was to go to ```System Settings -> General -> Disable compositing on fullscreen applications```.
* I want to run Skyrim with mods, but **skse64_loader.exe can't launch Skyrim**:
    * You might want to verify your installed files, just to be sure everything's where it's supposed to be.
    * You probably installed MO2 to manage your mods and if that's the case, **uninstall** it along with SKSE64 and then **reinstall MO2 _without_ manually installing SKSE64**. It turns out MO2 already installs skse on its own and the MO2-installed version somehow works better than the manually installed one. 
* Skyrim gets **stuck** at **"Running Install script (Microsoft Visual C++)"**:
    * Clear the download cache.
* Skyrim starts, but gets **stuck at a black screen**:
    * You might want to verify your installed files, just to be sure everything's where it's supposed to be.
    * If you are big into modding:
       * You probably installed some mods via Vortex. If you add the launch option ```PROTON_LOG=1``` and look at the end of the steam-489830.log file, if it says that it tries to load certain mods(e.g. EngineFixes or MCM as was the case with my install), you'll have to remove all Vortex created Files from the Data directory. A Vanilla Skyrim directory looks like in the file FileTree-SSE-AE-detail.md which can be found in this repo. You can use that as a reference and delete all directories/files that are "Not on the list" (get it? cuz Hadvar says that in the opening cutscene...)
* Skyrim crashes when it tries rendering the world after clicking "New Game" (enbseries installed):
    * You might want to verify your installed files, just to be sure everything's where it's supposed to be.
    * You might have some enbseries files missing or incorrectly installed. Try un-/reinstalling enbseries and try it again.



## Portal 2
The Problems:
* The game would start with a wrong aspect ratio and resolution
    * Add the Launch option ```-vulkan```
* The game stutters
    * Try [disabling compositing on fullscreen applications](https://linux-gaming.kwindu.eu/index.php?title=Compositor_(X11)#Cinnamon), if you are on Linux Mint Cinnamon.

## Satisfactory
The Problem:
* It just didn't launch. The fix for that was to add the following launch options: ```LD_BIND_NOW=1 VKD3D_CONFIG=dxr PROTON_ENABLE_NVAPI=1 DXVK_ENABLE_NVAPI=1 %command% -NO_EOS_OVERLAY ‑USEALLAVAILABLECORES -nosplash -nothreadtimeout```
   * Not sure whether or not all of those are needed. Might want to [muntz](https://en.wikipedia.org/wiki/Muntzing) around a bit, throw launch options at the game and see what sticks. (or in this case rip launch options out of the game)

## Icarus
The Problem:
* Didn't launch. Added Launch options: ```PROTON_ENABLE_NVAPI=1 %command%```

## Sniper Elite 4
The Problem:
* Didn't Launch:
   * If you're on the **amdvlk** driver:
      * Screw it and switch to radv, maybe install vkd3d via protontricks
      * If you want to really keep amdvlk just add the launch option ```PROTON_USE_WINED3D=1 %command%```, but prepare for immense performance issues even just hovering your mouse over menu items. I'm not 100% sure if redv is what did it, but it's at least about 90% and that's plenty enough for me to recommend switching.
   * If you're on radv:
      * You might wat to try installing it via lutris.
