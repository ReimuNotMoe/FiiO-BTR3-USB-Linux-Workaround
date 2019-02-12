# FiiO BTR3 USB Linux Workaround
Workaround to get FiiO BTR3's USB mode work in Linux

As of Linux 4.20.5, this device won't work on 44100 sample rate, only 48000 works.


### Pulseaudio howto
For most users, just append these lines to `/etc/pulse/daemon.conf`:

```
 default-sample-rate = 192000
 alternate-sample-rate = 48000
```

Explaination:

Nowdays most built-in sound cards support 192kHz sample rate, so we set `default-sample-rate` to `192000`.

When any of our sound devices doesn't support 192kHz, we want it to fallback to 48kHz (supported since AC97). So we set `alternate-sample-rate` to `48000`.

In this way, pulseaudio will only use either 192kHz or 48kHz. This solves the problem.


### ALSA howto
Configure your software to use 44100 sample rate.

### Appendix
`# cat /proc/asound/BTR3/stream0`

```
FiiO BTR3 at usb-0000:00:14.0-9, full speed : USB Audio

Playback:
  Status: Running
    Interface = 1
    Altset = 1
    Packet Size = 192
    Momentary freq = 48000 Hz (0x30.0000)
  Interface 1
    Altset 1
    Format: S16_LE
    Channels: 2
    Endpoint: 3 OUT (NONE)
    Rates: 48000, 44100
```


`# lsusb -d 0a12:1243 -v`

```
Bus 001 Device 031: ID 0a12:1243 Cambridge Silicon Radio, Ltd 
Device Descriptor:
  bLength                18
  bDescriptorType         1
  bcdUSB               2.00
  bDeviceClass            0 (Defined at Interface level)
  bDeviceSubClass         0 
  bDeviceProtocol         0 
  bMaxPacketSize0        64
  idVendor           0x0a12 Cambridge Silicon Radio, Ltd
  idProduct          0x1243 
  bcdDevice           25.20
  iManufacturer           0 
  iProduct                2 FiiO BTR3
  iSerial                 3 ABCDEF0123456789
  bNumConfigurations      1
  Configuration Descriptor:
    bLength                 9
    bDescriptorType         2
    wTotalLength          141
    bNumInterfaces          3
    bConfigurationValue     1
    iConfiguration          0 
    bmAttributes         0x80
      (Bus Powered)
    MaxPower              500mA
    Interface Descriptor:
      bLength                 9
      bDescriptorType         4
      bInterfaceNumber        0
      bAlternateSetting       0
      bNumEndpoints           0
      bInterfaceClass         1 Audio
      bInterfaceSubClass      1 Control Device
      bInterfaceProtocol      0 
      iInterface              0 
      AudioControl Interface Descriptor:
        bLength                 9
        bDescriptorType        36
        bDescriptorSubtype      1 (HEADER)
        bcdADC               1.00
        wTotalLength           43
        bInCollection           1
        baInterfaceNr( 0)       1
      AudioControl Interface Descriptor:
        bLength                12
        bDescriptorType        36
        bDescriptorSubtype      2 (INPUT_TERMINAL)
        bTerminalID             1
        wTerminalType      0x0101 USB Streaming
        bAssocTerminal          0
        bNrChannels             2
        wChannelConfig     0x0003
          Left Front (L)
          Right Front (R)
        iChannelNames           0 
        iTerminal               0 
      AudioControl Interface Descriptor:
        bLength                13
        bDescriptorType        36
        bDescriptorSubtype      6 (FEATURE_UNIT)
        bUnitID                 2
        bSourceID               1
        bControlSize            2
        bmaControls( 0)      0x01
        bmaControls( 0)      0x00
          Mute Control
        bmaControls( 1)      0x02
        bmaControls( 1)      0x00
          Volume Control
        bmaControls( 2)      0x02
        bmaControls( 2)      0x00
          Volume Control
        iFeature                0 
      AudioControl Interface Descriptor:
        bLength                 9
        bDescriptorType        36
        bDescriptorSubtype      3 (OUTPUT_TERMINAL)
        bTerminalID             3
        wTerminalType      0x0301 Speaker
        bAssocTerminal          0
        bSourceID               2
        iTerminal               0 
    Interface Descriptor:
      bLength                 9
      bDescriptorType         4
      bInterfaceNumber        1
      bAlternateSetting       0
      bNumEndpoints           0
      bInterfaceClass         1 Audio
      bInterfaceSubClass      2 Streaming
      bInterfaceProtocol      0 
      iInterface              0 
    Interface Descriptor:
      bLength                 9
      bDescriptorType         4
      bInterfaceNumber        1
      bAlternateSetting       1
      bNumEndpoints           1
      bInterfaceClass         1 Audio
      bInterfaceSubClass      2 Streaming
      bInterfaceProtocol      0 
      iInterface              0 
      AudioStreaming Interface Descriptor:
        bLength                 7
        bDescriptorType        36
        bDescriptorSubtype      1 (AS_GENERAL)
        bTerminalLink           1
        bDelay                  0 frames
        wFormatTag              1 PCM
      AudioStreaming Interface Descriptor:
        bLength                14
        bDescriptorType        36
        bDescriptorSubtype      2 (FORMAT_TYPE)
        bFormatType             1 (FORMAT_TYPE_I)
        bNrChannels             2
        bSubframeSize           2
        bBitResolution         16
        bSamFreqType            2 Discrete
        tSamFreq[ 0]        48000
        tSamFreq[ 1]        44100
        AudioControl Endpoint Descriptor:
          bLength                 7
          bDescriptorType        37
          bDescriptorSubtype      1 (EP_GENERAL)
          bmAttributes         0x81
            Sampling Frequency
            MaxPacketsOnly
          bLockDelayUnits         2 Decoded PCM samples
          wLockDelay              0 Decoded PCM samples
      Endpoint Descriptor:
        bLength                 9
        bDescriptorType         5
        bEndpointAddress     0x03  EP 3 OUT
        bmAttributes            1
          Transfer Type            Isochronous
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x00c0  1x 192 bytes
        bInterval               1
        bRefresh                0
        bSynchAddress           0
    Interface Descriptor:
      bLength                 9
      bDescriptorType         4
      bInterfaceNumber        2
      bAlternateSetting       0
      bNumEndpoints           1
      bInterfaceClass         3 Human Interface Device
      bInterfaceSubClass      0 No Subclass
      bInterfaceProtocol      0 None
      iInterface              0 
        HID Device Descriptor:
          bLength                 9
          bDescriptorType        33
          bcdHID               1.11
          bCountryCode            0 Not supported
          bNumDescriptors         1
          bDescriptorType        34 Report
          wDescriptorLength      69
         Report Descriptors: 
           ** UNAVAILABLE **
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x81  EP 1 IN
        bmAttributes            3
          Transfer Type            Interrupt
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0010  1x 16 bytes
        bInterval               1
Device Status:     0x0000
  (Bus Powered)
```
