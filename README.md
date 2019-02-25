# mDNSResponder
Apple - mDNSResponder for Linux Platform

Avahi is best zero conf package on Linux platform but it could not help you to pass the SRV probing test case in Apple Bonjour Conformance Test.Avahi could not able to resolve the hostname and dns services at the same time.

100-Register-Services.patch:
This patch helps mdns core(mdnsd) to publish services and so that you need to either create a stand-alone application or command line tool (dns-sd) to publish the services.

mDNSResponder-561.1.1.tar.gz:
Base code from Apple site.

And so I compiled mDNSResponder source code and added required patches to pass all test cases in Bonjour Conformance Test(BCT).

How to setup the mDNSResponder source folder?


1) Download the latest mDNSResponder source from apple site:
	http://opensource.apple.com/tarballs/mDNSResponder/

2) Apply all patches using following command
	patch -p1 -d <path-to-mDNSResponder-folder> < <path-to-patch-file>


How to compile on PC?
Tested on UBUNTU PC.

1) cd path-to-mDNSResponder-folder/

2) Clean:
	make clean os="linux" -C "mdnsPosix"

3) Build:
	make os="linux" -C "mdnsPosix"

4) Install:
	sudo make install os="linux" -C "mdnsPosix"

	-OR-

	Refer the Install script.

Note:
	For more info on binary files refer the README file in mDNSPOSIX folder.


How to cross-compile?
Tested on beaglebone/AM335x.

Set the following ENV variables with cross toolchain path. I think you can figure out these things if you know how to cross compile package.
CC=<gcc-cross-toolchain>
STRIP=<STRIP-cross-toolchain>

1) cd path-to-mDNSResponder-folder/

2) Clean:
	make clean os="linux" CC="$(CC)" LD="$(CC) -shared" STRIP="$(STRIP)" -C "mdnsPosix"

3) Build:
	make os="linux" CC="$(CC)" LD="$(CC) -shared" STRIP="$(STRIP)" -C "mdnsPosix"

4) Install:
	sudo make install os="linux" CC="$(CC)" LD="$(CC) -shared" STRIP="$(STRIP)" -C "mdnsPosix"

	-OR-

	For more info on what files needs to copy refer the Install script.

Note:
	For more info on binary files refer the README file in mDNSPOSIX folder.


How to start the mDNSResponder?
Before starting mdns, copy mdnsd.conf and mdnsd-services.conf to /etc folder.(don't change name, those are hardcoded in code)

Note: Update the files mdnsd.conf and mdnsd-services.conf according to your requirement.

Refer Services.txt file for more info on how to create services records file.

sudo service mdns start

-or- 

sudo /etc/init.d/mdns start

If things are not working then follow either one of the below method,

1) Run mdnsd -in debug mode
	mdnsd -debug

2) Enabel debug option in Makefile(mDNSResponder/mDNSPosix/MakeFile)
	set the debug variable to 1
	
	# Set up diverging paths for debug vs. prod builds
	DEBUG=1



