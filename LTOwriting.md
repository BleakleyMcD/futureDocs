---
title: LTO
layout: default
nav_order: 1
---

# LTO Writing
## We write many of our preservation and DPX files to LTO tape. This process is done entirely through the command line and is based on following steps 3, 7 and 9 of this document for creating DPX packages and video preservation AIP. 
## The process for writing LTO tapes is based on three main steps:
1.	Getting the files to be written to LTO to one of our two staging areas and validating all checksums
2.	Writing the files to an LTO tape
3.	Validating the checksums of the files written to the LTO tape These basic steps guide the whole process of writing files to LTO tape.

## Procedures for step 1
### If the files you wish to write to LTO are still on an external drive then you will need to get them either onto the staging area of the LTO Mac computer or onto the staging area of UtilitySAN drive. We can write LTO tapes from either. Unless there is a specific reason not to, the UtilitySAN is recommended.
1. If writing from the LTO Mac computer run:
* `rsyncT --exclude=".*" <source> <destination> && rsyncT -c
--exclude=”.*” <source> <destination>`
* We exclude hidden files because we don’t want them on our LTO tapes. The second rsync run with the -c flag causes rsync to run a full checksum on every file transferred on both ends of the transfer. Depending on how many Gigs of data is being moved, this can take a very long time.
2. If writing from the UtilitySAN drag and drop the files into the UtilitySAN.

3. If verifying the checksums is desired run:
* `rsync -vPic --stats --exclude=”.*” <source> <destination>`
7
 

11.2.	Change permissions of all files on LTO drive to 777.
11.2.1.	chmod 777 -vR <LTO_drive>
11.2.1.1.	This should avoid any permissions errors that may slow down the writing.
11.3.	Now that files we want to write to LTO are on one of our two staging areas it is time to name the tape(s) to which we will write and the directory from which we will write from.
11.4.	Our tape naming convention for LTO tapes is LT001A, LT001B, LT002A, LT002B, LT010A, LT010B, LT011A, LT001B, etc. Note that those are zeros not O’s.
11.5.	We want to fill tapes with between 2TB and 2.2TB of data. Try not to go over 2.2TB and definitely don’t go over 2.3TB. 2.4 is the total capacity of each tape. If you don’t have enough material it is ok to fill a lesser amount than 2.0TB and then fill in with more data later, but it is much preferable to write a full tape in one pass.
11.6.	Within the LTO Mac drive there is a directory called LTO and that is where you will create the directory for the tape you would like to write to - i.e. LT001A. The full path for this will look like:
11.6.1.	/Volumes/LTO/LTO/LT001A/<contents>
11.7.	Within the UtilitySAN there is a directory called a LTO and within that another directory called LTO. Inside this second directory is where you will create the directory for the tape you would like to write to - i.e. LT001A. The full path for this will look like:
11.7.1.	/Volumes/UtilitySAN/LTO/LTO/LT001A/<contents>
11.8.	Fill the LTO write directory (LT001A in the above examples) with between 2.0 and 2.2 TB of data.
11.8.1.	Since all the files are already on the drive this can be accomplished by simply dragging the files into the directory.
11.9.	To ease in the comparison of checksums once the LTO tape has been written we want to copy all checksums that will be being written to the tape into a single file which we will call LT001A_checksums.txt. This is easily accomplished by the following command:
★ find /Volumes/UtilitySAN/LTO/LTO/LT001A -name “*.md5” -exec cat {} >>
/Volumes/UtilitySAN/LTO/LTO/LT001A/LT001A_checksums.txt
This command will locate any files in directory LT001A (which we have just filled with all the files we want to write to LTO) that end with .md5. If all the files in LT001A have been properly checksummed to a .md5 file (which they should be) then this command will concatenate them all and write them to a new file, LT001A_checksums.txt
11.10.	We want the checksums to be sorted by file name, which the concat command about did not do. So run:

8
 

