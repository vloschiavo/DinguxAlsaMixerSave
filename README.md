# DinguxAlsaMixerSave
This project allows the user to save the alsamixer volume settings between boots/reboots/poweroff on OpenDingux operating systems like the RG350, RG350M, etc

OpenDingux, like a lot of it's Linux counterparts, uses a program called alsamixer to adjust the various audio volume levels for PCM, Headphone, etc.

# The issue:
The default aslamixer volume settings on my RG350M weren't set how I like them.  First, using the volume buttons produced big steps in volume (going up or down).  This normally wasn't a problem, but on certain games, the volume was either too loud and distored or too soft to hear clearly.  

So before I played certain games, I had to go into Settings -> Sound Mixer and adjust the "Headphone" option from the default (somewhere around '51') to something a little lower like: '31'.  The result was a much more fine control over the volume.  Each button press on the volume up/down buttons made a smaller adjustment in the resulting output volume to the speakers (or headphone jack).

The problem was, these settings weren't saved when you rebooted or powered off your device.

# The Solution:
There are at least two possible solutions (anyone have another one?):  

One solution (not the route I chose): is to modify the SquashFS filesystem and permanently set those volumes to something you like.
Another solution (the one I chose): is to use alsactl to save any changes you made in Alsamixer to your SD card and restore those saved settings on boot.

# Install:
The install is simple:  Use your favorite method to put the S91alsaSettings file from this repository onto your device.  You should put it in the /media/data/local/etc/init.d/ directory; this is on the TF1/Internal card.  The S91alsaSettings file is a bash init script that runs when you startup or shutdown/reboot your device.

# How to use:
-Boot your device
-Make any changes you'd like to the volumes: Go to the Settings tab, open the Sound Mixer app (This is alsamixer).  Then exit the Sound Mixer app by pressing select.
-Copy the S91alsaSettings file to your device using your favorite method.  It needs to go in this directory: /media/data/local/etc/init.d/
-Profit.

# What happens:
On startup, the S91alsaSettings init script is run with the "start" commandline option.  The script checks to see if you have an alsactl state file '/usr/local/etc/asound.state'.  If not, the script does nothing and your device volume is set to the system defaults.

If you do have the '/usr/local/etc/asound.state' file, the init script will load those settings from the file and reset your levels to your previous settings.

# Reset back to defaults:
If you mess up your alsamixer (Sound Mixer app) settings, they get saved.  This can be undone and you can revert back to the original defaults using this method.
.... fill this in:
Outline: ssh to device: run `/media/data/local/etc/init.d/S91alsaSettings reset` to reset to system defaults.


