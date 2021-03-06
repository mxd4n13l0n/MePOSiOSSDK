# MePOS Connect iOS SDK

The MePOS connect iOS SDK is designed to allow communication from any iOS 9 or iOS 10 capable device.
This document is a reference on how to integrate the MePOS unit into your own tablet application. This document does
not include information on how to set up the MePOS unit, for this please refer to the documentation that came with your
MePOS unit.

## Contents
- [Supported tablet devices](#supported-tablet-devices)
- [Supported MePOS firmware](#supported-mepos-firmware)
- [Version](#version)
- [Requirements](#requirements)
- [Use of the MePOS connect SDK on iOS](#use-of-the-mepos-connect-sdk-on-ios)
	- [Libraries](#libraries)
	- [How to import the framework](#how-to-import-the-framework)
- [Creating a new MePOS object](#creating-a-new-mepos-object)
- [MePOS SDK Methods](#mepos-sdk-methods)
	- [public func openCashDrawer() throws -> Bool](#public-func-opencashdrawer-throws---bool)
	- [public func openCashDrawer(validateCashDrawerStatus:Bool) throws -> Bool](#public-func-opencashdrawervalidatecashdrawerstatusbool-throws---bool)
	- [public func setDiagnosticLed(position:Int, colour:Int)](#public-func-setdiagnosticledpositionint-colourint)
	- [public func setCosmeticLedCol(colour:Int)](#public-func-setcosmeticledcolcolourint)
	- [public func printerBusy() -> Bool](#public-func-printerbusy---bool)
	- [public func print(receipt: MePOSReceipt, callback: MePOSPrinterCallback?)](#public-func-printreceipt-meposreceipt-callback-meposprintercallback)
	- [public func print(receipt: MePOSReceipt)](#public-func-printreceipt-meposreceipt)
	- [public func enableUSB() throws](#public-func-enableusb-throws)
	- [public func disableUSB() throws](#public-func-disableusb-throws)
- [MePOSReceipt](#meposreceipt)
	- [public func setCutType(cutType:Int)](#public-func-setcuttypecuttypeint)
	- [MePOSReceiptBarcodeLine(type:Int, data:String)](#meposreceiptbarcodelinetypeint-datastring)
	- [MePOSReceiptFeedLine(lines:Int)](#meposreceiptfeedlinelinesint)
	- [MePOSReceiptImageLine(image:CIImage)](#meposreceiptpricelinelefttextstring-leftstyleint-righttextstring-rightstyleint)
	- [MePOSReceiptPriceLine(leftText:String, leftStyle:Int, rightText:String, rightStyle:Int)](#meposreceiptpricelinelefttextstring-leftstyleint-righttextstring-rightstyleint)
	- [MePOSReceiptSingleCharLine(chr:Character)](#meposreceiptsinglecharlinechrcharacter)
	- [MePOSReceiptTextLine(text:String, style:Int, size:Int, position:Int)](#meposreceipttextlinetextstring-styleint-sizeint-positionint)
	- [Text style constants](#text-style-constants)
	- [Text position constants](#text-position-constants)
- [Sample Codes](#sample-codes)


## Supported tablet devices
The MePOS connect library supports Android tablets and Windows PC’s via a USB and Wi-Fi connection. Due to the
restrictions of the iOS platform it is not possible to connect to the MePOS unit via USB from an Apple device.

## Supported MePOS firmware

The MePOS connect SDK has been tested with the latest MePOS 3.1 firmware.

## Version
The current version of the MePOS SDK for iOS is 1.0.0

## Requirements
* iOS 9 or later
* XCode 8.2.1
* Swift 3

## Use of the MePOS connect SDK on iOS

### Libraries
The MePOS connect SDK for iOS currently includes two flavors.

* MePOS Connect for devices
* MePOS Connect for simulator
The MePOS connect library is provided as a folder (.framework) for use in XCode.

### How to import the framework
In this repository there are two frameworks folder. One of them must
be added to your Xcode project, depending if you are testing in a simulator or a real device.

This must be done in two places:

1) Add the framework to your project (Build Phases -> Link Binary -> + -> Other).

2) Add the path to the framework to the library search paths (Build Settings->Search Paths->Library Search
Paths).


## Creating a new MePOS object
To create a new MePOS object in your application you can use the following code:

```Swift
var mePOS:MePOS = MePOS();
```

##MePOS SDK Methods

Once a MePOS object has been created there are several methods that can be executed that perform actions on the MePOS unit.

###public func openCashDrawer() throws -> Bool###
Opens the cash drawer. Same as ```openCashDrawer(validateCashDrawerStatus:true)```

###public func openCashDrawer(validateCashDrawerStatus:Bool) throws -> Bool###
Opens the cash drawer. If the ```validateCashDrawerStatus``` flag is ```True``` the SDK will validate if the cash drawer is already opened. If not, the relay signal will be sent.
Returns true if the relay signal was sent, false if was prevented due of the validation.

###public func setDiagnosticLed(position:Int, colour:Int)###
sets die diagnostic leds to the position and color indicated. The color codes are shown in the ```MePOSColorCodes``` class:

```Swift
	public static let LED_OFF:Int = 0;
	public static let LED_GREEN:Int = 1;
	public static let LED_RED:Int = 2;
	public static let LED_AMBER:Int = 3;
```
Also the positions are referenced in the ```MePOSDiagnosticLEDS``` class:

```Swift
	public static let LED_POWER:Int = 0;
	public static let LED_NETWORK:Int = 1;
	public static let LED_TABLET:Int = 2;
	public static let LED_PED:Int = 3;
	public static let LED_PRINTER:Int = 8;
	public static let LED_USB1:Int = 9;
	public static let LED_USB2:Int = 10;
```

###public func setCosmeticLedCol(colour:Int)###

Sets the cosmetic led to the color indicated shown in the ```MePOSColorCodes``` class:

```Swift
    public static let COSMETIC_OFF:Int = 0;
    public static let COSMETIC_BLUE:Int = 1;
    public static let COSMETIC_GREEN:Int = 2;
    public static let COSMETIC_CYAN:Int = 3;
    public static let COSMETIC_RED:Int = 4;
    public static let COSMETIC_MAGENTA:Int = 5;
    public static let COSMETIC_YELLOW:Int = 6;
    public static let COSMETIC_WHITE:Int = 7;
```

###public func printerBusy() -> Bool###

Asks to the MePOS if the device is printing.

###public func print(receipt: MePOSReceipt, callback: MePOSPrinterCallback?)###

Prints a predefined MePOS receipt using the built-in receipt printer API. To print a receipt, you must first create a MePOS receipt and add lines to it using the add command.

The example below prints a single line receipt:

```Swift
	let receipt:MePOSReceipt = MePOSReceipt();
	let textLine:MePOSReceiptTextLine = MePOSReceiptTextLine();
	textLine.setText(text: "RECEIPT", style: MePOS.TEXT_STYLE_BOLD, size: MePOS.TEXT_SIZE_NORMAL, position: MePOS.TEXT_POSITION_CENTER);
	receipt.addLine(line: textLine);
	mepos.print(receipt: receipt, callback:self);
```
Note that you are asked to supply a ```MePOSPrinterCallback```. We recommend that you implement those methods to get notified when the MePOS starts printing a receipt, finished a receipt or there is an error during the printing process.

###public func print(receipt: MePOSReceipt)###

Same as ```print(receipt: receipt, callback: StubPrinterCallback());```

###public func enableUSB() throws###

Enables the USB ports on the MePOS device.

###public func disableUSB() throws###

Disables the USB ports on the MePOS device.


## MePOSReceipt
The MePOS library also contains the classes to define and print a receipt using the receipt printer.

To create a new receipt, use the following code:

```Swift
	let receipt:MePOSReceipt = Receipt();
```

After the receipt has been initialised you can add lines to the receipt for printing.

###public func setCutType(cutType:Int)###

Specifies whether to perform a full or partial cut at the end of the receipt where the cut type is:

* `MePOS.CUT_TYPE_FULL`
* `MePOS.CUT_TYPE_PARTIAL`

Once the receipt has been created you can modify how the printer will cut the receipt after it has finished printing. As a default the printer is set to perform a full receipt cut.

###MePOSReceiptBarcodeLine(type:Int, data:String)###
The barcode line can be used to add a barcode to a receipt. There are currently three supported barcode types, UPC-A, Code 39 and PDF417. They are specified using the following constants:

* `MePOS.BARCODE_TYPE_UPCA`
* `MePOS.BARCODE_TYPE_CODE39`
* `MePOS.BARCODE_TYPE_PDF417`

The following example shows how to add a barcode to a receipt:

```Swift
	receipt.addLine(line: MePOSReceiptBarcodeLine(type:MePOS.BARCODE_TYPE_PDF417, data:"Hello World!"));
```

By default, the Barcode prints a human-interface readable string below, and a defined height of around 0.35 inches. If you want to customise the height or the hri, please use this constructor instead:

```Swift
	receipt.addLine(line: MePOSReceiptBarcodeLine(type: MePOS.BARCODE_TYPE_CODE39, hri:MePOS.BARCODE_HRI_NONE, height:0.50, data:"Hello World!"));
```

Please note that the height is **not customisable on PDF 417** barcodes.

###MePOSReceiptFeedLine(lines:Int)###

The feed line can be used to add whitespace to a receipt. The parameter supplied is the number of lines to feed.

The following example shows how to feed 10 lines on a receipt:

```Swift
	receipt.addLine(line:MePOSReceiptFeedLine(lines:10));
```

###MePOSReceiptImageLine(image:CIImage)###

The image line can be used to print black and white raster graphics to the printer. The bitmap provided must be a valid Bitmap. The image size should be not less than 288 pixels wide.

The following example shows how to add an image to a receipt:

```Swift
	let fileUrl:URL = Bundle.main.url(forResource: "my_logo", withExtension: "bmp")!;
	let image:CIImage = CIImage(contentsOf: fileUrl)!;
	receipt.addLine(line: MePOSReceiptImageLine(image: image));
```

###MePOSReceiptPriceLine(leftText:String, leftStyle:Int, rightText:String, rightStyle:Int)###

The price line can be used to add a line to a receipt with text on the left and right simulating the common price item layout of many receipts. The price line takes parameters defining the text on the left and right and also the style of the text. Text styles are discussed later in this document.

The following example shows how to add a price line to a receipt:

```Swift
	receipt.addLine(MePOSReceiptPriceLine(leftText:"Some item", leftStyle:MePOS.TEXT_STYLE_NONE, rightText:"Some price", rightStyle:MePOS.TEXT_STYLE_NONE))
```

###MePOSReceiptSingleCharLine(chr:Character)###

The single character line can be used to fill a single line with the same character. The parameter is the character to repeat across the whole line.

The following example shows how to add a single character line to a receipt:

```Swift
	receipt.addLine(line:MePOSSingleCharLine(chr:"."))
```

###MePOSReceiptTextLine(text:String, style:Int, size:Int, position:Int)###

The text line can be used to put text on a receipt on the left centre or right in different sizes or styles. The first parameter is the text to print, the size and position constants are discussed later in this document.

The following example shows how to add a text line to a receipt:

```Swift
	receipt.addLine(line:MePOSReceiptTextLine(text:"Hello World!", style:MePOS.TEXT_STYLE_NONE, size:MePOS.TEXT_SIZE_NORMAL, position:MePOS.TEXT_POSITION_CENTER));
```

###Text style constants###

Some of the printer line commands accept a style parameter. This parameter can be any one of the following constants:

* `MePOS.TEXT_STYLE_NONE`
* `MePOS.TEXT_STYLE_BOLD`
* `MePOS.TEXT_STYLE_ITALIC`
* `MePOS.TEXT_STYLE_UNDERLINED`
* `MePOS.TEXT_STYLE_INVERSE`

Text styles can also be combined using the "or" operator to achieve a mix of styles, for example to print bold italic text you would use the following:

```MePOS.TEXT_STYLE_BOLD | MePOS.TEXT_STYLE_ITALIC```

###Text position constants###

Some of the printer line commands accept a position constant. This can be any of the following:

* `MePOS.TEXT_POSITION_LEFT`
* `MePOS.TEXT_POSITION_CENTER`
* `MePOS.TEXT_POSITION_RIGHT`

##Sample Codes##

- [MePOS Test iOS](https://github.com/UniqueSecure/MePOS-Test-iOS)