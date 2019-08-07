### Who this is for
Me, primarily. Also for people outputting stereo, downmixed audio, and *maybe* raw/LPCM multichannel.

### Context
My audio receiver, and possibly most devices that function the same, likes to drop the connection when there's silence. This is annoying when starting content, resuming from pause, or intentional silence in the content. The solution I came up with is to automatically play an inaudible, on my receiver, audio file so the connection stays alive. 

The drawback from this is AC3 transcoding and potentially any bitstreaming outputs static because it cannot gain exclusive control. However, for the people who this is for they should not have this issue.

I'm aware of Kodi's low noise option, but it's not effective enough for my receiver. More options would help, especially for AC3 and the off the wall workaround I came up with for that.

### Usage
To use, download and copy this folder's contents into `/storage/.config/`.

If `autostart.sh` already exists, merge the contents manually.

----

#### Audacity

`bg.wav` was generated in Audacity via:
 - Set Project Rate (Hz) as desired. I used 8000.
 - Generate -> Tone
   - Waveform: Square
   - Frequency (Hz): 60
   - Amplitude (0-1): 0.1
   - Duration: Doesn't matter, longer is better as there may be a gap when it loops.
 - Set track's Gain to -36 dB
 - Export as WAV with default settings.

`autostart.sh` is run when libreelec starts. It runs `keepalive.sh` in the background.
`keepalive.sh` uses `aplay` to loop `bg.wav` indefinitely.
