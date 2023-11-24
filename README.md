<a id="Top"></a>

# Table of Contents
- [Disclaimer](#disclaimer)
- [Introduction to RFID Access Control Systems](#introduction-to-rfid-access-control-systems)
  - [Frequencies](#frequencies)
  - [Passive RFID Cards](#passive-rfid-cards)
  - [Unique Identifier](#unique-identifier)
  - [RFID Protocols](#rfid-protocols)
  - [Reader/Controller Communication Protocols](#readercontroller-communication-protocols)
- [PN532 card cloning](#pn532-card-cloning)
  - [What you need](#what-you-need)
  - [Connecting the PN532 to the USB/TTL converter](#connecting-the-pn532-to-the-usbttl-converter)
- [libnfc](#libnfc)
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

# PN532 card cloning
^ [Back to top](#top)

![PN532 with USB to TTL](https://github.com/nfc-tools/libnfc/assets/4102106/56ae6814-fbef-48c0-a550-48b8ad139402)
With the PN532 RFID module connected to a usb to ttl converter, you can scan and clone High Frequency RFID cards.

## What you need
1. PN532 module [AliExpress](https://www.aliexpress.com/item/32848242166.html)
2. USB to TTL converter [AliExpress](https://www.aliexpress.com/item/32345829369.html)

## Connecting the PN532 to the USB/TTL converter
1. PN532 SCL to USB TX
2. PN532 SDA to USB RX
3. PN532 VCC to USB 5.0V
4. PN532 GND to USB GND

Set the module to UART mode by setting the onboard switch to the 0-0 position.

# libnfc
^ [Back to top](#top)

`libnfc` is an open-source software library and a set of utilities that facilitates communication with Near Field Communication (NFC) and Radio-Frequency Identification (RFID) devices. It supports a wide range of NFC/RFID hardware, and can be used on Linux, Windows, and macOS.

Install the following packages.
```
sudo apt install libnfc-bin libnfc-dev libnfc5 libnfc-examples mfoc
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