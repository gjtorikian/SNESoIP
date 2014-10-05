# SNESoIP: The SNES ethernet adapter #

![SNESoIP prototype](hardware/images/rev02-small.jpg?raw=true)

For images in higher resolution click [here](hardware/images/).


## Introduction ##

The SNESoIP (SNES over IP) is an ethernet adapter for the Super Nintendo
Entertainment System which is also known as Super NES, SNES or Super
Nintendo. It is basically an open-source, proof-of-concept,
network-bridge for sending and receiving data over the Internet. This
data can be generated by another SNESoIP's controller input, homebrew
software, a virtual client or a custom service. Virtual services are
basically virtual clients with a special purpose: e.g. bulletin board
system, custom homebrew game server, you name it!

If you ever wanted to hook-up your childhood to the Internet without
spreading all of your embarrassing pictures from the 1990s on Facebook,
you definitely came to the right place.

But be aware that the project is still in an early testing/development
stage and I'm currently not planning to build and/or sell these devices
in a larger quantity – mostly due to the legal situation in Germany. So,
if you want one, you have to build one by yourself or at least wait
until someone decides to produce and sell them professionally.

I also highly recommend to use this project only in combination with a
flash cartridge like the [sd2snes](http://sd2snes.de/blog/) or the
[Super Everdrive](http://krikzz.com/index.php?route=product/product&product_id=51).

The SNESoIP is an open-source project developed completely in my spare
time. If you find it useful, please consider
[a small tip](https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=ESZJS7TMYMNNW&lc=GB&item_name=mupfelofen%2ede&item_number=SNESoIP&no_note=1&no_shipping=1&currency_code=USD&bn=PP%2dDonationsBF%3abtn_donateCC_LG%2egif%3aNonHosted). Your
donation will be used to support the further development of the project.

The easiest way to donate is via PayPal, simply click
[here](https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=ESZJS7TMYMNNW&lc=GB&item_name=mupfelofen%2ede&item_number=SNESoIP&no_note=1&no_shipping=1&currency_code=USD&bn=PP%2dDonationsBF%3abtn_donateCC_LG%2egif%3aNonHosted).

If you want to help with the further development of this project, join
us in
[#retrotardation](http://de.irc2go.com/webchat/?net=euIRC&room=retrotardation)
on [euIRC](http://www.euirc.net/en/).

And feel free to contribute to our
[development wiki](https://github.com/mupfelofen-de/SNESoIP/wiki). Any
help is appreciated!


## Hardware ##

### Assembly ###

Here are some instructions on how to assemble the current rev. 02 hardware:

#### External components ####

The orientation of the [ENC28J60 module](hardware/rev02/docs/enc28j60-module.jpg):

![Assembly 1](hardware/images/instructions/assembly-rev02-01-small.jpg?raw=true)

The pinout on the PCB for input, port0, port1 is in the same order
(ascending):
```
VCC, GND, Clock, Latch, Data
```
Pin 1 is marked with a tiny 1 on the silk-layer.
The actual pinout for the controller plug is as follows:

![SNES controller pinout](hardware/images/instructions/snes-controller-pinout.png?raw=true)

![Assembly 2](hardware/images/instructions/assembly-rev02-02-small.jpg?raw=true)

The [last-minute modification](#the-last-minute-modification):

![Assembly 3](hardware/images/instructions/assembly-rev02-03-small.jpg?raw=true)

For images in higher resolution click
[here](hardware/images/instructions/).

The assembly of the self-explanatory, bipolar components is not
illustrated.

### The last-minute modification ###

_After_ I finished soldering the first 50 PCBs of the rev. 02 hardware,
we stumbled over a software register called WRIO, the programmable I/O
port (out-port), that was rarely used by Nintendo. This register can be
used to flip a single bit on each controller port using the (usually)
unconnected pin 6. After doing some field research, we realised that we
could adapt this feature to build the missing bridge in our yet pretty
experimental networking concept. In a word: we found a way to send data
directly from a homebrew application to the SNESoIP and establish a
_full_ two-way connection. This is awesome for obvious reasons!

But because the PCBs were already produced, we had to find a way how to
use this feature on our existing hardware. And so we did! After a short
period of time, we found a pretty reasonable solution:

The SNESoIP uses the SNES controller ports as a power supply and because
VCC and GND are connected on both sides, we could spare at least one pin
on every cable.  So we opened the two connectors and pulled out GND
(port0 cable) and VCC (on the port1 cable) and switched them with the
previously mentioned pin 6.

We then connected these pins to the yet unused GPIO port (which was
added to the PCB layout with nothing in mind back then). To be precise:
pin 6 (port0, previously VCC) to PB1 and pin 6 (port1, previously GND)
to PB0 (for mechanical reasons).

With this solution we managed to use both programmable output ports and
keep both input ports usable as well, without soldering or changing the
actual PCB layout.

Because these connectors aren't designed to be reopened again, I made
some simple instructions how they can be modified to fit the
requirements:

#### Connector modification instructions ####

Make sure that you're cutting the retention hooks on the correct side of
the connectors:

![Connector modification 1](hardware/images/instructions/connector-modification-01-small.jpg?raw=true)

![Connector modification 2](hardware/images/instructions/connector-modification-02-small.jpg?raw=true)

Now use a pair of pliers to push in the retention hooks on the opposite
side of the plug:

![Connector modification 3](hardware/images/instructions/connector-modification-03-small.jpg?raw=true)

Then push the plug out of its shell:

![Connector modification 4](hardware/images/instructions/connector-modification-04-small.jpg?raw=true)

![Connector modification 5](hardware/images/instructions/connector-modification-05-small.jpg?raw=true)

![Connector modification 6](hardware/images/instructions/connector-modification-06-small.jpg?raw=true)

Original pin assignment:

![Connector modification 7](hardware/images/instructions/connector-modification-07-small.jpg?raw=true)

Modified cable for port0:

![Connector modification 8](hardware/images/instructions/connector-modification-08-small.jpg?raw=true)

Modified cable for port1:

![Connector modification 9](hardware/images/instructions/connector-modification-09-small.jpg?raw=true)

For images in higher resolution click [here](hardware/images/instructions/).


## How it works ##

This is magic.
Don't even try to understand it, unless your last name is Dumbledore.


## Authors of the SNESoIP project ##

The following contributions warranted legal paper exchanges with Michael
Fitzmayer:

### Core developers ###

[Daniel Baumann](mailto:/sciurus@blastprocessing.de): The author of the
web interface and the java adaption of the login server.

[saturnu](http://jensma.de/rehkopf/gallery-images/saturnu.gif), the
fabulous: He's most likely the only person who understands the project
as a whole. He also develops homebrew software, wrote a login server and
a virtual userspace device, etc.

[Michael Fitzmayer](mailto:/mail@michael-fitzmayer.de): Hard- and
firmware stuff, project founder.

### Additional credits ###

[Daniel Otte](mailto:/daniel.otte@rub.de): The author of the
[AVR-Crypto-Lib](https://www.das-labor.org/wiki/AVR-Crypto-Lib/en).

[Guido Socher](mailto:/guidosocher@fastmail.fm): The author of the
[tuxgraphic TCP/IP stack](http://tuxgraphics.org/common/src2/article09051/
"The tuxgraphics TCP/IP stack") used in the firmware.

[Jan Schmitz](mailto:/pw_returner@web.de): Early code review and
improvements.

## Special thanks goes to ##

- The guys from
  [#retrotardation](http://de.irc2go.com/webchat/?net=euIRC&room=retrotardation)
  on [euIRC](http://www.euirc.net/en/) for all the support and
  amusement,
- [lytron](http://pantalytron.com) for testing the initial prototype,
- Farbauti for his code improvements,
- the [snesfreaks.com](http://snesfreaks.com) community for all the
  support and motivation and
- last but not least, to my mates at our local hackerspace,
  [shackspace](http://shackspace.de).  You guys rock!


## Licenses ##

The project has been released under the terms of a BSD-like license.
See the file [LICENSE](LICENSE) for details.

The
[tuxgraphic TCP/IP stack](http://tuxgraphics.org/common/src2/article09051/
"The tuxgraphics TCP/IP stack") is under the terms of
[The MIT License (MIT)](http://www.opensource.org/licenses/mit-license.html).

For license information of the
[AVR-Crypto-Lib](https://www.das-labor.org/wiki/AVR-Crypto-Lib/en) see
the [GNU General Public License](http://www.gnu.org/licenses/).

"Nintendo" is a registered trademark of Nintendo of America Inc.
