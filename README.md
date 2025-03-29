# tenori-norns

`tenori-norns` lets you connect Tenori-ON to Norns via the `midigrid` project [[community page](https://norns.community/midigrid)] [[source](https://github.com/jaggednz/midigrid)].  It requires a Tenori-ON running [pika.blue's firmware A042 or later](https://www.pika.blue/posts/tenori-on/releases/).

* The Tenori-ON acts as a 16x16 grid with 256 available buttons.  
* The Tenori-ON L1, L2, and L3 buttons are mapped to K1, K2 and K3 on the Norns.

## Usage

The Tenori-ON uses Layer 1 for the grid.

The easiest way to proceed is:
1. Follow Installation instructions (next section)
1. Start with Norns off.
1. Connect Norns to Tenori-on via your USB MIDI Interface.
1. Make sure Tenori-ON is on (running [pika.blue's firmware A042 or later](https://www.pika.blue/posts/tenori-on/releases/)).
1. Turn on Norns.

### Usage Notes 

If Norns is already on before you connect it to the Tenori-ON, plugging 
it in may result in LED signals from Norns+midigrid being misinterpreted as MIDI notes on the Tenori-ON.

Also it is recommended that you disable the Tenori-ON screensaver mode to prevent the LED signals from the Norns+midigrid being misinterpreted as MIDI notes when the screensaver activates.

## Installation

1. Use Maiden to install `midigrid` on your Norns by browsing to the IP address of Norns.
1. Add `tenori-on.lua` from this repository under `code/midigrid/lib/devices/`
1. Edit `code/midigrid/lib/midigrid.lua` around line 50, change `midigrid:init('64')` to `midigrid:init('256')`
    * Note: It may be possible to configure this via Midigrid's Norns settings UI, but I haven't yet successfully done this. 
1. Edit `code/midigrid/lib/supported_devices.lua` to add a `supported device` entry for the Tenori-ON via your USB Midi Interface:
    1. In my case, when I plug in my MIDI interface, in Maiden's `matron` debug window, I saw `device_monitor(): adding midi device USB2.0 Hub
`, so I added the following line (see arrow):
```
local supported_devices = {
  midi_devices = {
      ...
          { midi_base_name= 'block 1',        device_type='livid_block'  },
          { midi_base_name= 'usb2.0 hub',     device_type='tenori-on'    }, -- <---
          { midi_base_name= 'launchpad',      device_type='launchpad'    },
      ...
``` 

That's it! You're ready to go.


