<a id="Top"></a>

# Table of Contents
- [Disclaimer](#disclaimer)
- [Introduction to RFID Access Control Systems](#introduction-to-rfid-access-control-systems)
  - [Frequencies](#frequencies)
  - [Passive RFID Cards](#passive-rfid-cards)
  - [Unique Identifier](#unique-identifier)
  - [RFID Protocols](#rfid-protocols)
  - [Reader/Controller Communication Protocols](#readercontroller-communication-protocols)
- [RFID Devices](#rfid-devices)
  - [Proxmark3](#proxmark3)
    - [Product list](#product-list)
    - [Installation](#installation)
  - [PN532](#pn532)
    - [Product list](#product-list-1)
    - [Connecting the PN532 to the USB/TTL converter](#connecting-the-pn532-to-the-usbttl-converter)
  - [Chinese LF RFID cloner](#chinese-lf-rfid-cloner)
    - [Product list](#product-list-2)
- [libnfc library](#libnfc-library)
  - [Commands](#commands)
    - [List](#list)
    - [Read and save (HF)](#read-and-save-hf)
    - [Write (HF)](#write-hf)

# Disclaimer
^ [Back to top](#top)

This guide is for educational purposes only. The information provided is not intended for any illegal or unauthorized activities. Users are responsible for complying with local laws and regulations. The author and contributors are not liable for any misuse or legal consequences resulting from the reader's actions. Obtain necessary permissions before engaging in RFID-related activities.

# Introduction to RFID Access Control Systems
^ [Back to top](#top)

## Frequencies
Low Frequency (LF) and High Frequency (HF) are two common frequency bands used in RFID access control systems. LF systems typically operate in the frequency range of 125 kHz to 134 kHz, while HF systems operate at 13.56 MHz.

## Passive RFID Cards
Passive LF and HF RFID cards do not have an internal power source. When near an RFID reader, the reader inductively couples and induces power in the card's coil, activating the chip. The chip is then able to communicate with the reader by modulating its power consumption.

## Unique Identifier
The Unique Identifier (UID) of the RFID card is used for identification and often also for authentication and authorization. The UID is a unique code assigned to each RFID card during the manufacturing process. In order to clone the UID, you need cards with a changeable UID. For LF systems, you need a card with a `T5577` chip. HF systems often use `MIFARE Classic` cards. HF cards with a changeable UID are often called `Magic Cards`.

## RFID Protocols
There are many protocols with specifications and protocols that define how the RFID system operates. Here we will just focus on the `EM4100` standard for Low Frequency and the `MIFARE` standard for High Frequency.

Other common RFID protocols for access control include MIFARE Ultralight, HID Prox, iCLASS, and DESFire. Each of these protocols may vary in terms of frequency and security features, catering to different requirements in access control applications.

## Reader/Controller Communication Protocols
The communication protocol between the reader and the controller is a separate consideration and is generally determined by the design and specifications of the reader and its integration with the access control system.

The `Wiegand Protocol` is one of the most common communication protocols used in access control systems, due to its simplicity and reliability. It is a one-way binary communication protocol, where data is transmitted over multiple wires as sequences of 0s and 1s to convey information from RFID card readers to access control panels.

A more secure alternative to the traditional Wiegand protocol, is OSDP (Open Supervised Device Protocol). OSDP is considered safer than Wiegand due to features like encryption preventing eavesdropping, bidirectional communication for advanced security, tamper detection, and modern security measures. While newer and more secure protocols are gaining popularity, you will often encounter access control systems that still utilize the Wiegand protocol.

# RFID Devices
^ [Back to top](#top)

## Proxmark3
![proxmark3](https://github.com/b04zdotcom/rfid-hacking-cloning/assets/4102106/3f51b061-1993-4954-b542-3ab1d604f2af)
The Proxmark3 is an open-source RFID module used for LF/HF card reading and cloning, emulating cards and RFID sniffing.

### Product list
You can get a Chinese Proxmark3 Easy for $35 - $55 USD on AliExpress.
- Proxmark3 Easy [AliExpress](https://www.aliexpress.us/item/1005005209158934.html)

### Installation
Install the `Iceman` client on your computer and the firmware on your Proxmark3 device. Go to the [Proxmark3 Iceman Fork repository](https://github.com/RfidResearchGroup/proxmark3/tree/master#proxmark3-installation-and-overview) and follow the installation instructions for your operating system.

If you are using the Chinese Proxmark3 Easy listed above, compile the firmware using the `PLATFORM=PM3GENERIC` platform parameter in your `Makefile.platform` file.

You can copy the Makefile.platform sample using the following command.
```
cp Makefile.platform.sample Makefile.platform
```

## PN532
![PN532 with USB to TTL](https://github.com/b04zdotcom/rfid-hacking-cloning/assets/4102106/597f6508-e4bd-44d0-b92f-370498a70530)

With the PN532 RFID module connected to a usb to ttl converter, you can scan and clone High Frequency RFID cards using the [libnfc library](#libnfc-library).

### Product list
- PN532 module [AliExpress](https://www.aliexpress.com/item/32848242166.html)
- USB to TTL converter [AliExpress](https://www.aliexpress.com/item/32345829369.html)

### Connecting the PN532 to the USB/TTL converter
1. PN532 SCL to USB TX
2. PN532 SDA to USB RX
3. PN532 VCC to USB 5.0V
4. PN532 GND to USB GND

Set the module to UART mode by setting the onboard switch to the 0-0 position.

## Chinese LF RFID cloner
![RFID-cloner](https://github.com/b04zdotcom/rfid-hacking-cloning/assets/4102106/c4bb2576-ac8a-400a-a6c6-e0653f0f377e)

This is the easiest and cheapest way to get started cloning Low Frequency RFID cards and tags. Just hold the card you want to clone in front of the device and press "READ". Next, hold your LF card with changeable UID in front of the device and press "WRITE".

### Product list
- LF RFID cloner [AliExpress](https://www.aliexpress.com/item/1005005316016488.html)

# libnfc library
^ [Back to top](#top)

`libnfc` is an open-source software library and a set of utilities that facilitates communication with Near Field Communication (NFC) and Radio-Frequency Identification (RFID) devices. It supports a wide range of NFC/RFID hardware, and can be used on Linux, Windows, and macOS.

Install the following packages.
```
sudo apt install libnfc-bin libnfc-dev mfoc
```

## Commands

### List
`nfc-list` Lists available NFC/RFID devices and provides information about nearby NFC/RFID cards.

Hold a card to the module and run the following command.
```
nfc-list
```

### Read and save (HF)
`mfoc` Stands for "MIFARE Classic Offline Cracker." It's a tool for recovering keys from MIFARE Classic cards.

Hold the card you want to clone to the module and run this command to save the contents of the card to a file. In this case `dump1.mfd`.
```
mfoc -O dump1.mfd
```

### Write (HF)
`nfc-mfclassic` is a utility for reading and writing HF MIFARE Classic cards.

Hold a writable card to the module and run this command to write the contents saved in `dump1.mfd` to the new card. Use a capital `W` if you also want to overwrite the UID. Use a lowercase `w` if you want the card to keep it's original UID.
```
nfc-mfclassic W a u dump1.mfd
```