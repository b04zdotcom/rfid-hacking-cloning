# RFID/NFC Hacking
Instructions for hacking and cloning RFID cards.

## PN532 card cloning
![PN532 with USB to TTL](https://github.com/nfc-tools/libnfc/assets/4102106/56ae6814-fbef-48c0-a550-48b8ad139402)
With the PN532 rfid/nfc module connected to a usb to ttl converter, you can scan and clone rfid cards.

### Connecting the PN532 to the USB/TTL converter
1. PN532 SCL to USB TX
2. PN532 SDA to USB RX
3. PN532 VCC to USB 5.0V
4. PN532 GND to USB GND

Set the onboard switch to the 0-0 position.

## libnfc
Install the needed packages
```
sudo apt install libnfc-bin libnfc-dev libnfc5 libnfc-examples mfoc
```

### List
Hold the card to the module and run `nfc-list`

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