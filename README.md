Android POS Print Driver (ESC/POS)
==================================
>Print easy, from your android device, in just 1 minute!
>Print direct from web or print from other web direct to a sinc printer linked in an Android device!

<img src="https://lh5.ggpht.com/oj7DSgpI4iY2BU_WQNhTejmxqQw5JjPeBv3i7ntFozBcptmtSHH0KIXDNoE-uQkw-VA=w300" alt="logo">

## Introduction

In this way Luis Blatta introduce his beautiful Android application that acts asa a printer driver for ESC/POS printers (so called "POS" printers): roll paper thermal printers.
The powerfull of the deriver is that allow three different network interfaces between your phone/tablet and the printer using a wired or wireless connection allowed by your printer: 

* USB cable 
* TCP/WIFI 
* Bluethoot

But this app excel for another reason so important for developers: with this driver developer can print with three different paradigmas: 

* inside a custom Android app, using usual Android "intents"
* inside a normal web page visualized in the handset browser
* from a remote cloud server 

You can download Pos Printer Driver Android app from Google Play [here](https://play.google.com/store/apps/details?id=com.fidelier.posprinterdriver).

> This repository isn't a standard project, I admit, but more a collection of notes, tests and small chunks of code to explain better the features of the app. 


## Features Detail
Simple driver app to interface ESC/POS printer with threee different network interface!

* ### POS network printer, TCP (ethernet cable interface) or WIFI (wireless interface)
Input the printer’s IP and port it’s running on. 
Consult your printer manufacturers set up instructions to configure the printer’s TCP settings. This will likely need to be done through a other software.

* ### POS printer with USB cable interface
Just plug your printer into your devices. 
Accept permission for the driver to access the USB device. 
Choose which printer you’d like to print to in the drop down box.

* ### POS printer with Bluethoot wireless interface
Just pair your printer into your devices. 
Accept permission for the driver to access the device. 
Choose which printer you’d like to print to in the drop down box. 
Test the connection and Click Save when satisfied. Test from your app.


Local app vs remote server printing
-----------------------------------

### Print from your Android App (interactive user action)

Create your ESC data using the helpers Create an Android Intent using Add your ESC data as a “Data” extra Start the intent.
You can be printing in minutes with just a couple lines of code. It's as simple as creating your intent, adding your ESC formatted string and start the (service) intent.

Example:

```java

String dataToPrint="$big$This is a printer test$intro$posprinterdriver.com$intro$$intro$$cut$$intro$";

Intent intentPrint = new Intent();

intentPrint.setAction(Intent.ACTION_SEND);
intentPrint.putExtra(Intent.EXTRA_TEXT, dataToPrint);
intentPrint.setType("text/plain");

this.startActivity(intentPrint);

```

### Print from your web page (user action)

You can print from web doing a link in your html page: 

Print from a wab page visualized in an android device web browser. 
Just embed in you page a link, or a button:

```

a href="com.fidelier.printfromweb://$biguhw$Print From Web$intro$$small$Print small letter$intro$->$intro$->$intro$->$intro$$intro$$intro$$intro$$cut$$intro$"Test print from web/a


```

### PosPrinterServer Cloud Server Remote API !

```
   Your App Beck-end Server
              |
              v
       .-------------.
       |             |
       |             |
       .------|------.
              | HTTP GET/POST API
              |
              |       Pos Printer Driver 
              |          Cloud Server         device 1  
              |              |         .-------------.                                       
              |              v         | your app    |                            
              |         .---------.    | |           |       Bluethoot
              .-------> |         |--> | .--> driver | ----------------> ESC/POS printer 1
                        |         |    |             |
                        |         |    .-------------.
                        |         |                               
                        |         |                               WIFI
                        |         |--> device 2 -----------------------> ESC/POS printer 2
                        |         |                               
                        |         |                               
                        |         |                                USB
                        |         |--> device N -----------------------> ESC/POS printer N
                        .---------.                                              
```

You need to initially configure the app:

- Download Pos Printer Driver Android app from Google Play [here](https://play.google.com/store/apps/details?id=com.fidelier.posprinterdriver).
- Register your printer pushing the ´register´ button.
- Make a call to the GET/POST HTTP API endpoint:

 `DEVICE_ID ` = XXXXX your device ID ("link code")

`
http://www.posprinterdriver.com/api/v1/api/sendDataToPrinter?linkcode=DEVICE_ID&data=ESC_POS_DATA 
`

So suppose your DEVICE_ID code is "12345" and the ESC_POS_DATA string is the string "HelloWorld$intro$".

With curl in a terminal with an HTTP GET:

```bash
$ curl -X GET http://www.posprinterdriver.com/api/v1/api/sendDataToPrinter? \
              linkcode=12345&data=HelloWorld$intro$ 
```


With curl in a terminal with an HTTP POST:

```bash
$ curl -X POST http://www.posprinterdriver.com/api/v1/api/sendDataToPrinter? \
               linkcode=12345&data=HelloWorld$intro$ 
```


ESC/POS URL-encoding
--------------------
An ESC/POS printers get a stream of text embedding some "escape" formatting code, following the ESC/POS "standard" original defined by Epson.
The problem is that ESC codes contains are also non printable chars. To URL-encode ESC/POS commands, Luis Blatta propose two solutions:

* DOLLAR_SIGN_ENCODING
ESC_POS_DATA is a string containing ESC/POS commands represented by $symbol$ specified by the following table:


| $ Escape code  | Description
| ------------ |------------------------------------| 
|$small$       | small size|
|$smallh$       | small size with double hight
|$smallw$ | small size with double width
|$smallhw$ | small size with double hight and width
|$smallu$ | small size underline
|$smalluh$ | small size with double hight underline
|$smalluw$ | small size with double width underline
|$smalluhw$ | small size with double hight and width underline
|  |   | 
|$big$ | big size
|$bigh$ | big size with double hight
|$bigw$ | big size with double width
|$bighw$ | big size with double hight and width
|  |   | 
|$big$ | big size, underline
|$bigh$ | big size with double hight, underline
|$bigw$ | big size with double width, underline
|$bighw$ | big size with double hight and width, underline
|  |   | 
|$cut$ | cut the paper
|$drawer$ | open the first drawer

* DOT_ENCODING
This is a more versatile, complete solution for URL-encode any ESC/POS command: 
developer must represent each 'non printable' char with a special encoding, enclosing the decimal representation of the code with special sign `▪` (%C2%B7 URL-encoded), so by example
 * the char with decimal value 27 become ▪27▪, 
 * and ESC/POS escape sequence to cut the paper 'ESC m', is equal to 1B6D in hexadecimal and is equal to 27 109 in decimal, become:
`▪27▪▪109▪`
 * at least the complete string to cut the paper is: `▪27▪▪109▪▪13▪▪10▪`


My Test Environment
-------------------
Android Samsung Galaxy S2 (Android v. 4.2.2) connected phisically (through an USB cable/adapter) with the great thank Epson TM-T20 printer.

To do
-----
- [x] introduction
- [ ] publish a web page example
- [ ] publish a remote server API example using Ruby
- [ ] publish a Ruby function that translate from a text stream containing ESC/POS codes to  URL-encoding compliant with the Blatta's APIs.



Business Usage
--------------
Please contact [Luis Blatta](mailto:luis.blatta.com) Pos Printer Driver app owner. 


Contacts
--------
Author of this repository [e-mail](mailto:giorgio.robino@gmail.com), [twitter](http://www.twitter.com/solyarisoftware)
Owner of the app [e-mail](mailto:luis.blatta.com), [web](http://www.posprinterdriver.com)
