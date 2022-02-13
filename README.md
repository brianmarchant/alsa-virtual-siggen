ALSA Virtual Soundcard : Signal Source
======================================

Based on the 'minivosc' project [here](http://www.alsa-project.org/main/index.php/Minivosc).

This is an extension of the above that can provide a sine-wave source at various sample-rates and in various formats.
It is intended as a stepping-stone to a Virtual Soundcard that will be able to receive proprietry network audio formats.

### Building


`make clean && make`

Optionally copy it somewhere sensible, *for example only*:<br/>
`sudo cp snd-vss.ko /lib/modules/5.4.0-80-generic/kernel/sound/drivers/`

### Installing / Removing

*To load the module*

`sudo insmod ./snd-vss.ko`
 
*To try and reload the module during development*

```
systemctl --user stop pulseaudio
sudo alsa force-unload

[do whatever with the module]

sudo alsa force-reload
systemctl --user start pulseaudio
```

*To unload the module (tricky - better to develop on a VM!)*

`sudo rmmod snd_vss` *(will probably fail because snd-pcm,snd depend on it)*

*To find out what's using sound so that you can stop/restart those...*

```
lsof | grep pcm
modinfo -F depends snd-vss.ko
```

### Testing

These commands are useful when checking if the module is loaded and recognised, and for checking naming.<br/>
Here are some example results:

`lsmod | grep snd_vss` :
```
snd_vss               16384  1
snd_pcm               102400  8 snd_hda_intel,snd_oxygen_lib,snd_ice1712,snd_vss
```

`cat /proc/asound/cards` :
```
 0 [STX            ]: AV200 - Xonar STX
                      Asus Virtuoso 100 at 0x4000, irq 16
 1 [M2496          ]: ICE1712 - M Audio Audiophile 24/96
                      M Audio Audiophile 24/96 at 0x5040, irq 19
 2 [HDMI           ]: HDA-Intel - HDA ATI HDMI
                      HDA ATI HDMI at 0x50260000 irq 17
 3 [Source         ]: VIRTUAL - Signal Source
                      Virtual Signal Source (no h/w)

```

`cat /proc/asound/card3/pcm0c/sub0/status` :

*closed*

`cat /proc/asound/card3/pcm0c/sub0/info` :

```
card: 3
device: 0
subdevice: 0
stream: CAPTURE
id: VIRTUAL
name: Signal Source
subname: subdevice #0
class: 0
subclass: 0
subdevices_count: 1
subdevices_avail: 1
```

`arecord -l` :
```
**** List of CAPTURE Hardware Devices ****
card 0: STX [Xonar STX], device 0: Multichannel [Multichannel]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: STX [Xonar STX], device 1: Digital [Digital]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 1: M2496 [M Audio Audiophile 24/96], device 0: ICE1712 multi [ICE1712 multi]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 3: Source [Signal Source], device 0: VIRTUAL [Signal Source]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
```