★ sort -k 2 -o
/Volumes/UtilitySAN/LTO/LTO/LT001A/LT001A_checksums.txt
/Volumes/UtilitySAN/LTO/LTO/LT001A/LT001A_checksums.txt
The file is now sorted. Open it and remove any .DS_Store lines and lines that are checksums of any checksum files. Delete these in the .md5 files that were concatenated as well.
11.11.	Let’s do one final remove of any hidden files in LT001A:
★ find <LTO_drive> -name “.*” -exec rm -vr {} \;
★ run as sudo, too, to remove spotlight, etc. (Is this automatic?)
★ be careful!!!
11.12.	Insert LTO tape into LTO deck
11.12.1.	Press Enter
11.12.2.	Highlight Commands and press Enter
11.12.3.	Highlight Insert Tape and press Enter
11.12.4.	Highlight To Drive and press Enter
11.12.5.	Insert tape, push until hard stop
11.13.	Run formatlto
11.13.1.	The terminal will ask for the tape ID. Our tape naming convention is LT001A, LT001B, LT002A, LT002B, etc. (those are zeros)
11.13.2.	If you need to force format a tape add the -f flag (formatlto -f). A tape will need to be force formatted if it has content on it and it needs to be erased. New tapes are blank, so this would be used in the case of a tape needing to be re-written due to error.
11.14.	Run mountlto
11.14.1.	The deck will mount the tape with the same name it was formatted with.
11.14.2.	It will appear on the desktop as a drive named LT001A
11.15.	In new Terminal window run writelto:
★ time writelto -e N -v -t LT001A /Volumes/UtilitySAN/LTO/LTO/
○	- e N means the tape with not eject when done writing
○	-v triggers “verbose” mode
○	-t indicates the target drive to write to, i.e. LT001A
○	/Volumes/UtilitySAN/LTO/LTO/ is the directory we are writing from
○	time will write to the terminal how long the entire process took once it has finished
11.16.	Press enter on the keyboard.
11.16.1.	All of the data in /Volumes/UtilitySAN/LTO/LTO/ will begin to write to the tape. This means there will be a directory at the top level of the tape named LT001A and all the data we want written will be inside it.
11.16.2.	This can take a long time.
11.17.	After the data has all written to the tape, the writelto function then creates checksums of every file on the tape.
11.17.1.	This part of the writelto command and can also take a long time.

9
 
11.17.2.   Anticipate this not all being able to finish in a single workday. LTO tapes run best overnight :)
11.18.	Once LTO tape is finished writing the last line in the terminal should read:
11.18.1.	"rsync exited with 0 on the first pass and 0 on the second pass"
11.18.2.	This means everything is cool. 0 is computer speak for all good.
11.18.3.	If it reads something different, there is a problem. Stop and investigate. Ask for help, if needed.
11.18.4.	This rsync error report also writes to
/Users/medialab/Documents/LT001A_writelto.txt, check that, too.
11.19.	The checksums writelto generates are saved in a .md5 manifest
11.19.1.	Find it at:
/Users/medialab/Documents/lto_indexes/readback_checksums/LT001A_ ReadBack_checksum_<yyyymmdd>-<hhmmss>.md5
11.20.	We want to compare this readback file to the checksums .txt file we created of all the checksums in LT001A
11.21.	To do this we will diff the two files:
★ diffy /Volumes/UtilitySAN/LTO/LTO/LT001A/LT001A_checksums.txt
/Users/medialab/Documents/lto_indexes/readback_checksums/LT001A_ ReadBack_checksum_<yyyymmdd>-<hhmmss>.md5
11.22.	The output in the terminal will be in two columns. If the checksums match there will be blank space between the columns. If there are any characters:
●		'|' - The corresponding lines differ, and they are either both complete or both incomplete
●	'<' - The files differ and only the first files contains the line
●	'>' - The files differ and only the second files contains the line
●	If there are any issues, stop and ask
●	read 'info diff' if you want to get deep :)
11.23.	If everything looks cool, we are ready to create the final manifest for the tape.
11.24.	Run makeltotree:
★ makeltotree /Volumes/LT001A
11.25.	Makeltotree will create a number of files about the contents on the tape and pull them all into a directory named LT001A_filemetadata in our Google Drive. It will then write this directory to the tape at the top level. It will then eject the tape.
11.26.	Don't forget to label the tape!!!!
11.27.	This whole process can now be repeated for LT001B. Just remember to change the names of relevant files and directories from L001A to LT001b

a. diff -y <(cat
/Users/medialab/Documents/lto_indexes/readback_checksums/LT001A_ReadBack_checksum_
<yyyymmdd>-<hhmmss>.md5 | cut -d ' ' -f 2) <(cat <path_to_checksums_on_3TB_LTO_drive> |
ut -d ' ' -f 2)

