ALSA Virtual Soundcard Signal Generator/Source
==============================================

Based on the 'minivosc' project [here](http://www.alsa-project.org/main/index.php/Minivosc).

This is an extension of the above that can provide sine-wave sources at various sample-rates and in various formats.
It is intended as a stepping-stone to a Virtual Soundcard that will be able to receive proprietry network audio formats.

### Building

- make clean && make

### Installing / Removing

*To load the module*

- sudo insmod ./snd-vsiggen.ko
 
*To reload the module during development*

- pulseaudio -k
- sudo alsa force-reload
- pulseaudio -D

*To unload the module (maybe)*

- sudo rmmod snd_vsiggen

### Testing

- lsmod | grep vsiggen
- arecord -l
- cat /proc/asound/cards