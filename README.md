# SteamGameFixesForLinux
Just some fixes for problems I came across while trying to run Steam games on Linux
These may or may not work for you. They did for me (on Linux Mint 21.2 Cinnamon, at the moment EndeavourOS), but depending on your distro and general setup your mileage may vary. 
I should probably also mention that, at the moment, I'm running on an all-AMD system, so I won't have anything regarding Nvidia GPUs recorded here (yet?).

> **<ins>Disclaimer:</ins>** These are all fixes that I'm pretty sure worked for me at least at some point. Some may be outdated and many of the reasons that I list for the issues are pure speculation on my part. Don't treat those parts as a definitive answer for why something doesn't work. Also don't use it as a reason to turn your wrath against a developer.

# General Fixes
## Problem 1: The game does not launch (completely) or it gets stuck while trying
### Solution 1: Verify Integrity
Try verifying the integrity of the game files under <code>Properties -> Installed Files -> Verify integrity of Game Files</code>. This can help with corrupted executables/game data or just a broken install due to modding or something like that.
### Solution 2: Recreate the wineprefix
You might also want to try <b><ins>deleting the game's wineprefix</ins></b> (located somewhere around [Steam Library Path]/steamapps/compatdata/[game ID]/pfx) and relaunching the game. <b><ins>BEWARE THOUGH</ins></b>, since this also deletes your save games. So if you didn't save them to steam cloud, enable that or just copy over the folder and copy back the save states after you're done. The Game ID can be found in the game's properties under the "Updates" tab.
## Problem 2: The game stutters/hitches
### Solution 1:
Try disabling the compositor before running games. At least on Cinnamon (the DE I'm on currently) you have to manually disable compositing for fullscreen applications in the settings. It's under the "General" tab the first option.

# Skyrim Special Edition (with AE DLC)
## Problem 1: It doesn't launch
### Solution 1: Wrong Filesystem
Don't make your Steam Library on an NTFS-formatted Drive. **Use ext4, btrfs or literally any other non-microsoft file system**. I also didn't find exfat (for compatibility with Windows) to be a viable option since the same "Not-Starting" problem would reappear.
### Solution 2: Add Launch options
Add the following launch options (note that these expect you to use the radv driver (vulkan-radeon package)):<br>
         <code>WINEDLLOVERRIDES="xaudio2_7=n,b" PULSE_LATENCY_MSEC=90 mesa_glthread=true %command%</code><br><br>
Now, when you look maybe on <a href="https://www.protondb.com/app/489830">Skyrim SE's protondb</a>, you might find that some people added a <code>PROTON_USE_WINED3D=1 %command%</code> option to their game. I didn't, because for me it would cause the performance to be abysmal. We're talking 1 FPS **at most** throughout the opening scene, average was more like 5 SPF. Your mileage may vary though, so you might wanna try it out, just to see if it works for you or not.
<br><br>
Another thing that I saw recommended was to <b><ins>install amdvlk</ins></b> and add the following options:<br>
         <code>WINEDLLOVERRIDES="xaudio2_7=n,b" PULSE_LATENCY_MSEC=90 VK_ICD_FILENAMES="/usr/share/vulkan/icd.d/amd_icd64.json" %command%</code>
## Problem 2: I want to run Skyrim with mods, but <b><ins>skse64_loader.exe can't launch Skyrim</ins></b>
### Solution 1: Verify just to be sure
You might want to verify your installed files, just to be sure everything's where it's supposed to be.
### Solution 2: 
If you installed MO2 to manage your mods, <b>uninstall</b> it along with SKSE64 and then <b><ins>reinstall MO2 <i>without</i> manually installing SKSE64</ins></b>. It turns out MO2 already installs skse on its own and the MO2-installed version somehow works better than the manually installed one.
## Problem 3: Skyrim gets <b>stuck</b> at <b>"Running Install script (Microsoft Visual C++)"</b>
### Solution 1: Clear the download cache.
Clear the download cache and start again.
## Problem 4: Skyrim starts, but gets <b>stuck at a black screen</b>
### Solution 1: Verify
Verify the installed files. It may be missing some critical resource/dependency that somehow doesn't throw an error, but instead hangs, or quietly fails while the game waits for an answer.
### Solution 2: Remove mods/Use MO2
If you installed mods <b>manually or via Vortex</b>, the game might try and fail to load them either due to missing dependencies, corrupted files or any other error with the mods. You can check whether that's the case by prepending <code>PROTON_LOG=1</code> to the launch options. If it says that it tries loading certain mods(e.g. EngineFixes or MCM as was the case with my game), you'll have to remove all the mods/Vortex created files from the Skyrim root folder. <a href="./FileTree-SSE-AE-detail.txt">This file</a> shows the fil tree of a vanilla Skyrim AE directory. You can use it to check whether you have/have deleted all the necessary files for a normal vanilla run. I would also personally recommend <b><a href=https://github.com/rockerbacon/modorganizer2-linux-installer>switching to MO2</a></b>, because it doesn't clutter the root folder with all the mods, but instead tells Skyrim to look at another folder for mods (or so I remember. I read about that once like a few years ago and I'm too lazy to factcheck that. One fact that I know for sure is that the source directory of Skyrim WILL be way cleaner under MO2).
<table>
   <tr>
      <td>Game Name</td>
      <td>Problem(s)</td>
      <td>Solution(s)</td>
   </tr>
   <tr>
      <td rowspan=3>
         General
         
      </td>
      <td>
         
      </td>
   </tr>
   <tr>
      <td rowspan="13"> 
         <b><ins>Skyrim Special Edition (with AE DLC)</ins></b> 
      </td>
      <td rowspan="4"> 
          
      </td>
      <td>
         
      </td>
   </tr>
   <tr>
      <td>
         
      </td>
   </tr>
   <tr>
      <td>
         
      </td>
   </tr>
   <tr>
      <td>
         
      </td>
   </tr>

   <tr>
      <td rowspan=2>
         
      </td>
      <td>
         
      </td>
   </tr>
   <tr>
      <td>
         
      </td>
   </tr>
   <tr>
      <td>
         
      </td>
      <td>
         
      </td>
   </tr>
   <tr>
      <td rowspan=2>
         
      </td>
      <td>
         
      </td>
   </tr>
   <tr>
      <td>
         
      </td>
   </tr>
   <tr>
      <td rowspan=4>
         Skyrim crashes when it tries rendering the world after clicking "New Game" (enbseries installed)
      </td>
      <td>
         You might want to try verifying the installed files.
      </td>
   </tr>
   <tr>
      <td>
         Maybe enbseries was installed incorrectly/the wrong version was installed. Try un-/reinstalling it and launch again. 
      </td>
   </tr>
   <tr>
      <td>
         One kind user named Eljeyna over on protondb mentioned, that enbseries isn't exactly friends with radv and will not work. You'll have to switch to amdvlk and install some dlls through this protontricks command: <code>protontricks 489830 d3dcompiler_43 d3dcompiler_47 d3dx11_43</code>. For switching between amdvlk and radv game by game Eljeyna recommend using a package called <b>amd-vulkan-prfixes</b> and the following launch options:
         <pre>
            <code>
               # Note: this 'requires' mangohud and gamemode to be installed. 
               # Mangohud adds an fps overlay and gamemode applies some tweaks
               # and changes to the OS while the game is running. 
               # If you don't need/want that, you can just leave out the 2nd and 3rd option.
               DXVK_FRAME_RATE=60 MANGOHUD=1 gamemoderun vk_amdvlk %command%
               # This here does the switching to amdvlk-----^
            </code>
         </pre>
         And if enb still doesn't work, you can override some winedlls. My current config is a combination of my solution to the first problem and Eljeyna's suggestions:
         <pre>
            <code>
               WINEDLLOVERRIDES="xaudio2_7=n,b","d3d11 = n,b","d3d12 = n,b","dxgi = n,b","d3dcompiler_43 = n,b","d3dcompiler_47 = n,b","d3dx11_43 = n,b" PUSE_LATENCY_MSEC=90 MANGOHUD=1 gamemoderun vk_amdvlk %command% 
            </code>
         </pre>
      </td>
   </tr>
   <tr>
      <td>
         It is also possible just substituting enbseries with other mods like <a href="https://www.reddit.com/r/skyrimmods/comments/103bcwd/enb_alternative/">these here</a> or you could also just use <a href="https://github.com/kevinlekiller/reshade-steam-proton">ReShade</a>. I haven't tried it though, so it might be possible that the same problem persists with reshade that was encountered with enb.
      </td>
   </tr>
   <tr>
      <td rowspan=1>
         Portal 2
      </td>
      <td>
         The game starts with the wrong aspect ratio/resolution
      </td>
      <td>
         Add the launch option <code>-vulkan</code>. If you have any additional launch options that require you to append a <code>%command%</code> to work, place the <code>-vulkan</code> <b>behind</b> that
      </td>
   </tr>
   <tr>
      <td>
         Satisfactory
      </td>
      <td>
         It didn't launch
      </td>
      <td>
         Added the launch options <code>LD_BIND_NOW=1 VKD3D_CONFIG=dxr PROTON_ENABLE_NVAPI=1 DXVK_ENABLE_NVAPI=1 %command% -NO_EOS_OVERLAY ‑USEALLAVAILABLECORES -nosplash -nothreadtimeout</code>. Not sure whether or not all of those are needed. Might want to <a href="https://en.wikipedia.org/wiki/Muntzing">muntz</a> around a bit, throw launch options at the game and see what sticks. (or in this case rip launch options out of the game) For example, I'm sure that you don't need VKD3D_CONFIG and DXVK_ENABLE_NVAPI configured simultaneously, since you can only use one at a time, but adding both doesn't seem to hurt, so I'm sticking with that for the time being.
      </td>
   </tr>
   <tr>
      <td>
         Icarus
      </td>
      <td>
         Didn't launch
      </td>
      <td>
         Added the launch options <code>PROTON_ENABLE_NVAPI=1 %command%</code>
      </td>
   </tr>
   <tr>
      <td>
         Sniper Elite 4
      </td>
      <td>
         It didn't launch/crashed while trying.
      </td>
      <td>
         Switch from amdvlk to radv with amd-vulkan-prefixes package and the following accompanying launch option:
         <code>vk_radv %command%</code>

         Alternatively, without switching to radv:
         <code>PROTON_USE_WINED3D=1</code>
      </td>
   </tr>
</table>

## Generation Zero
The Problems:
* Crashes at launch:
   * Switch to a (GE-)Proton Version 7 or earlier. This will launch the game, but probably crash when trying to play.
   * Add ```PROTON_USE_WINED3D=1 %command%``` to the launch options. With this you can play on newer versions of proton. I recommend the new 9.0 beta version.
   * UPDATE: Apparently dxvk got an update or something, because it works for me now without WINED3D. It might also be because I installed [reshade](https://github.com/kevinlekiller/reshade-steam-proton)
* The game is slow af:
   * If you have a cpu with integrated graphics, it's using that instead of the dGPU. Add  ```DRI_PRIME=1 %command%``` to the launch options. You can also go into the settings file at <steam library folder>/steamapps/compatdata/704270/pfx/drive_c/users/steamuser/documents/avalanche studios/saves/settings/<Directory with random numbers for a name>/settings.json and change the DisplayDxAdapter variable to 0.
* The colors are messed up. Everything is made up of only red, green, yellow or blue and nothing in between:
   > * it's probably due to the pos anti aliasing in the game. You can either install [this mod](https://www.nexusmods.com/generationzero/mods/1) (follow the install instructions on the mod's site, but install reshade from [this repository](https://github.com/kevinlekiller/reshade-steam-proton), because just running the exe through wine won't work) or you can just disable it entirely if you go into the game's wineprefix (<steam library folder>/steamapps/compatdata/704270/pfx/drive_c/users/steamuser/documents/avalanche studios/saves/settings) there should be another folder with a bunch of random numbers in the name. In there should be a settings.json file. Find the variable responsible for AA and set it to 0. In game it'll still show some AA enabled, but that's not true. (credit for these fixes goes to [this steam community thread](https://steamcommunity.com/app/704270/discussions/0/1643164649208311196/)
   * **_UPDATE:_** It's probably **not** because of the AA. My new theory is that WINED3D just can't handle the textures/shaders that well for some reason. The Fix I found for that is **[installing Reshade](https://github.com/kevinlekiller/reshade-steam-proton) (Follow the instructions in the README and _read the output_. especially the last few lines that tell you to add launch options/environment variables.)** and ditching WINED3D. If my other theory about why a similar config didn't work before is correct then ReShade should fix the crashing error that dxvk had.
> My current config:\
> The mod from nexusmods. Proton 9.0(beta). ReShade. The following launch options:\
> ```DRI_PRIME=1 WINEDLLOVERRIDES="d3dcompiler_47=n;dxgi=n,b" mangohud gamemoderun %command%```

## Valheim
The Problem:
* It's only using 4GB of ram:
   * Let me preface this by saying that I have not been able to validate if this actually works. I just read [here](https://steamcommunity.com/sharedfiles/filedetails/?id=2861463097) that this *generally* works, but it might as well not.
      * Adding ```-MaxMem=8192``` *should* double the available ram for Valheim
      * If Valheim is comiled for a 32bit OS though, then 4GB would be the maximum amount it can take at all. However I have little reason to believe this other than the fact that it limits it self to 4GB, because the system requirements list a 64-bit OS as a minimum requirement. I might be wrong tho, so don't take what I wrote here at face value.
