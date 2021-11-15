# shadysim

Reproducing the SIM Toolkit research from Dieter Spaar from https://osmocom.org/projects/cellular-infrastructure/wiki/Shadysimpy using what I found in my drawer in 2021:
- docker
- PCSC reader 
- sysmoUSIM-SJS1 (pink/orange ones, does not work with the SJA2 bc of the old pysim used underneath, I think)
- linux desktop (for exposing USB PCSC reader in docker), but a VM on Mac is certainly doable with some adaptation of the underlying VM

## how to build/run

This will build the STKApplet witin Docker and prepare the python run time environment. 
You need to pcsc reader to be operational on your linux already (like pcsc_scan needs to detect your SIM Card).

```
docker build -t shadysimpy  .
docker run -v /dev/usb:/dev/usb --privileged --rm -it shadysimpy:latest --pcsc
```

typical argument to flash a card:
```
# flash the applet 
docker run -v /dev/usb:/dev/usb --privileged --rm -it shadysimpy:latest --pcsc  -l HelloSTK.cap -i HelloSTK.cap  --enable-sim-toolkit --module-aid d07002ca44900101 --instance-aid d07002CA44900101 --nonvolatile-memory-required 0100  --volatile-memory-for-install 0100 --max-menu-entry-text 15  --max-menu-entries 05 --kic YOURKIC1HERE --kid YOURKID1HERE  
# clean the applet from the SIM
docker run -v /dev/usb:/dev/usb --privileged --rm -it shadysimpy:latest --pcsc -d d07002CA449001 --kic YOURKIC1HERE --kid YOURKID1HERE
```

On my old nexus 5 and after a reboot, the phone detects the STK app and display a nice helloworld in it, and I could reproduce the error message from the wiki
when trying to reflash the applet already present on the card.

## identified issues

- code base no more updated
- no support for newest sysmosim card
- python2 
- no true SMPP or SMS API support to send this remotely




