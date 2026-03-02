# 2/27/26

### TIME: 1h

**Man, don't you just hate how so many devices are wired instead of wireless? Well, I sure do.**

The idea is simple in concept, have two separate modules: one for acting as the host and one as the dongle while keeping optional BT LE support.

For the most part this session was just fleshing out the idea to the features that I want to include. After I created a general map, I did quite a bit of research on decent 2.4GHz+BT LE MCUs, and I found that the nRF52840 would work the best. However, there needs to be a USB host controller because as far as I know, the nRF52840 can only act as a USB device.

<img width="765" height="713" alt="image" src="https://github.com/user-attachments/assets/3a91d6bb-35e5-4b2c-9949-a1f43d2ddd46" />

# 2/28/26

### TIME: 1.2h

So... I may have wanted to include both a USB C and USB A receptacle on the receiver, but I decided that [this](https://www.youtube.com/watch?v=Fj0XuYiE7HU) would be against making the device user friendly by suggesting that both ports would be able to work at once (which wouldn't be the case). I decided to just go with USBC because USB-C to A dongles aren't too uncommon.

Following this, I watched this video to learn a bit about battery management circuits and what I need to create something safe. I stumbled upon a TI PMIC, and then skimmed through the datasheet

<img width="722" height="398" alt="image" src="https://github.com/user-attachments/assets/f6bf59b1-8655-481a-8e2e-7ab5aad05650" />

(Also found a suitable USB host)


# 3/1/26

### TIME: 1.4h

Obviously the first thing to do for this session would be to finish up what I was working on yesterday, which was sourcing a PMIC for the project. After reading the documentation for BQ25611D, I looked at other options on the JLCPCB parts library, and most of the documentation there was pretty mid compared to TI. 

Then, I started gathering the part numbers for the parts I'd be using

And finally, finally started the schematic

The only problem was I couldn't really find a premade symbol and footprint for the MAX3421E, so I decided I'd make my own. Since I hadn't made anything PCB related since summer, I was quite rusty. I literally put the pins inside out, then painstakingly flipped each of them, only to forget that making pins that aren't needed (like the thermal pad underneath the chip) invisible was a feature. This resulted in a symbol that looks pretty horrendous 



<img width="429" height="411" alt="image" src="https://github.com/user-attachments/assets/53cbe568-47fa-4e93-b277-bb2ce8d16c56" />


I finally decided to search online, and lo and behold, the [first result](https://www.snapeda.com/) was the footprint and symbol I needed. 

Well lesson learned for next time... I guess


P.S. Just decided this after posting this journal entry. I guess I'll practice making symbols since it's good for experience, but I'll 100% start from scratch to avoid this mess
