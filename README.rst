======================
Personal Fedora Notes
======================

1. `Yum Config`_

2. `Yum Plugins`_

3. `Disable Nouveau`_

4. `Google Chrome`_

5. `RPM Fusion`_

6. `Android`_

7. `Java`_

8. `Thinkfan`_

9. `Media Codecs`_

10. `Bumblebee`_

11. `Dropbox`_

12. `ksuperkey`_

13. `TLP`_

14. `VirtualBox`_

15. `HandBrake`_

16. `Skype`_

17. `RedShift Plasma Widget`_

18. `Dropbox-Dolphin Integration`_

19. `Gnome Encfs Manager`_

20. `Genymotion`_

21. `SoundKonverter`_

22. `reStructuredText`_

23. `Microsoft Core Fonts`_

24. `Create A Yum Repository`_

25. `Speed up LibreOffice`_

26. `wmsystemtray`_
    
Yum Config
----------

- Keep Cache

::

  /etc/yum.conf
  ------------------------
  keepcache=1
  clean_requirements_on_remove=1

Yum Plugins
-----------

::

  sudo yum install yum-plugin-fastestmirror yum-plugin-local \
  yum-plugin-changelog yum-plugin-show-leaves

Disable Nouveau
----------------

- Blacklist ``nouveau`` module

::

  echo 'blacklist nouveau' >> /etc/modprobe.d/blacklist-nouveau.conf

- Recreate initramfs

::

  sudo mv /boot/initramfs-$(uname -r).img /boot/initramfs-$(uname -r)-nouveau.img
  sudo dracut --omit-drivers nouveau /boot/initramfs-$(uname -r).img $(uname -r)


`Ask Fedora how-to-disable-nouveau-in-fedora-18 <https://ask.fedoraproject.org/en/question/23982/how-to-disable-nouveau-in-fedora-18/>`_

Google Chrome 
-----------------
 
`Chrome <https://www.google.com/intl/en_in/chrome/browser/>`_
 
`Hangouts Plugin <https://www.google.com/tools/dlpage/hangoutplugin>`_

RPM Fusion
------------
 
::

  su -c 'yum localinstall --nogpgcheck http://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
  http://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm'


`RPMFusion <http://rpmfusion.org/Configuration>`_

Android
--------

- Install `Java`_


- Android SDK Dependencies

::

  sudo yum install glibc.i686 glibc-devel.i686 libstdc++.i686 zlib-devel.i686 \
  ncurses-devel.i686 libX11-devel.i686 libXrender.i686 libXrandr.i686 \
  mesa-libGL.i686

- Android Build Dependencies

::

  sudo yum install gcc gcc-c++ gperf flex bison glibc-devel.{x86_64,i686} \
  zlib-devel.{x86_64,i686} ncurses-devel.i686 readline-devel.i686 perl-Switch

- Set Android SDK Path

::
  
  ~/.bashrc
  -----------------------------------------------------
  PATH=$PATH:$HOME/AndroidSDK:$HOME/AndroidSDK/tools
  export PATH

  # For SDK version r_08 and higher, also add this for adb:
  PATH=$PATH:$HOME/AndroidSDK/platform-tools
  export PATH

- ``udev`` rule

::

  cd /etc/udev/rules.d
  wget https://raw.githubusercontent.com/M0Rf30/android-udev-rules/master/51-android.rules
  chmod a+r /etc/udev/rules.d/51-android.rules
  
`Fedora Wiki HOWTO_Setup_Android_Development <https://fedoraproject.org/wiki/HOWTO_Setup_Android_Development>`_

`Using Hardware Devices <http://developer.android.com/tools/device.html>`_

`MORf30 Github <https://github.com/M0Rf30/android-udev-rules/blob/master/51-android.rules>`_


Java
-----

- Install OpenJDK

::

  sudo yum install java-1.7.0-openjdk.x86_64 icedtea-web.x86_64


- Install Oracle Java 6

::

  sudo su
  sh jdk-6u45-linux-x64-rpm.bin
  

- Install Oracle Java 7

::
  
  sudo su
  rpm -ivh jdk-7u60-linux-x64.rpm
  
If upgrading

::
  
  rpm -Uvh jdk-7u60-linux-x64.rpm

- Set Java Path for JDK 6

::

  export JAVA_HOME=/usr/java/jdk1.6.0_45/
  export PATH=$JAVA_HOME/bin:$PATH
  
- Set Java Path for JDK 7

::
  
  export JAVA_HOME=/usr/java/default/
  export PATH=$JAVA_HOME/bin:$PATH
  
- Set Alternatives

