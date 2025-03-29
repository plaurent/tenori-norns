# Tenori-ON Grid Mode <-> midigrid Reference

`tenori-norns` requires pika.blue's Tenori-ON firmware A042 (available from [https://pika.blue](https://pika.blue)).

The A042 firmware introduces a new "Grid" layer mode on Tenori-ON, designed to interact with Norns via `midigrid`.

Below are some reference notes on the Grid layer mode <-> `midigrid` protocol.

## Norns-->TNR (Init)

Norns can send an init message to activate Grid mode on the Tenori-ON. A reasonable init
message is is `0xB0 0x55 0x47` which sets Layer 1 (layer 0) to grid, sets its output midi channel to 1, and makes it visible.

Details:

* `bit 1` = Set layer (1) to Grid type
* `bit 2` = Force the midi channel on this layer to the same as the layer number
* `bit 3` = Make this layer visible (i.e. change layer to this one)
* `bit 4-5` = unused
* `bit 6` = 1 : norns-->TNR configuration message

With no bits set, the message is interpreted as "clear LEDs"

## TNR-->Norns (Init Acknowledgement)

After init (or at other times), the Tenori-ON can send back `0xB0 0x55 0xYZ`, with `Y` being 2 and `Z` being a combination of bits, like this:

* `0x21`: This outbound channel (i.e. channel 1, if message was B0) is now grid, so please expect to receive grid messages on this channel.
* `0x22` : This outbound channel has matching layer number, but isn't grid (I don't think you'll ever actually see this), so it's OK to send on this channel
* `0x23` : This channel is both grid and has matching layer number.
* `0x20` : This channel was grid, but isn't any more.
  
So communications from Norns should proceed once Norns gets a 0xB0 55 23, but be suppress of it ever sees 0xB0 55 2[0-2]

## Norns-->TNR (LEDs)

* For the first 128 LEDs, Note On (`0x90`) is used.
* For the next 128 LEDs, Pressure (`0xA0`) is used.

## TNR-->Norns (Special Keys for K1, K2, K3)

We use bit6 = 0 to tell the Norns what extra buttons have been pressed. 
Clear and L1-L5 don't do anything on the TNR in grid controller mode.

* `0xB0 55 10` : Clear button pressed
* `0xB0 55 00` : Clear button released
* `0xB0 55 16` : L1 button pressed
* `0xB0 55 06` : L1 button released
* `0xB0 55 17` : L2 button pressed
* `0xB0 55 07` : L2 button released
* ...
* `0xB0 55 1A` : L5 button pressed
* `0xB0 55 0A` : L5 button released
