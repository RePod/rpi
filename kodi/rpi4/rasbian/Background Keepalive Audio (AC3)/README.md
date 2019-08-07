### Who this is for
Me, primarily. Also for people outputting Dolby Digital/AC3 up to 5.1.

I don't particularly recommend this for many reasons explained below, but it's something.

For what it's worth, I was doing something similar before with my Steam Link hardware just for stereo.

### Context
My audio receiver, and possibly most devices that function the same, likes to drop the connection when there's silence. This is annoying when starting content, resuming from pause, or intentional silence in the content. The solution I came up with is to automatically play an inaudible, on my receiver, audio file so the connection stays alive.

If you recognize this scenario from the stereo solution, great. I also wanted the ability to use my receiver's Dolby Digital/AC3 5.1 surround sound!

Unfortunately it's quite more involved and requires a second machine until I'm motivated to learn more about ALSA/Pulse. Rasbian is used until I do equal digging on LibreElec's audio handling, but Kodi on Rasbian is too old for the Pi 4 to be usable so Steam Link streams it remotely.

### Usage
You will need the following:

  - A dedicated PC capable of running Steam and reliably streaming Kodi (installed on that PC) via Remote Play
     - This may be swapped out for anything that can stream Kodi, reassign its audio output device, and that device have raw 5.1, like Moonlight
  - A *separate* audio player on the PC
     - Steam's built-in music playback is hit and miss, not recommended but can be considered
  - A Pi running Rasbian (I used Buster) or any other distro that supports the Steam Link application or alternative of your choice

**On the PC:**
  - Set up Kodi as you normally would
  - Add Kodi as a non-Steam game in Steam
  - Make sure the audio output device is default (so Steam can force it to switch) and the channels are set to 5.1
  - When you want to actually utilize this setup, play `bg.wav` in your media player on repeat at max volume (it's -36dB to begin with)
      - To generate your own `bg.wav` refer [to this](https://github.com/RePod/rpi/tree/master/kodi/rpi4/libreelec/Background%20Keepalive%20Audio%20(stereo%20or%20raw)#audacity)
      - Or just have it automatically start
      - Some may have noticed this is the *mixing step*, it may not be required but I don't know enough about ALSA/Pulse to do it on Rasbian
        
**On the Pi:**
  - Complete the Steam Link setup as normal. In the advanced settings set the audio stream to 5.1
      - Thus far Kodi will output 5.1 to Remote Play, and Remote Play will carry that 5.1 as is with `bg.wav` from the media player mixed in.
  - Create `~/.asoundrc` with the contents:
    - Warning: `pcm.!default` is quite greedy and will start on any audio source. It's recommended to not make any sound beforehand and maybe do this on another user if you do other things.

```
pcm.!default {
    type a52
    channels 6
}
```
  - I'm probably forgetting a step of getting the a52 encoder, but you'll need it
    - `libasound2-plugins`, for any issues refer to [this post](https://lb.raspberrypi.org/forums/viewtopic.php?p=1425779&sid=6cf7ddc3d31087977d7e03817caa37aa#p1425779) 
  - Reboot
  - Launch the Steam Link application, connect, then launch Kodi from Steam
  - The receiver should switch to DD5.1 mode, and `bg.wav` playing on the host machine should keep the connection alive.
  
This is by no means bulletproof and it's really combersome. But it was the easiest and quickest solution I could find until I can figure out sane mixing with ALSA/Pulse. The second machine did the mixing step so the Pi could encode the stream directly.

Optimally I would also want a solution for LibreElec similar to my non-AC3 solution but it seems to handle its audio outside of ALSA and Pulse (primarily) so it's difficult to mix the noise in when AC3 encoding.
