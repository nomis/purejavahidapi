### Summary

PureJavaHidApi is a crossplatform  Application Programmin Interface (API) for accessing USB HID devices from Java, so it is a library aimed at programmers, not end users.


PureJavaHidApi is written 100% in Java so it is easy for Java programmers to develop and debug and it requires no native libraries. 

Native access to the underlaying operating system's USB device interface is provided by the wonderful JNA library which takes away all the pain of compiling and deploying native code.

### Project Status

In someways this is early days but the code is actual daily production use in the <a href="http://www.sparetimelabs.com/eazycnc/welcome/welcome.php" target ="eazycnc"> EazyCNC Project </a> so there is some credibility.

### Supported Platforms

* Windows
* Mac OS X 
* Linux

### Basic Functionality

PureJavaHidApi provides the capability to enumare (find) and open attached USB HID devices and send and receive reports i.e. chucks of bytes.

### Planned Functionality

Ability to read and parse the report descriptors (there is a semi decent parser in <a href="https://github.com/nyholku/purejavahidapi/tree/master/src/purejavahidapi/hidparser" target="hidparser"> purejavahidapi.hidparser </a> but the ability to read raw descriptor still eludes me.

### Documentation

The definitive PureJavaHidApi reference is the <a href="http://nyholku.github.io/purejavahidapi/javadoc/index.html" target="javadoc" > JavaDoc </a>.

### Why HID?

Why would you like to use PureJavaHidApi to access HID devices?

The answer is that you usually don't!

Most HID devices (like Mouse, Keyboard etc) have specific APIs provided by the operating system and you should use those to access them.

However some device which are not really Human Interface Devices in the original intent of the USB HID standard nevertheless represent themselves as HID devices. Devices like ThinkGeek  USB Rocket Launcher or  Oregon Scientific WMR100 weather stations to name two. 

It is spefifically for accessing these types of devices that PureJavaHidApi is aimed ford.

Now why do they represent represent themselves as HID devices?

For one overwhelming advantage over other class of USB devices: all major operating systems have built in HID drivers which means that no driver installation is required. Hard as it for a programmer to understand driver installation is a major hurdle and can make the difference between making the sale or not.

If you are considering developing a USB device then incarnating it as a HID device is an option worth considering.

So what is the catch?

HID devices are limited to transferring one 64 byte packet once each 1 msec or 64000 bytes/sec each way. If you need more than that you have take an other route, I suggest you head over to <a href="http://libusb.info" libusb project </a>.

### Alternatives

PureJavaHidApi is by no means the only game in town, for example there is <a href="https://github.com/gary-rowe/hid4java" target = "hid4java" > hid4java </a> which incidentally uses JNA just like PureJavaHidApi with the crucial difference that it builds on the  C-library <a href="https://github.com/signal11/hidapi" target="hidapi"> HIDAPI </a> which means that you need to solve the distribution and deployment of a native library along with your Java code.

### Code Example

To list all available HID devices use code like:

```java
import purejavahidapi.*;
...
List<HidDeviceInfo> list = PureJavaHidApi.enumerateDevices(0x0000,0x0000);
String path = null;
for (HidDeviceInfo info : list) {
	System.out.printf("VID = 0x%04X PID = 0x%04X Manufacturer = %s Product = %s Path = %s\n", //
	info.getVendorId(), //
	info.getProductId(), //
	info.getManufacturerString(), //
	info.getProductString(), //
	info.getPath());
	}

```

To open a generic gamepad and attach and input report listener:

```java
import purejavahidapi.*;

List<HidDeviceInfo> list = PureJavaHidApi.enumerateDevices(0x0810, 0x0005);
if (!list.isEmpty()) {
	HidDeviceInfo info = list.get(0);
	HidDevice dev = PureJavaHidApi.openDevice(info.getPath());
	dev.setInputReportListener(new InputReportListener() {
		@Override
		public void onInputReport(HidDevice source, byte reportID, byte[] reportData, int reportLength) {
			System.out.printf("onInputReport: reportID %d reportLength %d\n", reportID, reportLength);
			}
		});
	}

```


### Getting Started


### License 

PureJavaHidApi is BSD licensed but please note it depends on JNA which is LGPL/ASL dual licensed.


### Acknowledgment 

While PureJavaHidApi is totally independent developement from the great <a href="https://github.com/signal11/hidapi" target="hidapi"> HIDAPI </a>  by <a href="http://www.signal11.us" target="signal11"> SIGNAL11 </a> HIDAPI a lot of the techical and intricate knowledge need to access HID devices we cherry picked ripe from that project which I gratefully acknowledge.







 