<bSong<br>
Spiring 2015<br>
CS 161<br>
Computer Security<br>
Project 3<br>
Due: May 8 11:59pm, 11:59PM<br></b>
####Background
Your unsung-but-heroic efforts a month ago lead to the miraculous defeat of the evil Calnet!
Freedom reigns across Caltopia . . .

. . . Or does it?

Walking home from a spirited late-night victory celebration with your Birkland colleagues,
you find your mind uneasy. In the wake of Calnet’s destruction, Governor Sylvester Stalloon
has called for an election to determine Caltopia’s future. Surely this is the right step forward,
yet . . . Something is out of place. Something is wrong. Your security spider-sense tingles!

Your phone vibrates with an incoming text. But whoa, you have zero bars—how
did that happen? Making a mental note to dump AT&T as soon as your contract
expires, you check out the short message: Vital u act or with this election we
lose all. Stalloony’s gonna steal it. Access and leverage his comm w/
gov-of-caltopia.info. Cripple repressive sw + steal email cred. + expose
fraud = profit. --Neo

Your head spins with questions and possibilities. Could such a heinous plot really be under
way? Couldn’t you just have a break from all this hassle until the end of the semester?

And how should you even begin? Can you really go up against the very top of the government
all alone by yourself?

Your phone vibrates again. Also, I suggest you work in team of 2. --Neo

####Getting Started
Haunted by the prospect of a stolen election, and daunted by the fate of Caltopia resting
entirely in your hands, you find yourself sleepless and your mind racing. Throughout the
night, texts come in, building out the picture and providing snippets of guidance.

First, Neo points you at an obscure file on a Berkeley server that turns out to supply you
with a VM tailored for enabling you to complete each step of the task ahead.

#####Software Setup
You can run and investigate the VM on your own computer. You will need the following
software:
• VirtualBox 1 , the virtualization server.
• Your favorite SSH client. 2

Start VirtualBox and go to File → Preferences → Network. Make sure there is a network
adapter listed under “Host-only Networks” named vboxnet0. 3 If the adapter list is empty,
click the plus on the right side to add a new interface. Confirm with OK.

Neo placed the VM image at http://cs.berkeley.edu/~cthompson/cs161s14/nethack_sp14.ova.
Download it and import it via File → Import Appliance.

Once you have imported the VM, you’ll need to point the CDROM at a Linux
LiveCD, which you can find at http://preview.tinyurl.com/xubuntutorrent (Torrent)
or http://preview.tinyurl.com/xubuntuiso (HTTP). To set up the CDROM open Vir-
tualBox and select the VM. Then click on Settings → Storage. Under Controller: IDE select
Empty. Now click on the little CD icon on the right. Select Choose a virtual disk and point
it to the Linux image you’ve downloaded. Note: If you are using Windows you may need
to disable the Serial Port. To do this click on Settings → Ports and uncheck “Enable Serial
Port.” You may need to do this for Port 1 and Port 2.

<bPro Tip:</b> It may be useful to make snapshots of your virtual machine in case you make
mistakes or break anything. Before you turn on your VM for the first time, select it and then
click Snapshots, and then press the camera icon to create a new snapshot (you can name this
one “pre login”). For example, if you enter your logins incorrectly, this will save you from
having to re-import the VM.

All of the network programs will run inside of this VM. The image is a bare-bones Xubuntu
Linux desktop installation on a 32-bit Intel architecture. The first time you boot the im-
age, you have to enter your class accounts in the format cs161-x 1 x 2 ,cs161-x 3 x 4 , where
x 1 , . . . , x 4 are the letters of your class accounts. You need to list the accounts in alpha-
betical order, with no spaces in between. For example, if a student with class account
cs161-we teams with a student with class account cs161-vv, then you would enter the
string “cs161-vv,cs161-we”. 5

#####Memory management.
The Virtual Machine comes pre-configured with a 1GB memory
limit. This is on the low side, and we recommend increasing it to at least 2GB, or as much
memory as you can afford. To do this, go to Settings → System and adjust the Base memory
slider. You can experience unexpected crashes if the VM runs out of memory. To conserve
memory, when operating within your VM:

• don’t surf the web (except where necessary);
• don’t browse “heavy” websites (such as nytimes.com);
• and definitely don’t watch any videos.

<b>Save early, save often.</b>

#####Instructional computing.
(Note: If you use your own computer to run the project,
skip this section.) Certain computers in the instructional labs have VirtualBox pre-installed
and pre-configured. We’ve tested this on the Hive machines. The virtual machines for this
project are somewhat large, so you’ll need to make a a temp directory. You need to set this
up before importing your VM, using the following steps:

1. SSH to cory.eecs.berkeley.edu and log in using your instructional account.
2. Run /share/b/bin/mkhometmpdir to create your personal directory. This creates a
folder /home/tmp/<accountname>. Logout.
3. Start VirtualBox on one of the Hive machines.
4. Go File → Preferences → General. Change the option Default Machine Folder to
/home/tmp/cs161-XX (your account name).
You can get more information about tmp folders in /home/tmp/README.txt.

####SSL Tools
The OpenSSL Project develops the libcrypto and libssl libraries, and the accompany-
ing openssl tool, which together provide full support for the latest SSL/TLS protocols.
Important tools you should familiarize yourself with include:

• openssl genrsa: This tool allows you to generate public/private keypairs. This key-
pair can be used for both authentication and encryption. As indicated by its name,
this tool generates keys for use with the RSA algorithm. While OpenSSL also supports
other public-key algorithms, Neo indicates you shouldn’t need those.
• openssl rsa: This tool allows you to inspect keys generated with the previous tool.
• openssl req: This tool allows you to generate and inspect Certificate Signing Requests
(CSR’s). A CSR is what you hand to a Certificate Authority (CA) for them to sign,
Project 3
Page 3 of 14
CS 161 – Sp 15and the CA will copy information from the CSR into a signed Certificate that they’ll
give you back.
• openssl x509: X.509 is the standard used for formatting certificates. This tool allows
you to inspect and manipulate such certificates.
• openssl s_client: The netcat of SSL connections. You can use this to connect to
SSL-enabled servers and send and receive data and view certificates and other connec-
tion metadata.

For more information about each tool, check out their man pages (e.g. man genrsa).
To inspect SSL-related files, you can use the following syntax:

RSA Private Key<br>
openssl rsa -in filename -text

RSA Public key. You may need to use -pubin instead of -RSAPublicKey_in depending on the input file.<br>
openssl rsa -in filename -text -RSAPublicKey_in

Certificate Signing Request<br>
openssl req -in filename -text

X.509 certificate<br>
openssl x509 -in filename -text

####Ethics
This project requires that you communicate and interact with real machines on the Internet.
You must not attack the machines in any way other than via intercepting and altering
communication that originates from your VM as part of this project. Do not ever attack the
servers directly for any reason! Any such attack or attempted unauthorized access constitutes
a violation of the Berkeley Campus Code of Student Conduct and could have direct
consequences for you as a student. In addition, it may expose you to legal jeopardy.
Finally, attacks on our servers or other university infrastructure may make it impossible
for us to do a project like this in the future. Please don’t ruin that opportunity for other
students.


You should conduct all of your analysis and exploration of the VM environment from within
the VM environment. Inspecting the VM externally (such as mounting it as a disk image
from outside VirtualBox) is not allowed and as such constitutes cheating.

If you have any questions about whether an attack or other action is in scope, do not hesitate
to email an instructor or post a private note on Piazza.


