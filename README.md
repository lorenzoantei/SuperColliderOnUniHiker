# SuperCollider Software | UniHiker board
The DFRobot UniHiker Development board does not come with audio IN/OUT, but if you have a USB-A Audio adapter lying around (shown below), please follow the instructions on this page.

<img src="https://github.com/ErikOostveen/SuperColliderOnUniHiker/assets/40121318/bf1fda80-4886-407f-ab96-9014c3f05585" width="225" />


Install the SuperCollider audio synthesis software on your DFRobot UniHiker development board:

initiate a SSH session to your DFRobot Development Board.

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

Login again through SSH  ...

```
cd ~ 
git clone --recurse-submodules https://github.com/supercollider/supercollider.git
cd supercollider
mkdir build && cd build

cmake -DCMAKE_BUILD_TYPE=Release -DSUPERNOVA=OFF -DSC_ED=OFF -DSC_EL=OFF -DSC_VIM=ON -DNATIVE=ON -DSC_IDE=OFF -DNO_X11=ON -DSC_QT=OFF ..

cmake --build . --config Release --target all --  
```

NOTE: The last cmake command will take a good hour or so ...

```
sudo cmake --build . --config Release --target install
sudo ldconfig

echo /usr/bin/jackd -P75 -p16 -dalsa -dhw:2 -r44100 -p1024 -n3 > ~/.jackdrc
```
... or (alternative):
```
echo /usr/bin/jackd -R -P 75 -T -d alsa -d hw:2 -r 44100 -p 2048 -n 3 > ~/.jackdrc
```
Notes: 
use "nano ~/.jackdrc" to further edit jack settings, if need be. dhw:2 is the USB audio adapter

# Time to play!

Create a file - for example:

```
sudo nano demoSound.scd
```

Copy/Paste the code below - and save this file

```
s.waitForBoot{
 (
  play{a=ar(PinkNoise,5e-4);ar(GVerb,({ar(Ringz,a*LFNoise1.kr(0.2),exprand(60,8000),3)}!40).sum,50,99).tanh}
 )
}
```

Play this cinematic drone by entering the following:

```
sclang demoSound.scd
```

Ctrl + C will take you back to the prompt.

Try changing values in the demoSound.scd file and listen to the drone again.

Here are a few sources to get you into SuperCollider:

https://supercollider.github.io/
<br/><br/>
https://sccode.org/1-5eN
<br/><br/>
https://w2.mat.ucsb.edu/l.putnam/sc3one/index.html
