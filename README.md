# DinguxAlsaMixerSave
This project allows the user to save the alsamixer volume settings between boots/reboots/poweroff on OpenDingux operating systems like the RG350, RG350M, etc

OpenDingux, like a lot of it's Linux counterparts, uses a program called alsamixer to adjust the various audio volume levels for PCM, Headphone, etc.

## The issue:
The default aslamixer volume settings on my RG350M weren't set how I like them.  First, using the volume buttons produced big steps in volume (going up or down).  This normally wasn't a problem, but on certain games, the volume was either too loud and distored or too soft to hear clearly.  

So before I played certain games, I had to go into Settings -> Sound Mixer and adjust the "Headphone" option from the default (somewhere around '51') to something a little lower like: '31'.  This resulted in each volume button press making more fine adjustments to the output volume (headphones or speaker).

The problem was, these settings weren't saved when you rebooted or powered off your device so I'd have to go in everytime, and more often then not, I'd just turn the sound off, which lessens the ambiance of the games.

## The Solution:
There are at least two possible solutions (anyone have another one?):  

One solution (not the route I chose): is to modify the SquashFS filesystem and permanently set those volumes to something you like.  This didn't seem flexible enough and is kinda a pain, requires extracting the SquashFS, making changes, then re-squashing it.  Not exactly a nimble or seamless user experience.

Another solution (the one I chose): is to use alsactl to save any changes you made in Alsamixer to your SD card and restore those saved settings on boot. Most importantly this is accomplished in the background without user intervention.

## Install:
The install is simple:  

1) Copy the S91alsaSettings file, using your favorite method, from this repository and onto your device. Put it in the /media/data/local/etc/init.d/ directory so that it runs on Startup and Shutdown.  This is on the TF1/Internal card.  

2) Make it executable: ssh into your device and run this: `chmod +x /media/data/local/etc/init.d/S91alsaSettings`

The S91alsaSettings file is a basic bash init script that runs when you startup or shutdown/reboot your device.

## How to use:
- Boot your device normally.
- Make any changes you'd like to the various volume levels by going to the Settings tab, open the Sound Mixer app (This is alsamixer).  Make changes using the d-pad.  I tend to turn down the "Headphone" level.  Then exit the Sound Mixer app by pressing select.
- When you turn off your device using the 'Poweroff' or 'Reboot' apps in the Settings tab, those volume level settings will get saved. 
- Profit.

## What happens:
On startup, the S91alsaSettings init script is run with the "start" commandline option by the linux init process.  The script checks to see if you have an alsactl state file '/usr/local/etc/asound.state'.  If not, the script saves your current settings as the __default__ for your device.  This will be handy if you mess up your settings and want to revert back to the system defaults.  At this point your device volume is set to the system defaults.

If you do have the '/usr/local/etc/asound.state' file (which would be true on subsequent boots), the init script will load those settings from the file and reset your levels to your previous settings.

## Reset back to defaults:
Let's say that you messed up your alsamixer (Sound Mixer app) settings and they got saved when you turned off the device.  You reboot, still can't hear anything and you're not sure what you need to do to restore the defaults.  This can be undone and you can revert back to the original defaults using this method.

ssh to device: run `/media/data/local/etc/init.d/S91alsaSettings load-defaults` to load the defaults that it previously saved.

If you don't like those, you can use the RG350M defaults by running: `/media/data/local/etc/init.d/S91alsaSettings load-rg350m`.  Your mileage may vary.

If you don't like any of those work, you can simply remove the /media/data/local/etc/init.d/S91alsaSettings file, reboot, and the operating system will go back to it's original defaults.

## Aside:
-OpenDingux (or at least the version I'm running on my RG350M) uses a similar method to save the "Volume" state.  This also uses an init script to save the volume state, but uses a different method.  It saves the state of the PCM level (volume buttons directly control the PCM level in alsamixer).  This function will continue to work, but runs before this script, so...this script will overwrite the PCM level.

## Feedback:
Let me know if it works for you and feel free to file issues or pull requests to add features or fix any bugs.




