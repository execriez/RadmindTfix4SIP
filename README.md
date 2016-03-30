# RadmindTfix4SIP
Process a Radmind transcript to be compatible with SIP on OSX

##Description:

Radmind expects the root user to be able to write to all areas of the local
filesystem. In OSX 10.11 and later, SIP (System Integrity Protection) 
protects certain system folders from being written to.

This script removes any entries from a transcript that are protected,
which allows the transcript to be applied (with lapply) without error.


The protected system files are defined in the following system file:

	/System/Library/Sandbox/rootless.conf
  
...or by the extended file attribute "com.apple.rootless"

The rootless.conf file is only examined at boot time and is itself
protected by SIP.

Entries in rootless.conf consist of a process name, followed by a file 
system object. Only the listed processes can modify the filesystem object.

Where the process name is shown as an asterix, this indicates that all
processes can modify the object. 

Where there is no process name, this indicates that no process can modify the object.

A trailing "/*" signifies that the restriction is on the contents of the directory, rather than the directory itself.

So consider the following lines from rootless.conf

	  /System                      - no process can modify this dir and contents
	* /System/Library/Extensions   - all processes can modify this dir and contents
	  /System/Library/Extensions/* - no process can modify the contents of this dir

So you can add files/folders to /System/Library/Extensions, but whatever you add will be protected after the next reboot.

Strangely, I have come across files that have neither a "com.apple.rootless" extended attribute, nor an entry in rootless.conf - and yet are still SIP protected. This script cannot handle such files, so whenever they turn up they should be added to an exclude file.

However in general, SIP protection appears to be too complex to completely emulate using simple negative or exclude transcripts, hence the need for this script.


##Installation:

Download the "RadmindTfix4SIP" script to a convenient location.


##How to use:

In a shell, Type:
	
	RadmindTfix4SIP SourceTranscriptFile [DestTranscriptFile]


##History:

1.0.7 - 30 MAR 2016

* Improved processing time.
* Removed fix 1.0.6 as it took way too long to process. Such files may be 
  handled via an exclude.
* Removed .sh extension
* Moved to GitHub

1.0.6 - 10 DEC 2015

* Fix to cater for files that have neither a "com.apple.rootless" extended 
  attribute, nor an entry in rootless.conf - and yet are still SIP protected!

1.0.5 - 09 DEC 2015

* Fix to cater for files with an extended attribute "com.apple.rootless"
  that don't appear in the rootless.conf file.

1.0.4 - 27 OCT 2015

* Fix to cater for files with an extended attribute "com.apple.rootless"
  hinted at the in rootless.conf file. 

1.0.3 - 15 OCT 2015

* Fix to make sure that the script only runs on pre 10.11 systems if 
  rootless.conf is in the same directory as the script. 
  (10.10 has a cut-down rootless.conf at /System/Library/Sandbox/.)

1.0.2 - 18 SEP 2015

* Added ability to run on a pre 10.11 system without SIP.
  (put the rootless.conf file in the same directory as the script)

1.0.1 - 18 SEP 2015

* First release.
