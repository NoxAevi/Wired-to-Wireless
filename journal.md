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

# 3/2/26

### 1.2h

I decided a better way to go about things rather than just jumping in, which would be to start from the source of power and work my way outwards from that. This meant I would start from the battery connector and then work my way through the system (where the natural next step would be the PMIC circuit).

Since there was no symbol for the PMIC I'm using in Kicad, I had to make my own one. Learning from yesterday's failure, I made a footprint that looked much nicer (but took quite a bit more time to create)

There were still some parts that I didn't fully understand about the PMIC chip, but I was still able to generally group pins based on their functions.

<img width="577" height="368" alt="image" src="https://github.com/user-attachments/assets/cc0506ca-0adb-43b9-b9fc-f8bf34abf97b" />

Afterwards I got started on the routing part, starting with the power. When I ran into the D- and D+ pins, I got a bit confused on how I would route them to both the MAX host controller and the PMIC while avoiding both of them using the lines at the same time. However, I realized that the battery would only be charging when not in use, and I could just keep the MAX chip off by default. (Though I would still have to find a MUX)

<img width="976" height="709" alt="image" src="https://github.com/user-attachments/assets/ad48248c-9d76-47b8-9b0e-3798007aee96" />


# 3/3/26

### 1h

Started off this session with finding a USB mux as I said I would yesterday, and I settled on the FSUSB42

Luckily, this datasheet was ***so much easier to read*** so I was able to integrate it into what I previous had without any difficulties.

<img width="442" height="393" alt="image" src="https://github.com/user-attachments/assets/ab54d715-cf22-4c23-9f11-6d536d012f54" />

As usual, the TI PMIC was still pretty confusing, but since I've read it multiple times through at this point, I feel like it's a lot more understandable. What took me the most time to figure out while routing the power for this chip was whether the VBUS pin can both be used as an input (for charging the battery) and as an output (at 5V to supply the device)

Anyways, I was able to completely finish routing the power portion of the PMIC, and I'll be able to start routing the nRF chip soon (which would also be when I route the I2C and the rest of the pins for the PMIC that relate to CTR)

<img width="636" height="612" alt="image" src="https://github.com/user-attachments/assets/a0faa2eb-44fe-42a2-b2ad-9151c26d1c3a" />

Finally, I sourced all the components that I used on the JLC parts library (making sure to use basic/extended preferred to avoid the loading fee)

<img width="268" height="200" alt="image" src="https://github.com/user-attachments/assets/9094d7ce-3a02-4469-b296-0c9402cbbe0b" />

# 3/4/26

### 1.1h

I started with importing the nRF52840 chip (which had a given footprint and symbol), and then I proceeded to look at the documentation for the nRF52 chip. What surprised me however was how there was 600+ pages of documentation all for this one chip (I only read the description before). Most of it seems to just be on the firmware implementation for the chip, luckily (for now).

What I did find surprising though was how Nordic Semicon gives a full copy of the PCB implementation, with all the values for external components neatly in one place (which should make making the schematic a breeze, I hope)

<img width="1115" height="806" alt="image" src="https://github.com/user-attachments/assets/53bb78ea-1d3a-4563-b090-6e7db4b9fc7f" />

However, I then realized that the different packages for the nRF52840 have different numbers of pins. Unfortunately, the footprint for the nRF52840 was a different one than the one I would be using (QFN for ease of routing). Additionally, the pin numbers were wrong, which meant that I wouldn't really be able to use it and the QFN footprint together. Thus, I decided that I would just have to make my own symbol.

With the rest of the time in this session, I started adding all the pins (and their corresponding pin numbers) as well as double checking them to make sure they were correct and then sorting them based on function. At this point, I realized that I forgot to double check the pins on the PMIC symbol I made, so I quickly did that as well this session.

<img width="862" height="633" alt="image" src="https://github.com/user-attachments/assets/e2584047-a0ce-49b1-add1-0e0bfb08dd01" />


