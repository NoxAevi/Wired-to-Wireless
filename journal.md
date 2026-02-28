# 2/27/26

### TIME: 1h

**Man, don't you just hate how so many devices are wired instead of wireless? Well, I sure do.**

The idea is simple in concept, have two separate modules: one for acting as the host and one as the dongle while keeping optional BT LE support.

For the most part this session was just fleshing out the idea to the features that I want to include. After I created a general map, I did quite a bit of research on decent 2.4GHz+BT LE MCUs, and I found that the nRF52840 would work the best. However, there needs to be a USB host controller because as far as I know, the nRF52840 can only act as a USB device.

<img width="765" height="713" alt="image" src="https://github.com/user-attachments/assets/3a91d6bb-35e5-4b2c-9949-a1f43d2ddd46" />

