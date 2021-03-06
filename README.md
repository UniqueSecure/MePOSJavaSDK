# MePOS Connect Java SDK version 1.4

The MePOS Connect Java SDK is designed to allow communication from a desktop envriroment to the MePOS host unit.

This document is a reference on how to integrate to the MePOS unit into your own java application. This document
does not include information on how to set up the MePOS unit, for this please refer to the documentation that came with
your MePOS unit.

## Contents
- [Supported platforms](#supported-platforms)
- [Supported MePOS firmware](#supported-mepos-firmware)
- [Use of the MePOS Connect SDK on Java](#use-of-the-mepos-connect-sdk-on-java)
  - [Libraries](#libraries)
  - [Add the SDK to your project](#add-the-sdk-to-your-project)
  - [Creating a new MePOS object](#creating-a-new-mepos-object)
  - [Adding log levels](#adding-log-levels)
- [MePOS SDK Methods](#mepos-sdk-methods)
  - [int setDiagnosticLed(int position, int colour)](#int-setdiagnosticledint-position-int-colour)
  - [int setLedOneCol(Integer colour), int setLedTwoCol(Integer colour), int setLedThreeCol(Integer colour)](#int-setledonecolinteger-colour-int-setledtwocolinteger-colour-int-setledthreecolinteger-colour)
  - [int setCosmeticLedCol(Integer colour)](#int-setcosmeticledcolinteger-colour)
  - [boolean printerBusy()](#boolean-printerbusy)
  - [boolean loadImage(InputStream stream) throws MePOSException](#boolean-loadimageinputstream-stream-throws-meposexception)
  - [boolean loadImage(Path path) throws MePOSException](#boolean-loadimagepath-path-throws-meposexception)
  - [boolean loadImage(BufferedImage image) throws MePOSException](#boolean-loadimagebufferedimage-image-throws-meposexception)
  - [int print(MePOSReceipt receipt)](#int-printmeposreceipt-receipt)
  - [int print(MePOSReceipt receipt, MePOSPrinterCallback callback)](#int-printmeposreceipt-receipt-meposprintercallback-callback)
  - [int printRAW(String command)](#meposreceipt-r--new-meposreceipt)
  - [int serialRAW(String command)](#int-printrawstring-command)
  - [int cashDrawerStatus() throws MePOSException](#int-cashdrawerstatus-throws-meposexception)
  - [boolean openCashDrawer(boolean validateCashDrawerStatus) throws MePOSException](#boolean-opencashdrawer-throws-meposexception)
  - [boolean openCashDrawer() throws MePOSException](#boolean-opencashdrawer-throws-meposexception)
  - [void enableUSB() throws MePOSException](#void-enableusb-throws-meposexception)
  - [void disableUSB() throws MePOSException](#void-disableusb-throws-meposexception)
  - [void enableWifi() throws MePOSException](#void-enablewifi-throws-meposexception)
  - [void disableWifi() throws MePOSException](#void-disablewifi-throws-meposexception)
  - [void enableCosmeticLEDButton() throws MePOSException](#void-enablecosmeticledbutton-throws-meposexception)
  - [void disableCosmeticLEDButton() throws MePOSException](#void-disablecosmeticledbutton-throws-meposexception)
  - [String getFWVersion()](#string-getfwversion)
  - [String getSerialNumber()](#string-getserialnumber)
  - [MePOSConnectionManager getConnectionManager()](#meposconnectionmanager)
- [MePOSConnectionManager](#meposconnectionmanager)
  - [int getConnectionStatus()](#int-getconnectionstatus)
  - [terminateCommunication()](#terminatecommunication)
  - [setConnectionIPAddress(string IPAddress)](#setconnectionipaddressstring-ipaddress)
  - [setConnectionPort(int port)](#setconnectionportint-port)
  - [String getConnectionIPAddress()](#string-meposgetassignedip)
  - [String MePOSGetAssignedIP()](#string-meposgetassignedip)
  - [String getMACAddress()](#string-getmacaddress)
  - [String getSSID()](#string-getssid)
  - [String getRouterFirmware()](#string-getrouterfirmware)
  - [boolean MePOSConnectDefault()](#boolean-meposconnectdefault)
  - [boolean MePOSConnectEthernet(String ipAddress, String netMask)](#boolean-meposconnectethernetstring-ipaddress-string-netmask)
  - [boolean MePOSConnectEthernet()](#boolean-meposconnectethernet)
  - [boolean MePOSConnectWiFi(String SSID, String IPAddress, String netmask, String encryption, String password)](#boolean-meposconnectwifistring-ssid-string-ipaddress-string-netmask-string-encryption-string-password)
  - [boolean MePOSSetAccessPoint(String SSID, String encryption, String password)](#boolean-meposconnectwifistring-ssid-string-ipaddress-string-netmask-string-encryption-string-password)
- [MePOSReceipt](#meposreceipt)
  - [setCutType(int cutType)](#setcuttypeint-cuttype)
  - [setHeaderFeed(int headerFeed)](#setheaderfeedint-headerfeed)
  - [setFooterFeed(int footerFeed)](#setfooterfeedint-footerfeed)
  - [setFeedAfterCut(int feedLines)](#setfeedaftercutint-feedlines)
  - [MePOSReceiptBarcodeLine(int type, String data)](#meposreceiptbarcodelineint-type-string-data)
  - [MePOSReceiptFeedLine(int lines)](#meposreceiptfeedlineint-lines)
  - [MePOSReceiptImageLine(Path path)](#meposreceiptimagelinepath-image)
  - [MePOSReceiptImageLine(BufferedImage image)](#meposreceiptimagelinebufferedimage-image)
  - [MePOSReceiptImageBufferLine()](#meposreceiptimagebufferline)
  - [MePOSReceiptPriceLine(String leftText, int leftStyle, String rightText, int rightStyle)](#meposreceiptpricelinestring-lefttext-int-leftstyle-string-righttext-int-rightstyle)
  - [MePOSReceiptSingleCharLine(char chr)](#meposreceiptsinglecharlinechar-chr)
  - [MePOSReceiptSingleCharLine(char chr)](#meposreceipttextlinestring-text-int-style-int-size-int-position)
  - [MePOSReceiptTextLine(String text, int style, int size, int position)](#meposreceipttextlinestring-text-int-style-int-size-int-position)
  - [Text style constants](#text-style-constants)
  - [Text size constants](#text-style-constants)
  - [Text position constants](#text-style-constants)
- [MePOS PRO Emulator](#mepos-pro-emulator)
- [Contact](#contact)

## Supported platforms

The MePOS Connect library supports Windows, MacOS and Linux enviroments.

## Supported MePOS firmware

The MePOS connect SDK has been tested with the latest MePOS 3.1 firmware.

## Use of the MePOS Connect SDK on Java

### Libraries

The MePOS Connect SDK for Java currently includes one jar.

* meposconnect-java.jar

### Add the SDK to your project

- Download the .jar file from [here](https://github.com/UniqueSecure/MePOSJavaSDK/tree/master/jars)
- Add the jar to your project

Continue [here](#creating-a-new-mepos-object)

#### References

  If the library has been referenced correctly, you should be able to add the following reference to your project without receiving errors:

**import com.uniquesecure.meposconnect.*;**

### Creating a new MePOS object

  To instantiate a new class to communicate with the MePOS unit, you will need to add the following code to your application:
  
##### MePOS mePOS = new MePOS(MePOSConnectionType.USB);
##### MePOS mePOS = new MePOS(MePOSConnectionType.WIFI);

  The above code has now instantiated a MePOS object.

### Adding log levels

  If you need to output log info into the console you can use the next constructor

##### MePOS(MePOSConnectionType type, MePOSLogger.LOG_LEVEL level);

  The MePOSLogger.LOG_LEVEL's are LEVEL_TRACE, LEVEL_DEBUG, LEVEL_INFO, LEVEL_ERROR.
  
  By default the MePOSLogger.LOG_LEVEL is set to `LEVEL_ERROR`.

### Adding a custom logger

  If you need to output log info into your own application you can use the next constructor

##### MePOS(MePOSConnectionType type, MePOSLogger.LOG_LEVEL level, MePOSLogger.LoggerListener listener);

  The object MePOSLogger.LoggerListener will receive all the events basted on the MePOSLogger.LOG_LEVEL.

### MePOS SDK Methods

Once a MePOS object has been created there are several methods that can be executed that perform actions on
the MePOS unit. Note that some of them are not recommended to execute on the main Thread.

### int setDiagnosticLed(int position, int colour)

  Will set the diagnostic LED indicated. The positions are defined in ***MePOSDiagnosticLEDS*** class, as the colors in ***MePOSColorCodes.***

### int setLedOneCol(Integer colour), int setLedTwoCol(Integer colour), int setLedThreeCol(Integer colour)

  (Deprecated: use setDianosticLed instead)
  Will set one of the three diagnostic LED's on the MePOS unit to one of the following colours:

  - MePOSColorCodes.LED_OFF (0)

  - MePOSColorCodes .LED_GREEN (1)

  - MePOSColorCodes.LED_RED (2)

  - MePOSColorCodes.LED_AMBER (3)

  ***Note:***  Between the development version and early versions of the MePOS unit the colours 1 and 2 (Green and
Red are swapped).

### int setCosmeticLedCol(Integer colour)
Will set the MePOS cosmetic LED to one of the following colours:
  - MePOSColorCodes.COSMETIC_OFF

  - MePOSColorCodes.COSMETIC_BLUE

  - MePOSColorCodes.COSMETIC_GREEN

  - MePOSColorCodes.COSMETIC_CYAN

  - MePOSColorCodes.COSMETIC_RED

  - MePOSColorCodes.COSMETIC_MAGENTA

  - MePOSColorCodes.COSMETIC_YELLOW

  - MePOSColorCodes.COSMETIC_WHITE

### boolean loadImage(InputStream stream) throws MePOSException

  Loads the image contained in the stream to the printer, it is advisable to load the image to the printer about 2-3 seconds before trying to print it from memory.

### boolean loadImage(Path path) throws MePOSException

  Loads the image contained in the path to the printer, it is advisable to load the image to the printer about 2-3 seconds before trying to print it from memory.

### boolean loadImage(BufferedImage image) throws MePOSException

  Loads the image to the printer, it is advisable to load the image to the printer about 2-3 seconds before trying to print it from memory.

### boolean printerBusy()

  Print commands are sent to the printer asynchronously, print or printRAW will return immediately to prevent locking the UI thread on a tablet device. To control the tablet UI and prevent possible print buffer overflow it is possible to monitor the printer busy status. New receipts cannot be printed unless the printerBusy method returns false.

### int print(MePOSReceipt receipt)

  Prints a pre-defined MePOS receipt using the built in receipt printer. To print a receipt, you must first create a MePOS receipt and add lines to it using the add command. This method will return 0 if the receipt was queued, or 1 otherwise. This method integrates by default a printer queue.

  The below example prints a single line receipt:

```java
    MePOSReceipt r = new MePOSReceipt();
    r.addLine(new MePOSReceiptTextLine(“Hello World”, MePOS.TEXT_STYLE_BOLD, MePOS.TEXT_SIZE_WIDE, MePOS.TEXT_POSITION_CENTER));
	int success = mePOS.print(r);
```

### int print(MePOSReceipt receipt, MePOSPrinterCallback callback)
  
  Same as above, but adding a `MePOSPrinterCallback` implementation to handle events as `onPrinterStarted`, `onPrinterCompleted` or `onPrinterError`.

### int print(MePOSReceipt receipt, MePOSPrinterCallback callback);

  Prints a pre-defined MePOS receipt using the built in receipt printer. When MePOS starts or finishes a receipt, it will call onPrinterStarted(), onPrinterCompleted() or onPrinterError(). This method also integrates by default a printer queue.

### int printRAW(String command)

  The raw print command allows the user to send ESC POS commands directly to the printer without using the receipt builder. This function is useful if your epos system already prints using the ESC POS command set. This method will return 0 for success, 1 if no MePOS is connected or 2 if the printer is busy.

### int serialRAW(String command)

  The serial raw command allows the user to send commands directly to the DE9 port. This method will return 0 for success or 1 if an  error happened.

### int cashDrawerStatus() throws MePOSException

  This command will return the status of the Cash Drawer. Possible values are:

  - MePOS.CASH_DRAWER_STATUS_OPENED (0)
  - MePOS.CASH_DRAWER_STATUS_CLOSED(1).

### boolean openCashDrawer(boolean validateCashDrawerStatus) throws MePOSException

  If there is any cash drawer connected to the MePOS, this command will try to open it. The method returns true if opens or false if it was already opened. If you need to avoid validation of cash drawer status and send the command directly, use ** validateCashDrawerStatus ** as ** false. **

### boolean openCashDrawer() throws MePOSException

  - Same as openCashDrawer(true).

### void enableUSB() throws MePOSException

  - Enables the USB ports on the MePOS device. It must be noted that, due to the nature of the USB architecture in the MePOS, the USB connection with the MePOS (if any) will reset, and a new USB instance must be created to give the apropriate permissions to the application to communicate with the devices as if the devices were physically unpluged and pluged back again.

### void disableUSB() throws MePOSException

  - Disables the USB ports on the MePOS device. It must be noted that, due to the nature of the USB architecture in the MePOS, the USB connection with the MePOS (if any) will reset, and a new USB instance must be created to give the apropriate permissions to the application to communicate with the devices as if the devices were physically unpluged and pluged back again.

### void enableWifi() throws MePOSException

  - Enables the Wifi module on the MePOS device.

### void disableWifi() throws MePOSException

  - Disables the Wifi module on the MePOS device. It must be noted that, if working with a WiFi instance, the communication after this method will be interruped, to re-enable WiFi communication a USB instance will be needed.
  
### void enableCosmeticLEDButton() throws MePOSException

  - Enables the cosmetic LED rotating button.

### void disableCosmeticLEDButton() throws MePOSException

  - Disables the cosmetic LED rotating button.

### String getFWVersion()

  - Gets the characteristics of the firmware version as Doc number, article code, revision and date of the firmware.

### String getSerialNumber()

  - Gets the serial number of the MePOS.

### MePOSConnectionManager getConnectionManager()

  - Gets the MePOSConnectionManager for the current MePOS instance. The connection manager can be used to set up the Wi-Fi network on the MePOS unit and query the connection state of the MePOS unit.

### MePOSConnectionManager

  The MePOSConnectionManager can be used to configure the connection settings for the MePOS unit and to configure the Wi-Fi module on a MePOS unit. The methods of this interface will not respond immediately, and is encouraged to the user to execute it asynchronously.

### int getConnectionStatus()

  Gets the current connection state of the MePOS unit:

  -1 = Not initialised

  0 = Not connected

  1 = Connected

### terminateCommunication()

  Terminates the bidirectional communication with the MePOS, it is good practice to call this method when not interested in receiving notifications from MePOS. The developer must garantee to innvoke this method before exiting their program.

### setConnectionIPAddress(string IPAddress)

  Sets the IP address on which the connection manager will look for a MePOS unit. The default IP address of the MePOS from the factory is 192.168.16.254, if the tablet is connecting to the MePOS as a client then you will not need to change this parameter unless the network settings on the MePOS unit have been changed.
  
### setConnectionPort(int port)

  Sets the tcp port on which the connection manager will look for a MePOS unit. The default port is 8080, but if you are controlling a generic esc/pos device you might want to change it to 9100.

### String getConnectionIPAddress()

  Gets the current IP address setting for the connection manager, this method returns "0.0.0.0" when the WiFi client configurations fails.

### String MePOSGetAssignedIP()

  Gets the current IP address from the connected MePOS unit. When connected to Wi-Fi this will be either the statically provided IP address or the DHCP network assigned IP address. In access point mode this will be the default IP address of 192.168.16.254.

### String getMACAddress()

  Gets the Mac Address of the MePOS unit.

### String getSSID()

  Gets the current SSID of the MePOS unit if is configured as WiFi Client or Access Point.

### String getRouterFirmware()
  Gets the actual Router Firmware of the MePOS unit.

### boolean MePOSConnectDefault()

  Sets the MePOS unit as factory settings, must call enableWiFi() before invoking this method to garantee the correct configuration of the module.

### boolean MePOSConnectEthernet(String ipAddress, String netMask)

  Set the MePOS unit to a network as a client using an Ethernet connection, must call enableWiFi() before invoking this method to garantee the correct configuration of the module.

  Valid input parameters are:

  - IPAddress – A valid static IP Address e.g. 192.168.0.100 or DHCP
  - ****(MePOSConnectionManager.IP_AS_DHCP)**** to request a DHCP address from the network.
  - netMask – A valid network mask e.g. 255.255.255.0, if the IPAddress parameter was DHCP this parameter will be ignored.

### boolean MePOSConnectEthernet()

  Set the MePOS unit to a network as a client using an Ethernet connection and as DHCP, must call enableWiFi() before invoking this method to garantee the correct configuration of the module. Same as MePOSConnectEthernet(“DHCP”, null)

### boolean MePOSConnectWiFi(String SSID, String IPAddress, String netmask, String encryption, String password)

  Connects the MePOS unit to a WiFi network as a client, must call enableWiFi() before invoking this method to garantee the correct configuration of the module. After performing a Wi-Fi connection, the setConnectionIPAddress method must be called with the provided static IP address or the assigned DHCP IP address using the MePOSGetAssignedIP() method. If the MePOS unit is being used as an access point, connecting to a Wi-Fi network will switch the MePOS to becoming a Wi-Fi client and the MePOS unit will no longer be a Wi-Fi access point. It is only possible to configure the WiFi module when the MePOS unit is plugged in via USB, a call to this method will return false if no USB connection is found.

  Valid input parameters are:

 ***SSID*** – The SSID of the Wi-Fi network you are connecting to.

 ***IPAddress*** – A valid static IP Address e.g. 192.168.0.100 or DHCP

 *** (MePOSConnectionManager.IP_AS_DHCP) *** to request a DHCP address from the network.

 Netmask – A valid network mask e.g. 255.255.255.0, if the IPAddress parameter was DHCP this parameter will be ignored.

 Encryption – The encryption type of the network you are connecting to, one of the following values NONE, WEP, WEP_OPEN, WPA_TKIP, WPA_AES, WPA2_TKIP, WPA2_AES, WPAWPA2_TKIP, WPAWPA2_AES. The Encryption modes are defined as constants in the enum: ***EncryptionMode.***

 Password - The password for the network you are connecting to. This can be left blank or will be ignored if the encryption type was set to NONE.

 ***boolean MePOSSetAccessPoint(String SSID, String encryption, String password)***

 Sets the MePOS unit in to access point mode, must call enableWiFi() before invoking this method to garantee the correct configuration of the module. When entering access point mode, the MePOS unit will create its own Wi-Fi network with the SSID, encryption and password provided. In access point mode the MePOS unit will create the IP network 192.168.16.0 and will get the IP address 192.168.16.254. Clients connecting to the MePOS unit will be automatically assigned IP addresses via DHCP. If the MePOS unit is connected to a Wi-Fi network as a client, setting the MePOS unit in access point mode will remove the MePOS unit from any Wi-Fi networks it is connected to in client mode. It is only possible to configure the WiFi module when the MePOS unit is plugged in via USB, a call to this method will return false if no USB connection is found.

  SSID – The SSID of the Wi-Fi network you are creating.

  Encryption – The encryption type of the network you are creating, one of the following values NONE, WEP, WEP_OPEN, WPA_TKIP, WPA_AES, WPA2_TKIP, WPA2_AES, WPAWPA2_TKIP, WPAWPA2_AES.
  The Encryption modes are defined as constants in the enum: EncryptionMode.

  Password - The password for the network you are creating. This can be left blank or will be ignored if the encryption type was set to NONE

### MePOSReceipt

  The MePOS library also contains the classes to define and print a receipt using the receipt printer.

  To create a new receipt, use the following code:

  ***MePOSReceipt r = new MePOSReceipt();***

  After the receipt has been initialised you can add lines to the receipt for printing. There are currently six types of line available to print on a receipt, these are detailed below.

### setCutType(int cutType)

  Specifies whether to perform a full or partial cut at the end of the receipt where the cut type is:
  - MePOS.CUT_TYPE_FULL
  - MePOS.CUT_TYPE_PARTIAL
  
  Once the receipt has been created you can modify how the printer will cut the receipt after it has finished printing. As a default the printer is set to perform a full receipt cut.
  
  By configuring this value as `MePOS.CUT_TYPE_PARTIAL` the `feedAfterCut` will override internally to 4 lines. if you need to specify the number of lines after the paper cut you'll need to call (setFeedAfterCut(int feedLines)[#setfeedaftercutint-feedlines]) after the cut type configuration.
  
### setHeaderFeed(int headerFeed)

  Specifies the number of feed lines before starting to print the receipt content. As a default is configured to 0 lines.
  
### setFooterFeed(int footerFeed)

  Specifies the number of feed lines after printing the receipt, but before the paper cut. As a default is configured to 12 lines.
  
  
### setFeedAfterCut(int feedLines)
  Specifies the number of feed lines after cutting the receipt. As a default is configured to 0 lines.
  
  
### MePOSReceiptBarcodeLine(int type, String data)
  The barcode line can be used to add a barcode to a receipt. There are currently three supported barcode types, UPC-A, Code 39 and PDF417. They are specified using the following constants:

- MePOS.BARCODE_TYPE_UPCA
- MePOS.BARCODE_TYPE_CODE39
- MePOS.BARCODE_TYPE_PDF417

  The following example shows how to add a barcode to a receipt:
### MePOSReceipt r = new MePOSReceipt();
### r.AddLine(new MePOSReceiptBarcodeLine(MePOS.BARCODE_TYPE_PDF417, “Hello World!”);

  By default, the Barcode prints a human-interface readable string below, and a defined height of around 0.35 inches. If you want to customise the height or the hri, please use this constructor instead:
### new MePOSReceiptBarcodeLine(MePOS.BARCODE_TYPE_CODE39, MePOS.BARCODE_HRI_NONE, 0.50, “Hello World!”);
  Please note that the height is ***not customisable*** on ***PDF 417*** barcodes.

### MePOSReceiptFeedLine(int lines)

  The feed line can be used to add whitespace to a receipt. The parameter supplied is the number of lines to feed.

  The following example shows how to feed 10 lines on a receipt:
  ***MePOSReceipt r = new MePOSReceipt();***
  ***r.AddLine(new MePOSReceiptFeedLine(10);***

### MePOSReceiptImageLine(Path path)

  The image line can be used to print black and white raster graphics to the printer. The path provided must be a valid java.nio.file.Path, the image size should be 576 x 200 pixels

  The following example shows how to add an image to a receipt:
  ***MePOSReceipt r = new MePOSReceipt();***
  ***r.AddLine(new MePOSReceiptImageLine(path);***

### MePOSReceiptImageLine(BufferedImage image)

  The image line can be used to print black and white raster graphics to the printer. The bitmap provided must be a valid java.awt.image.BufferedImage, the image size should be 576 x 200 pixels

  The following example shows how to add an image to a receipt:
  ***MePOSReceipt r = new MePOSReceipt();***
  ***r.AddLine(new MePOSReceiptImageLine(image);***

### MePOSReceiptImageBufferLine()

  The image buffer line can be used to print the image loaded to the printer

  The following example shows how to add an image to a receipt:
  ***MePOSReceipt r = new MePOSReceipt();***
  ***r.AddLine(new MePOSReceiptImageBufferLine();***

### MePOSReceiptPriceLine(String leftText, int leftStyle, String rightText, int rightStyle)

  The price line can be used to add a line to a receipt with text on the left and right simulating the common price item layout of many receipts. The price line takes parameters defining the text on the left and right and also the style of the text. Text styles are discussed later in this document.

  The following example shows how to add a price line to a receipt:
  ***MePOSReceipt r = new MePOSReceipt();***
  ***r.AddLine(new MePOSReceiptPriceLine(“Some Item”, MePOS.TEXT_STYLE_NONE, “Some Price”, MePOS.TEXT_STYLE_NONE);***

### MePOSReceiptSingleCharLine(char chr)
  The single character line can be used to fill a single line with the same character. The parameter is the character to repeat across the whole line.

  The following example shows how to add a single character line to a receipt:

  ***MePOSReceipt r = new MePOSReceipt();***
  ***.AddLine(new MePOSReceiptSingleCharLine(‘.’);***

### MePOSReceiptTextLine(String text, int style, int size, int position)

  The text line can be used to put text on a receipt on the left centre or right in different sizes or styles. The first parameter is the text to print, the size and position constants are discussed later in this document.

  The following example shows how to add a text line to a receipt:
  ***MePOSReceipt r = new MePOSReceipt();***
  ***r.AddLine(new MePOSReceiptTextLine(“Hello World!”, MePOS.TEXT_STYLE_NONE, MePOS.TEXT_SIZE_NORMAL, MePOS.TEXT_POSITION_CENTER);***

### Text style constants
  Some of the printer line commands accept a style parameter. This parameter can be any one of the following constants:
- MePOS.TEXT_STYLE_NONE
- MePOS.TEXT_STYLE_BOLD
- MePOS.TEXT_STYLE_ITALIC
- MePOS.TEXT_STYLE_UNDERLINED
- MePOS.TEXT_STYLE_INVERSE

  Text styles can also be combined using the "or" operator to achieve a mix of styles, for example to print bold italic text you would use the following:
- MePOS.TEXT_STYLE_BOLD | MePOS.TEXT_STYLE_ITALIC

### Text size constants

  Some of the printer line commands accept a size constant. This can be one of the following but not both:
- MePOS.TEXT_SIZE_NORMAL
- MePOS.TEXT_SIZE_WIDE

### Text position constants

  Some of the printer line commands accept a position constant. This can be any of the following:
- MePOS.TEXT_POSITION_LEFT
- MePOS.TEXT_POSITION_CENTER
- MePOS.TEXT_POSITION_RIGHT

## Sample Codes
- [MePOS print WIFI/USB](https://github.com/UniqueSecure/MePOS-Print-button-WIFI-USB)
- [MePOS print WIFI/USB With decorative LEDs](https://github.com/UniqueSecure/MePOS-Print-button-WIFI-USB-Decorative-LED)
- [MePOS WIFI modes](https://github.com/UniqueSecure/WifiModesExample)

## MePOS PRO Emulator
If you don't have a MePOS PRO unit you can download the MePOS PRO Emulator [here](http://sdk.mepos.io/MePOSEmulator_v_0_3.zip)

## Contact

Please rise a ticket on [MePOS Support](https://mepos.zendesk.com/hc/en-us/requests/new)
