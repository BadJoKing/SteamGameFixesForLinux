# SteamGameFixesForLinux
Just some fixes for problems I came across while trying to run Steam games on Linux
These may or may not work for you. They did for me (on Linux Mint 21.2 Cinnamon, at the moment EndeavourOS), but depending on your distro and general setup your mileage may vary. 
I should probably also mention that, at the moment, I'm running on an all-AMD system, so I won't have anything regarding Nvidia GPUs recorded here (yet?).

## Skyrim Special Edition (with AE DLC)
The Problems:
* The game would initially not launch at all, the fixes I found for that are:
    * Don't make your Steam Library on an NTFS-formatted Drive. **Use ext4, btrfs or literally any other non-microsoft file system**.
    * I also didn't find exfat (for compatibility with Windows) to be a viable option since the same "Not-Starting" problem would reappear.
    * Also, I added the following Launch Options (note that these expect you to use the radv driver (vulkan-radeon package))
    ```
    WINEDLLOVERRIDES="xaudio2_7=n,b" PULSE_LATENCY_MSEC=90 mesa_glthread=true %command%
    ```
    * Now, when you do your own research, maybe on [Skyrim SE's protondb](https://www.protondb.com/app/489830), you might find that some people added a ```PROTON_USE_WINED3D=1 %command%``` option to their game. I didn't, because for me it would cause the performance to be abysmal. We're talking 1 FPS **at most** throughout the opening scene, average was more like 5 SPF. Your mileage may vary though, so you might wanna try it out, just to see if it works for you or not.
    * Another thing that I saw recommended was to **<ins>install amdvlk</ins>** and add the following options:```VK_ICD_FILENAMES="/usr/share/vulkan/icd.d/amd_icd64.json" %command%```. However, this again caused my game to just not start at all, so I removed the option. Again, your mileage may vary tho and maybe this information is of some value to you.
    * You might also want to try **<ins>deleting the wineprefix</ins>** for Skyrim SE (located somewhere around /path/to/steamapps/compatdata/489830/pfx) and relaunching the game. **<ins>BEWARE THOUGH,</ins>** since this also deletes your save games. So if you didn't save them to steam cloud, enable that or just copy over the folder and copy back the save states after you're done. 
* Skyrim runs now, but there's some very annoying **hitching** happening
    * I'm on Linux Mint 21.2 cinnamon right now and cinnamon has this annoying quirk where it doesn't disable compositing for fullscreen apps by default, so the way to do exactly that was to go to ```System Settings -> General -> Disable compositing on fullscreen applications```.
* I want to run Skyrim with mods, but **<ins>skse64_loader.exe can't launch Skyrim</ins>**:
    * You might want to verify your installed files, just to be sure everything's where it's supposed to be.
    * You probably installed MO2 to manage your mods and if that's the case, **uninstall** it along with SKSE64 and then **<ins>reinstall MO2 _without_ manually installing SKSE64</ins>**. It turns out MO2 already installs skse on its own and the MO2-installed version somehow works better than the manually installed one. 
* Skyrim gets **stuck** at **"Running Install script (Microsoft Visual C++)"**:
    * Clear the download cache.
* Skyrim starts, but gets **stuck at a black screen**:
    * You might want to verify your installed files, just to be sure everything's where it's supposed to be.
    * If you are big into modding:
       * You probably installed some mods via Vortex. If you add the launch option ```PROTON_LOG=1``` and look at the end of the steam-489830.log file(should be in your user home folder), if it says that it tries to load certain mods(e.g. EngineFixes or MCM as was the case with my install), you'll have to remove all Vortex created Files from the Data directory. A Vanilla Skyrim directory looks like in the file [FileTree-SSE-AE-detail.txt](./FileTree-SSE-AE-detail.txt) which can be found in this repo. You can use that as a reference and delete all directories/files that are "Not on the list" (get it? cuz Hadvar says that in the opening cutscene...). You also probably want to switch to [MO2](https://github.com/rockerbacon/modorganizer2-linux-installer), as it doesn't clutter the source directory with all the mods, but instead tells Skyrim to think that a seperate directory is definitely, really for sure in the source folder and that it should look there for mods (or so I remember. I read about that once like a year ago and I'm too lazy to factcheck that. Fact is that the source directory of Skyrim WILL be way cleaner under MO2).  
* Skyrim crashes when it tries rendering the world after clicking "New Game" (enbseries installed):
    * You might want to verify your installed files, just to be sure everything's where it's supposed to be.
    * You might have some enbseries files missing or incorrectly installed. Try un-/reinstalling enbseries and try it again.
* **Wine C++** Runtime "assertion failed" error pop-up on (GE-)proton 8 and newer:
   * Yeah, that's a problem with enbseries. If you want to keep enbseries and don't really care about the proton version then just **change it to something like GEProton-7-55** or earlier. If you want the newer proton version, I've got bad news for you as I haven't found a way yet to get it to work. I think I'll just **uninstall it** and substitute it with other mods like ELFX. I found a [list](https://www.reddit.com/r/skyrimmods/comments/103bcwd/enb_alternative/) of enb alternatives that you might wanna give a look. Note that this list is not exhaustive by any stretch of the imagination.

## Portal 2
The Problems:
* The game would start with a wrong aspect ratio and resolution
    * Add the Launch option ```-vulkan```
* The game stutters
    * Try [disabling compositing on fullscreen applications](https://linux-gaming.kwindu.eu/index.php?title=Compositor_(X11)#Cinnamon), if you are using Cinnamon as a desktop environment.

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
      * If you want to really keep amdvlk just add the launch option ```PROTON_USE_WINED3D=1 %command%```, but prepare for immense performance issues even just hovering your mouse over menu items. I'm not 100% sure if radv is what did it, but it's at least about 90% and that's plenty enough for me to recommend switching.
   * If you're on radv:
      * You might wat to try installing the game via lutris.

## Generation Zero
The Problems:
* Didn't launch:
		 * Switch to a (GE-)Proton Version 7 or earlier. This will launch the game, but crash when trying to play.
		 * Add ```PROTON_USE_WINED3D=1 %command%``` to the launch options. With this you can play on newer versions of proton. I recommend the new 9.0 beta version.
* The game is slow af:
		 * If you have a cpu with integrated graphics, it's using that instead of the dGPU. Add  ```DRI_PRIME=1 %command%``` to the launch options
* The colors are messed up:
		 * it's probably due to the pos anti aliasing in the game. You can either install [this mod](https://www.nexusmods.com/generationzero/mods/1) (follow the install instructions on the mod's site) or you can just disable it entirely if you go into the game's wineprefix (<steam library folder>/steamapps/compatdata/704270/pfx/drive_c/users/steamuser/documents/avalanche studios/saves/settings) there should be another folder with a bunch of random numbers in the name. In there should be a settings.json file. Find the variable responsible for AA and set it to 0. In game it'll still show some AA enabled, but that's not true. (credit for these fixes goes to [this steam community thread](https://steamcommunity.com/app/704270/discussions/0/1643164649208311196/)\
My current config:
The mod from nexusmods. Proton 9.0(beta). The following launch options:
```DRI_PRIME=1 PROTON_USE_WINED3D=1 %command%```
