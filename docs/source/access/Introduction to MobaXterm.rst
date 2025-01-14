Introduction to MobaXterm
=========================

MobaXterm is a comprehensive terminal application with many features
that provides remote computing tools such as SSH, RDP, X11, SFTP, FTP,
Telnet, Rlogin etc for programmers, webmasters, IT administrators, and
other users.

**Setting up MobaXterm on your local computer**

To install MobaXterm, go to the `MobaXterm
website <https://mobaxterm.mobatek.net/>`__ and selected 'Download' at
the top of the page

|img!|

On the next page, choose "Download Now" under Home Edition

|image1|

Finally, click the blue "Portable Edition" button or the green installer
edition.

|image2|

The Portable offers the convenience of unzip and run, no need for
installation.

Go into the unzipped folder and click on the MobaXterm application to
run the program

|image3|

**Setting up and SSH connection session to FEDGEN HPC CLUSTER**

MobaXterm allows to define a 'SSH Session' to simplify the process of
connecting to a remote server. First, start MobaXterm, then click on the
"Session" button in the upper left.

Next, choose the "SSH" option from pop-up menu

In the following menu, fill in "allot.hpc.fedgen.net" as the remote
host. Then check the "Specify username" box and fill in your Fedgen
Username.

Now, if you click the yellow star on the left panel, you should see a
line in the 'User sessions' column named something like
"allot.hpc.fedgen.net (Fedgen UserID)

|image4|

Click this and you will be prompted to enter the password for your
username, after which you will be logged into Allot login node.

**Copying files to and from the cluster**

After you connect to the cluster successfully, you will see on the left
sidebar on the *Sftp* tab a file browser on the cluster you are
connected to.

You can simply drag and drop files from your computer to that panel and
they will be copied to the cluster. The same is valid for retrieving
files from the cluster to your computer.

If you right click on that panel, you will see different options to
interact with the sftp browser (see the figure below). Remember always
to press the Refresh current folder button **after** you copied
something or a new file or folder is created on the cluster.

|SFTP MobaXtermNew|

.. |img!| image:: media/image1.png
   :width: 13.47847in
   :height: 8.25069in
.. |image1| image:: media/image2.png
   :width: 13.76042in
   :height: 8.37778in
.. |image2| image:: media/image3.png
   :width: 14.35139in
   :height: 7.50347in
.. |image3| image:: media/image4.png
   :width: 12.20069in
   :height: 6.70833in
.. |image4| image:: media/image5.jpeg
   :width: 11.975in
   :height: 8.125in
.. |SFTP MobaXtermNew| image:: media/image6.png
   :width: 7.5in
   :height: 5.6in
