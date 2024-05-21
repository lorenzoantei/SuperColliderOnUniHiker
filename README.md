# SuperCollider Software | UniHiker board
Install the SuperCollider audio synthesis software on your DFRobot UniHiker development board

initiate a ssh session to your DFRobot Development Board.

Follow the steps below:
```
sudo apt remove '*jack*'
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade
cd ~ 
git clone https://github.com/jackaudio/jack2.git 
cd jack2 
./waf configure --alsa --prefix=/usr --libdir=/usr/lib/x86_64-linux-gnu 
./waf 
su -c "./waf install" 
sudo ldconfig 
sudo apt-get install jackd (Select the onscreen "Yes")
apt-get install libjack-jackd2-dev 
```

At this point entering "jackd" should output its normal usage menu and "apt list --installed | grep jack" should return the following:

```
 jackd2/oldoldstable,now 1.9.12~dfsg-2 arm64 [installed,automatic]
 jackd/oldoldstable,now 5+nmu1 all [installed]
 libjack-jackd2-0/oldoldstable,now 1.9.12~dfsg-2 arm64 [installed,automatic]
 qjackctl/oldoldstable,now 0.5.0-1 arm64 [installed,automatic]
```

 ...or...

```
jackd2/oldoldstable,now 1.9.12~dfsg-2 arm64 [installed,automatic]
jackd/oldoldstable,now 5+nmu1 all [installed]
libjack-jackd2-0/oldoldstable,now 1.9.12~dfsg-2 arm64 [installed,automatic]
libjack-jackd2-dev/oldoldstable,now 1.9.12~dfsg-2 arm64 [installed]
qjackctl/oldoldstable,now 0.5.0-1 arm64 [installed,automatic]
```

Now, carry on:

```
sudo apt-get install libsamplerate0-dev libsndfile1-dev libasound2-dev libavahi-client-dev libreadline-dev libfftw3-dev libudev-dev libncurses5-dev cmake git

sudo sh -c "echo @audio - memlock 256000 >> /etc/security/limits.conf"
sudo sh -c "echo @audio - rtprio 75 >> /etc/security/limits.conf"

sudo reboot now
```

# Login again ...

```
cd ~ 
git clone --recurse-submodules https://github.com/supercollider/supercollider.git
cd supercollider
mkdir build && cd build

cmake -DCMAKE_BUILD_TYPE=Release -DSUPERNOVA=OFF -DSC_ED=OFF -DSC_EL=OFF -DSC_VIM=ON -DNATIVE=ON -DSC_IDE=OFF -DNO_X11=ON -DSC_QT=OFF ..

cmake --build . --config Release --target all --  
```

# The above command will take a good hour or so ...
