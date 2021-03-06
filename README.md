General instructions to get Lotus/IBM Notes to install
======================================================

Lotus/IBM Notes is generally only supported on LTS-version of Ubuntu. Since
package names tend to change over releases, deb-files like this will typically
fail to install because of their specific package requirements.

Chances are that things will work just fine, no matter what name the CUPS
printing system package is.

There are three scripts that can be used to fix dependencies in Lotus/IBM
Notes on debian based systems, more specifically Ubuntu.

* fix-9.0.1-dependencies.sh to fix ibm-notes-9.0.1.i586.deb
* fix-9.0.0-dependencies.sh to fix ibm-notes-9.0.i586.deb
* fix-8.5.2-dependencies.sh to fix ibm-lotus-notes-8.5.2.i586.deb

All scripts will unpack the package and create new ones postfixed with 'fixed'.
Install the fixed package and everything should be ready for the next setup steps.

Distro compatability
====================

The following distros have been verified:

* Ubuntu 13.04
* Ubuntu 13.10
* Ubuntu 14.04 (currently in development)

The following distro have been verified to work with 9.0.X

* openSUSE 13.1 (oxygen-gtk in KDE with notes-wrapper-gtk2-rc)

The following distro has been verified to work with 8.5.X
* openSUSE 12.3 (for Lotus Notes 8.5.2 & 8.5.3 with notes-wrapper-libgtk-fix)

Even though your distro is not a part of these cases are that it will work
anyway. Please report if you have success/failure on other distros.

Installation
-------------

The script ends by telling you which dpkg command you can use to install.

This should be this:

	$ sudo dpkg -i <filename>

The result might be that the system reports that the package can not be
installed because of missing packages. You must then complete the installation
by runnint this:

	$ sudo apt-get install -f

This will force installation of all packages to resolve all package
requirements.

Once done, you should now see 'IBM Notes' as application within the Office
section of LMDE Desktop or from some other place depending on your desktop
environment.

Now you are ready to do the post-install changes depending on the version of
IBM/Lotus Notes your have installed.

For IBM Notes 9.0.x
===================

IBM Notes works out of the box, except that weird things happen in
relation to the oxygen-gtk theme that are used primarily in KDE. Notes will
crash after sending email and many other places too. Selecting another theme
avoids the crash - but do you realy whant to look at firefox in Raleigh?

Luckily this can be fixed by unsetting the GTK2_RC_FILES environment variable
prior to starting notes, or by selecting a GTK theme other than oxygen-gtk in:
Configure Desktop --> Application Apperance --> GTK

Installation
------------

Just run 'make install90' to install the wrapper that reset the environment
variable.

Installation - openSUSE 13.1
----------------------------

You can install the notes-wrapper script via an rpm to null GTK2_RC_FILES by
installing notes-wrappper-gtk2-rc from the following repository:
http://download.opensuse.org/repositories/home:/eclipseagent:/lotus_notes/openSUSE_13.1/

This can be done by adding the repository, then running
   $ sudo zypper in notes-wrapper-gtk2-rc

The RPM will update the IBM Notes related .desktop files.

For Lotus Notes 8.5.x
=====================

Recent versions of Ubuntu and most likely all other distributions package
GTK 2.23.3 ir higher. That version contains an optimization that somehow
breaks Lotus Notes making it unable to draw things correctly on the screen.
Things tend to get drawn as gray areas instead of real content. This
software works around that problem.

Prerequisites
-------------
On Ubuntu the required libraries can be installed by running the following
command.

   $ sudo apt-get install libgtk2.0-dev

On 64bit Ubuntu g++-multilib also needs to be installed.

   $ sudo apt-get install g++-multilib

Installation
------------
Run 'make' to build the library. Remember to have all the required libraries
installed. Then run 'sudo make install85' to install the files and modify the
desktop-file. You can now run Lotus Notes from application-menu on your desktop.

Installation - openSUSE 12.3
----------------------------
You can install the notes-wrapper script and related gtk fixes via an rpm by
installing notes-wrappper-libgtk-fix from the following repository:
http://download.opensuse.org/repositories/home:/eclipseagent:/lotus_notes/openSUSE_12.3/

This can be done by adding the repository, then running
   $ sudo zypper in notes-wrapper-libgtk2-fix
   
The RPM will update the Lotus Notes related .desktop files.

Manual installation
-------------------
To install copy the files 'notes-wrapper' and 'libnotesgtkfix.so' to the
location of the 'notes' binary. For me it is in /opt/ibm/lotus/notes, but
for you it might be different.

Now fire up Lotus Notes with the 'notes-wrapper'-script instead of 'notes'.

With the current version of Lotus Notes (8.5.2) on linux the system wide
desktop-files is at /usr/share/applications/LotusNotes8.5.desktop. Editing that
files changing the shortcut there, will make notes run for all local users.

Running IBM/Lotus Notes
=======================

Upon first laucnh of IBM Lotus Notes, it should start in the following order:
* Display the Notes splash screen
* Open a terminal window in front where you should Read and Accept the licence,
or Abort. 
** If you Accept, you will a few moments later see the Notes Setup window where
you typically want to connect to an existing Domino server using your ID-file. 
* The rest is the same as for any other IBM Notes distribution.

Enjoy !