::

  alternatives --install /usr/bin/java java /usr/java/default/jre/bin/java 200000
  alternatives --install /usr/bin/javaws javaws /usr/java/default/jre/bin/javaws 200000
  alternatives --install /usr/lib64/mozilla/plugins/libjavaplugin.so libjavaplugin.so.x86_64 /usr/java/default/jre/lib/amd64/libnpjp2.so 200000
  alternatives --install /usr/bin/javac javac /usr/java/default/bin/javac 200000
  alternatives --install /usr/bin/jar jar /usr/java/default/bin/jar 200000

  alternatives --config java
  alternatives --config javaws
  alternatives --config libjavaplugin.so.x86_64
  alternatives --config javac
  alternatives --config jar


`Oracle Docs <http://docs.oracle.com/javase/7/docs/webnotes/install/linux/linux-jdk.html#install-64-rpm>`_

`if-not-true-then-false.com <http://www.if-not-true-then-false.com/2010/install-sun-oracle-java-jdk-jre-7-on-fedora-centos-red-hat-rhel/>`_

`Fedora Forums <http://forums.fedoraforum.org/showthread.php?t=297016>`_

`John Goltzer Blogspot <http://johnglotzer.blogspot.in/2012/09/alternatives-install-gets-stuck-failed.html>`_


Thinkfan
---------

- Install and enable systemd file

::

  sudo yum install thinkfan
  sudo systemctl enable thinkfan

- Modify config and add output of following command to it prefixing with ``sensors``

::

  find /sys/devices -type f -name "temp*_input"
  
  /etc/thinkfan.conf
  ---------------------------------------------------------------
  sensor /sys/devices/virtual/hwmon/hwmon0/temp1_input
  sensor /sys/devices/platform/coretemp.0/hwmon/hwmon2/temp3_input
  sensor /sys/devices/platform/coretemp.0/hwmon/hwmon2/temp1_input
  sensor /sys/devices/platform/coretemp.0/hwmon/hwmon2/temp2_input
  

Media Codecs
------------

::

  sudo yum install -y amrnb amrwb faac faad2 flac gstreamer1-libav gstreamer1-plugins-bad-freeworld gstreamer1-plugins-ugly \
  gstreamer-ffmpeg gstreamer-plugins-bad-nonfree gstreamer-plugins-espeak gstreamer-plugins-fc gstreamer-plugins-ugly \
  gstreamer-rtsp lame libdca libmad libmatroska x264 xvidcore gstreamer1-plugins-bad-free gstreamer1-plugins-base \
  gstreamer1-plugins-good gstreamer-plugins-bad gstreamer-plugins-bad-free gstreamer-plugins-base gstreamer-plugins-good
  

`Fedy <https://github.com/satya164/fedy/blob/master/plugins/util/media_codecs.sh>`_


Bumblebee
-----------

::

   yum install libbsd-devel libbsd glibc-devel libX11-devel help2man autoconf git tar glib2 glib2-devel kernel-devel kernel-headers automake gcc gtk2-devel

   yum install VirtualGL

   yum install VirtualGL.i686

   yum install http://install.linux.ncsu.edu/pub/yum/itecs/public/bumblebee/fedora21/noarch/bumblebee-release-1.2-1.noarch.rpm

   yum install bbswitch bumblebee

   yum install http://install.linux.ncsu.edu/pub/yum/itecs/public/bumblebee-nonfree/fedora21/noarch/bumblebee-nonfree-release-1.2-1.noarch.rpm

   yum install bumblebee-nvidia

   yum install primus

   yum install primus.i686


Dropbox
--------

::
  
    cd ~ && wget -O - "https://www.dropbox.com/download?plat=lnx.x86_64" | tar xzf -
  ~/.dropbox-dist/dropboxd
  

ksuperkey
----------

- Installation

::
  
  sudo yum install git gcc make libX11-devel libXtst-devel pkgconfig
  git clone https://github.com/hanschen/ksuperkey.git
  cd ksuperkey
  make
  sudo make install
  
- Autostart

::

  ksuperkey -e 'Control_L=Escape;Super_L=Alt_L|F2'

`Github hanschen <https://github.com/hanschen/ksuperkey>`_

TLP
-------

- Configure Repo

::
  
  yum localinstall --nogpgcheck http://repo.linrunner.de/fedora/tlp/repos/releases/tlp-release-1.0-0.noarch.rpm
  yum localinstall --nogpgcheck http://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-stable.noarch.rpm

- Installation

::
  
  sudo yum install tlp tlp-rdw akmod-tp_smapi akmod-acpi_call kernel-devel

`Linrunner.de <http://linrunner.de/en/tlp/docs/tlp-linux-advanced-power-management.html#installation Linrunner>`_


VirtualBox
-----------

- Configure Repo

::

  cd /etc/yum.repos.d/
  wget http://download.virtualbox.org/virtualbox/rpm/fedora/virtualbox.repo
  
