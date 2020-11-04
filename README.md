# DinguxAlsaMixerSave
This project allows the user to save the alsamixer volume settings between boots/reboots/poweroff on OpenDingux operating systems like the RG350, RG350M, etc

The default aslamixer volume settings on my RG350M weren't set how I like them.  First, using the volume buttons produced big steps in volume (going up or down).  This normally wasn't a problem, but on certain games, the volume was either too loud and distored or too soft to hear clearly.  

So before I played certain games, I had to go into Settings -> Sound Mixer and adjust the "Headphone" option from the default (somewhere around '51') to something a little lower like: '31'.  The result was a much more fine control over the volume.  Each button press on the volume up/down buttons made a smaller adjustment in the resulting output volume to the speakers (or headphone jack).

The problem was, these settings weren't saved when you rebooted or powered off your device.

The solution is to put the S91alsaSettings file on your device in the /media/data/local/etc/init.d/ directory.  This is on the TF1/Internal card and is a bash script that runs on startup and shutdown of your device.

How to use:
-Boot your device
-Copy the S91alsaSettings file to your device at: /media/data/local/etc/init.d/S91alsaSettings
On startup, it checks to see if you have a alsactl state file '/usr/local/etc/asound.state'.  If not, the script does nothing and you device volume is the system defaults.
If you do have this file, it will load your settings.

