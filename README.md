Android POS Print Driver (ESC/POS)
==================================

Download Pos Printer Driver Android app from Google Play [here](https://play.google.com/store/apps/details?id=com.fidelier.posprinterdriver).
<img src="https://lh5.ggpht.com/oj7DSgpI4iY2BU_WQNhTejmxqQw5JjPeBv3i7ntFozBcptmtSHH0KIXDNoE-uQkw-VA=w300" alt="logo">
Print easy, from your android device, in just 1 minute!
Print direct from web or print from other web direct to a sinc printer linked in an Android device!


## Features 
Simple driver app to interface ESC/POS printer with threee different network interface!

### TCP/WIFI
Input the printer’s IP and port it’s running on. 
Consult your printer manufacturers set up instructions to configure the printer’s TCP settings. This will likely need to be done through a other software.

### USB
Just plug your printer into your devices. 
Accept permission for the driver to access the USB device. 
Choose which printer you’d like to print to in the drop down box.


### BLUETOOTH
Just pair your printer into your devices. 
Accept permission for the driver to access the device. 
Choose which printer you’d like to print to in the drop down box. 
Test the connection and Click Save when satisfied. Test from your app.


REMOTE / LOCAL PRINT
--------------------

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

### Remote Server API Interface Through PosPrinterServer Cloud Server!

```

   Your App Beck-end Server
              |
              v
       .-------------.
       |             |
       |             |
       .-------------.
              |
              | HTTP GET/POST 
              |
              |
              |         Pos Printer Driver 
              |            Cloud Server  
              |                |                                                  
              |                v                                             
              |         .-------------.
              .-------> |             |--> device 1 -> ESC/POS printer (e.g. Bluethoot)
                        |             |
                        |             |--> device 2 -> ESC/POS printer (e.g. WIFI)
                        |             |           
                        |             |--> device N -> ESC/POS printer (e.g. USB)
                        .-------------.                                              


```




You need to initially configure the app
1. Download Pos Printer Driver Android app from Google Play [here](https://play.google.com/store/apps/details?id=com.fidelier.posprinterdriver).
2. Register your printer pushing the ´register´ button.
3.- Make a call to the GET/POST HTTP API endpoint:

 <DEVICE_ID> = XXXXX your device ID ("link code")

`
http://www.posprinterdriver.com/api/v1/api/sendDataToPrinter?linkcode=<DEVICE_ID>&data=HelloWorld$intro$ 
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

Open and close a tag
Include helpers for ESC commands like
Easy font size selection.

| Escape code  | Description
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


MY TEST ENVIRONMENT
-------------------
Android Samsung Galaxy S2 (Android v. 4.2.2) connected phisically (through an USB cable/adapter) with the great thank Epson TM-T20 printer.


BUSINESS MODEL
--------------
Free version could print ads on your ticket


CONTACTS
--------
Author of this readme e-mail: [giorgio.robino@gmail.com](mailto:giorgio.robino@gmail.com)
twitter: [@solyarisoftware](http://www.twitter.com/solyarisoftware)

[luis@blatta.com](mailto:luis.blatta.com)
[home page](http://www.posprinterdriver.com)