- Installation

::

  yum install binutils gcc make patch libgomp glibc-headers glibc-devel \
  kernel-headers kernel-devel dkms VirtualBox-4.3
  
- Setup

::

  /etc/init.d/vboxdrv setup
  usermod -a -G vboxusers $USER
  

`Fedoraonline.se <http://www.fedoraonline.se/install-oracle-vm-virtualbox-fedora-20/>`_

`Oracle <https://www.virtualbox.org/wiki/Linux_Downloads>`_


HandBrake 
------------

`Negativo17 HandBrake <http://negativo17.org/handbrake/>`_

Skype
-------

- 32-bit Libraries for 64-bit systems

::

  sudo yum -y install libXv.i686 libXScrnSaver.i686 qt.i686 qt-x11.i686 pulseaudio-libs.i686 \
  pulseaudio-libs-glib2.i686 alsa-plugins-pulseaudio.i686 qtwebkit.i686
  
- Follow Negativo17's post.

`Negativo17 Skype <http://negativo17.org/skype-and-skype-pidgin-plugin/>`_

`Skype.com <https://support.skype.com/en/faq/FA12120/getting-started-with-skype-for-linux>`_

RedShift Plasma Widget
----------------------

::

  sudo yum install kde-plasma-redshift


Dropbox-Dolphin Integration
---------------------------

::

  sudo yum install kde-baseapps-devel
  git clone git://anongit.kde.org/scratch/trichard/dolphin-box-plugin
  cd dolphin-box-plugin
  cmake -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_BUILD_TYPE=Release .
  make
  sudo make install


`trichard-kde.blogspot.in <http://trichard-kde.blogspot.in/2010/12/introducing-dropbox-integration-for.html>`_

`aur.archlinux.org <https://aur.archlinux.org/packages/do/dolphin-box-plugin-git/PKGBUILD AUR>`_

Caffeine
---------

`My blog <http://sudhirkhanger.com/2014/03/18/how-to-install-caffeine-in-fedora-20/>`_

`OBS zhonghuaren <http://software.opensuse.org/download.html?project=home%3Azhonghuaren&package=caffeine>`_

Gnome Encfs Manager
--------------------

::

  cd /etc/yum.repos.d/
  wget http://download.opensuse.org/repositories/home:moritzmolch:gencfsm/Fedora_20/home:moritzmolch:gencfsm.repo
  yum install gnome-encfs-manager

`Project Homepage <http://www.libertyzero.com/GEncfsM/>`_

`OBS mortizmolch <http://software.opensuse.org/download.html?project=home:moritzmolch:gencfsm&package=gnome-encfs-manager>`_



Genymotion
------------

::

  ./genymotion-2.2.1_x64.bin


SoundKonverter
--------------

`Github HessiJames <https://github.com/HessiJames/soundkonverter/wiki/Installing-soundKonverter#precompiled_packages>`_

reStructuredText
-----------------

::

  sudo yum install python-docutils python-sphinx
  
Microsoft Core Fonts
---------------------

::

    sudo yum install msttcore-fonts-installer-2.6-1.noarch.rpm
    
http://sourceforge.net/projects/mscorefonts2/?source=typ_redirect

Create A Yum Repository
------------------------

::

    yum install createrepo
    mkdir /path/to/repo
    createrepo --database /path/to/repo

Create a .repo file in /etc/yum.repos.d/

::

   nano _local.repo
   ---------------------
    [local]
    name=local Repository
    baseurl=http:/path/to/repo
    enabled=1
    
Speed up LibreOffice
---------------------
- Undo steps 20 or 30 steps
- Under Graphics cache, set Use for LibreOffice to 128 MB
- Set Memory per object to 20 MB (up from the default 5 MB).

wmsystemtray
--------------

::

  yum install wmsystemtray
  
- KWin Rules

::

  [Application settings for wmsystemtray]
  Description=Application settings for wmsystemtray
  desktop=-1
  desktoprule=2
  noborder=true
  noborderrule=2
  skippager=true
  skippagerrule=2
  skipswitcher=true
  skipswitcherrule=2
  skiptaskbar=true
  skiptaskbarrule=2
  type=2
  typerule=2
  wmclass=wmsystemtray0 wmsystemtray
  wmclasscomplete=true
  wmclassmatch=1

- Further tweaking
  - Uncheck Arrangement & Access > Skip Taskbar
  - Appearance & Fixes > Window Type > Force > Normal
  
- Autostart

::

  wmsystemtray --non-wmaker --bgcolor white

`Where Are My Systray Icons? <http://blog.martin-graesslin.com/blog/2014/06/where-are-my-systray-icons/>`_

`How to use KWin window rules for legacy system tray icons? <https://forum.kde.org/viewtopic.php?f=111&t=122722>`_