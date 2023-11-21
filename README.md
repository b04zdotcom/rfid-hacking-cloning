# Disclaimer
This guide is for educational purposes only. The information provided is not intended for any illegal or unauthorized activities. Users are responsible for complying with local laws and regulations. The author and contributors are not liable for any misuse or legal consequences resulting from the reader's actions. Obtain necessary permissions before engaging in RFID-related activities.

# RFID/NFC Hacking
## Introduction
Low Frequency (LF) and High Frequency (HF) are two common frequency bands used in RFID access control systems. LF systems typically operate in the frequency range of 125 kHz to 134 kHz, while HF systems operate at 13.56 MHz. The Unique Identifier (UID) of the RFID tag or card is commonly used for authentication. The UID is a unique code assigned to each RFID tag during the manufacturing process.

### Cards
In order to clone the UID, you need cards with a changeable UID. For LF systems, you need a card with a T5577 chip. HF systems often use the Mifare Classic 1k or 4k cards. HF cards with a changeable UID are often called "Magic Cards".

## PN532 card cloning
![PN532 with USB to TTL](https://github.com/nfc-tools/libnfc/assets/4102106/56ae6814-fbef-48c0-a550-48b8ad139402)
With the PN532 rfid/nfc module connected to a usb to ttl converter, you can scan and clone rfid cards.

### What you need
1. PN532 module [AliExpress](https://www.aliexpress.com/item/32848242166.html)
2. USB to TTL converter [AliExpress](https://www.aliexpress.com/item/32345829369.html)

### Connecting the PN532 to the USB/TTL converter
1. PN532 SCL to USB TX
2. PN532 SDA to USB RX
3. PN532 VCC to USB 5.0V
4. PN532 GND to USB GND

Set the module to UART mode by setting the onboard switch to the 0-0 position.

## libnfc
Install the needed packages
```
sudo apt install libnfc-bin libnfc-dev libnfc5 libnfc-examples mfoc
```

### List
Hold the card to the module and run
```
nfc-list
```

### Read and save
Hold the card you want to clone to the module and run this command to save the contents of the card to a file. In this case `dump1.mfd`.
```
mfoc -O dump1.mfd
```

### Write
Hold the writable card or tag to the module and run this command to write the contents saved in `dump1.mfd` to the new card. Use a capital `W` if you also want to overwrite the UID. Use a lowercase `w` if you want the card to keep it's original UID.
```
nfc-mfclassic W a u dump1.mfd
```