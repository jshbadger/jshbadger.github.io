+++
title = 'Sway on Windows'
date = 2025-06-17T22:00:40-06:00
lastmod = 2025-06-18T22:00:40-06:00
draft = false
+++

One line summary: A guide to setup Sway on Windows through WSL 

## 0. Prerequisite Notes
This guide assumes a basic understanding of using terminals. Hypothetically, a complete novice *could* follow this guide successfully, but it would be strange for a complete novice to be interested in installing [Sway](https://swaywm.org/). Perhaps the novice reader stumbled on this guide and is curious what it is about. For this curious reader, you are free to read through the guide, though I suggest going through Software Carpentry's [The Unix Shell](https://swcarpentry.github.io/shell-novice/) lesson before seriously considering setting up Sway.
## 1. Install WSL
I am following the [install instructions from Microsoft](https://learn.microsoft.com/en-us/windows/wsl/install) to install WSL. 

WSL is powerful even outside of Sway. For example, you can mount drives formatted for Linux (ext4, btrfs, etc) using WSL. 

To install WSL, simply run the command
```PowerShell
wsl --install
```
in PowerShell.
Note that the default distribution is Ubuntu. To change which distribution you are installing, use the `-d` flag. If you are not sure which distribution you want to install, you can use `wsl --list --online` in PowerShell to browse your options. I am installing Debian. Thus, I ran the command
```PowerShell
wsl --install -d Debian
```
After downloading and installing WSL, you will have to reboot your machine to finish the distribution installation.
### Installing WSL through the Microsoft Store
If the terminal scares you, or you prefer a GUI to decide which distribution to use, distributions can be installed through the Microsoft Store. I have never installed WSL from the Microsoft Store, but I have used it to download additional distributions.
### Installing Any Distribution
If your distribution is neither listed in PowerShell nor in the Microsoft Store, [this guide](https://learn.microsoft.com/en-us/windows/wsl/use-custom-distro) provides a method for any Linux distribution to be installed (I assume if the desired distribution matches your [CPU architecture](https://dev.to/bhaveshgoyal182/arm-vs-x86-whats-the-difference-and-why-it-matters-in-2025-1efn)).
## 2. Setup the Linux Distribution
Upon reboot, the installation will finish for your distribution. With Debian, the loading bar was stuck on "Installing: 0%" for a while during one of my installs. Giving it around 5 minutes, the install eventually completed.

You will be asked to provide a username and password for the distribution. Note that this username and password is separate from your Windows's user, along with nearly all the files in your Linux distribution. Find out more about using files and commands between operating systems [here](https://learn.microsoft.com/en-us/windows/wsl/filesystems).

After providing login credentials, it is best practice to update everything. On Debian based distros, use `sudo apt update && sudo apt upgrade -y`.
## 3. Installing Sway and PowerToys
### Installing Sway
Installing Sway on Debian and Ubuntu is easy: just run
```Bash
sudo apt install sway
```
All the basic necessities will be downloaded.

For distros other than Debian, Ubuntu, Arch, and ElementaryOS, I can't guarantee Sway will be supported. You will have to use your own Google-fu to figure it out.

### Installing and Using PowerToys
If you are unfamiliar with Sway (and i3), Sway relies heavily on a modifier key. By default, this is the super key. However, the super key is the Windows key, which Windows uses for many tasks. For this guide, we will be repurposing the Caps Lock key to be our modifier key.

[Download and install PowerToys](https://learn.microsoft.com/en-us/windows/powertoys/). Once installed, open it (it will open by default after install) and navigate to `Keyboard Manager` (under `Input/Output` when opening from scratch rather than opening after install). Enable it and click `Remap a key`. We will be remapping Caps Lock to Num Lock. If you want to keep Caps Lock, find another key on the keyboard you are willing to sacrifice and map it to Num Lock. If you want to keep Num Lock, then the next best option is to remap to Alt. Alt isn't a great target, as Linux distros use it as well, but it's better than nothing.

## 4. Setting Up Sway
The following instructions are developed for Debian. Different distros might have slight variations for the locations of Sway install files, but the principles should be the same.

Sway operates from a single config file. The default config file is located at `/etc/sway/config`. The user defined config file is expected to be located at `~/.config/sway/config`. If this file does not exist, Sway uses the default config file.

Because we used PowerToys to change the modifier key from the Windows key to Num Lock (or Win (Right)), we will need to change Sway's config file. It is best practice to leave the default config alone and copy it into the user's directory. On Debian, the `~/.config/sway` directory is not created automatically and is done manually using `mkdir`. The default config is then copied over using
```bash
cp /etc/sway/config ~/.config/sway/
```

Then, using your preferred text editor (vim or nano are common choices), edit the config file located at `~/.config/sway/config`. On line 10 of the config you will see the command
```Vim
set $mod Mod4
```
Mod4 is the left super key, or the left Windows key on the keyboard. Earlier, we changed Caps Lock to Num Lock, which is `Mod2`. If you changed your sacrificial key to Alt, then your modifier key is `Mod1`. In summary:

| Mod  | Key                       | Comment                                                                                |
| ---- | ------------------------- | -------------------------------------------------------------------------------------- |
| Mod1 | Alt                       | Alt is common to use in both Windows and Linux, and is not advised to use if possible. |
| Mod2 | Num Lock                  | Most advised to use. However, can be problematic for numpad lovers.                    |
| Mod3 | Right super (Windows) key | Not usable due to conflicts with Windows.                                              |
| Mod4 | Left super (Windows) key  | Not usable due to conflicts with Windows.                                              |

Whichever mod you ended up using, change `set $mod Mod4` to the required key. for me it was changed to
```Vim
set $mod Mod2
```

If you want to keep Alt and Num Lock, the best path appears to be changing how Sway interprets your keyboard. In other words, you'll have to remap your keyboard on Sway. The program to do this appears to be with xkb. However, I have not been able to get this to work.

Once you have changed the modifier key, you're ready to launch Sway. In the terminal of your Linux distribution, simply type
```bash
sway
```
## 4. Essentials of Sway
These are the bare minimum commands to survive Sway. Sway isn't very well documented online, but it is designed to be a continuation of i3, so if you're having trouble finding answers to Sway navigation questions, try searching your question with i3 instead of Sway.

The menu produced by Sway will be separate from your Linux distribution's terminal window. If, at any time, you need to force close your Sway window, you can switch back to your distro's terminal and terminate Sway with `Ctrl+C` (`^C`). 

To move the Sway window around on Windows, use `Win+<Arrow Keys>`. For fullscreen for example, use `Win+Up Arrow`.

To open a terminal in Sway, use your modifier key (`mod`) and hold it down as you press enter (`mod+enter`). Press `enter` as many times as you want to get as many terminals as you want.

To open apps, use `mod+d`.

To make the current app full screen, use `mod+f`.

To close the currently highlighted window, use `mod+Shift+q`.

To exit Sway, use `mod+e`.

To move to other virtual desktops, use `mod+<Num>` where `<Num>` is a number from 0-9. You can see which virtual desktop is open on the top of the screen.

Switch between active windows with `mod+<Arrow keys>` (or, if you're used to vim, you can replace `<Arrow keys>` with jkhl).

Move the active window around by adding shift: `mod+Shift+<Arrow keys/vim>`. You can even move the window to other virtual desktops with `mod+Shift+<Num>`.

Switch from side-by-side windowed view to above/below with `mod+e`.

Switch to a view more like tabs with `mod+w` or `mod+s`. Return to the original side-by-side formatting with `mod+e`.

A cool feature is the scratchpad which can be accessed with `mod+minus`. By default, there is no window here, so you can move the currently selected one to the scratchpad with `mod+Shift+minus`. Now if you press `mod+minus` it will pop back up. When you open a window from the scratchpad, it is in a floating state. Floating windows can be moved around either with the cursor or with `mod+Shift+<Arrow keys/vim>`. They can also be made larger or smaller with `mod+r`. Note that windows moved to the scratchpad will keep the dimensions you change them to. You can also add windows behind the floating ones. Go ahead and try with `mod+enter`. You can switch between focusing on the static and floating windows with `mod+Spacebar`. You can remove a file from the scratchpad the same way you would move a window between the static and floating state: `mod+Shift+Spacebar`. Finally, you can add multiple windows to the scratchpad and cycle through them by repeatedly typing `mod+minus`.

A more advanced feature in Sway/i3 is to create a sort of sub-group of windows. For example, let's say I want to split the windows in half, but on the left I want one window, and on the right I want two. This can be done by opening the first two windows, moving to the window on the right and pressing `mod+v`. You'll notice the bottom of the current highlighted window is brighter than the other edges. The next window that is added will be placed on this highlighted side below. You'll notice that you can organize these windows on the right without affecting the window on the left (`mod+w`, `mod+e`, etc.). They are effectively in there own sub-group. To select the subgroup, use `mod+a`. To move windows out of the subgroup, use `mod+Shift+<Left Arrow/h>` to pull it back out. Once all windows have been moved out of the sub-group, the sub-group will dissolve. You can do this same procedure to put a window to the right instead of below by replacing `mod+v` with `mod+b`.

Other controls can be deduced from the Sway config file, [i3 docs](https://i3wm.org/docs/), or the internet.
## 5. Customizing and Enhancing Sway
Sway, like i3, is highly customizable. And nearly all of it can be done in the ~/.config/sway/config file. This is great if you use many different computers, as a single .config directory can control an entire working environment, making it quick and easy to transfer between nearly any Linux OS.

Anything not included here is likely on the [Sway Wiki](https://github.com/swaywm/sway/wiki) or in somesone else's config file on GitHub.
### Background Image
Though the glorious default background of Sway isn't too bad, it's nice to be able to customize it.
The command for deciding your background is already provided in Sway's config file. We're just going to add an image of our choosing and direct Sway to its location in your Linux distro. Personally, I prefer to keep the backgrounds close to my config file by making the directory at `~/.config/sway/backgrounds` and putting all desired backgrounds there.

To get your background in WSL, you can either use wget, download a browser in your Linux distro (see [Vieb](#vieb) for more info), or transport an image from Windows into your distro. Because the first two methods are relatively intuitive to the Linux minded reader, I will explain the latter. 
On Windows, open file explorer. On the left side where you see Quick access, This PC, etc., you will see a new directory called Linux. Windows appears to be able to access Linux by treating the files as a network share; a clever way to handle differences in file system formatting. We can simply navigate our Linux distro's directories to drag and drop our Windows's background image into the desired directory on the Linux distro.

If you consider Windows the scorn of the Earth, you can access Windows files from your Linux distro by going to `/mnt/c`. This will give you access to everything in your Windows's C drive. Feel free to navigate to your background and copy it over to your distro with `cp`.

Now that the Linux distro has a new background image, go into your `~/.config/sway/config` file and navigate to the line with `output * bg /usr/share/backgrounds/sway/Sway_Wallpaper_Blue_1920x1080.png fill` and replace the directory to `Sway_Wallpaper_Blue_1920x1080.png`. Note that for me because mine is located in the `.config` directory that I can use `~/.config/sway/backgrounds/...` to change my background image. However, for best practices, I tend to replace `~` with `/home/<user>/`.

If you edited this config file within Sway, you can reload the config file and check if it worked with `mod+Shift+c`.
### Sway Windows Look
I'm not a huge fan of the blue outline of the sway terminals; they look dated. So, let's change them!

In the `~/.config/sway/config` file, add the following. I think it can go anywhere, but I prefer to put it at the beginning of the `### Output configuration` section of the config file.
```vim
## Sway windows look
# Remove boarders
default_border pixel 3
smart_borders on
gaps inner 5
#Change colors of the Sway windows
# class                 border  backgr. text    indicator child_border
client.focused          #aec6cf #aec6cf #ffffff #000000 #aec6cf
client.unfocused        #000000aa #000000aa #ffffffbb #000000aa #000000aa
client.urgent           #dd1b16bb #dd1b16bb #ffffff #dd1b16bb #dd1b16bb
```
You will have to restart Sway from your distro's terminal to see these effects completely.
### Alacritty (and other terminals)
The default terminal for Sway is foot. It's an acceptable terminal, but others are better. My personal pick is alacritty.
Simply install the desired terminal through your package manager. For me, it's just `sudo apt install alacritty`. Once installed, go into your `~/.config/sway/config` file and replace `foot` with the name of your terminal. The line in my config file looks like 
```vim
set $term alacritty
```
Modifications can be made as specified by your terminal.
### Tofi
When you hit `mod+d`, you are using a program called dmenu. There's nothing wrong with dmenu, but [tofi](https://github.com/philj56/tofi) is better. It's easier to customize and wicked fast. Install with your package manager (apt for Debian) and go into your config file, and replace
```vim
set $menu dmenu_path | dmenu | xargs swaymsg exec --
```
with
```vim
set $menu tofi-drun | xargs swaymsg exec --
```
This is for running .desktop files. If you want tofi to search and run any executable, you can use 
```vim
set $menu tofi-run | xargs swaymsg exec --
```
The default theme doesn't look the best, so let's change it by creating a new file and folder at `~/.config/tofi/config`:
```vim
width = 100%
height = 100%
border-width = 0
outline-width = 0
padding-left = 35%
padding-top = 35%
result-spacing = 25
num-results = 5
font = monospace
background-color = #000A
```

### Gnome Apps
You can download and use Gnome apps as you would expect to use them in Gnome! A personal favorite is Nautilus (files) thanks to its implementation of Apple's Quick View through sushi.

### Clipboard Between Windows and Linux
Based on Jordan Koehn's [sway-wsl2](https://github.com/jordankoehn/sway-wsl2) install script, you can use wl-clipboard (installed with apt) to allow you to share your clipboard between Windows and Linux, allowing easy copy and paste between the two. Add the following to the `~/.config/sway/config` file:

```vim
# When wayland clipboard changes, run clipboard-to-win
exec wl-paste --watch clipboard-to-win
# Run win to clipboard service to sync windows clipboard to wayland
exec systemctl --user restart win-to-clipboard
```
And then run then follow the instructions from [sway-wsl2](https://github.com/jordankoehn/sway-wsl2) to install and setup the scripts to copy over the clipboards. 
In one of my installs, I had trouble getting the win-to-clipboard service to work; systemctl was reporting permission issues. The fix was to go to the scripts generated by Jordan's install script at `~/.local/bin` and make them executable with `chmod +x clipboard-to-win` and `chmod +x win-to-clipboard`.
### Multiple Monitors
Also from Jordan Koehn's [sway-wsl2](https://github.com/jordankoehn/sway-wsl2), use
```vim
exec swaymsg create_output

workspace 1 output WL-1
workspace 8 output WL-2
```
to allow multiple windows to be created by Sway, which can then be formatted on Windows onto multiple monitors.
### Vieb
Even though Windows almost certainly has a web browser, it is still helpful to have a browser within Sway. You could go for your default Chrome or Firefox, but if you're used to vim, why not use your keyboard magic in a browser? [Vieb](https://vieb.dev/) allows this.

To install, you need to go to the [download page](https://vieb.dev/download) and install the binaries for your distribution. For Debian based distros, you can download the deb files and install with 
```bash
sudo dpkg -i vieb_xx.x.x_axx64.deb
```
To get the .deb file in your distro, you can use wget and the link to the download from the wsl command line (not through Sway).

In my case, after running the `dpkg` command, there were some dependencies missing. These can be installed with
```bash
sudo apt update && sudo apt install -f
```

#### Electron Apps on Wayland
Note that Vieb is an Electron app. For electron apps, you need to specify that they run in Wayland mode. To do this, go to the .desktop file for Vieb at `/usr/share/applications/vieb.desktop` and replace the flag `--ozone-platform-hint=auto` to `--ozone-platform-hint=wayland`.
### Swaybar
We can change the look of the bar at the top of the screen as well. This is called swaybar. It (like most other things) is changed in your Sway config file. My setup is 
```vim
bar {
    position top

    # When the status_command prints a new line to stdout, swaybar updates.
    # The default just shows the current date and time.
    status_command while date +'%Y-%m-%d %H:%M:%S'; do sleep 1; done

    colors {
        statusline #ffffff
        background #000000aa
        inactive_workspace #32323200 #32323200 #5c5c5c
#                         border |bkgnd  |text
	focused_workspace #ffffff #000000aa #ffffff
    }
}
```
This sets the time to military time, the background translucent, and the color of the virtual desktop indicator to black and white rather than blue.

If you want some more power than swaybar, you can use [waybar](https://github.com/Alexays/Waybar) instead.
