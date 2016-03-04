#Running Headless VMs with Fusion8


Most of the code taken and inspired from [https://gist.github.com/jasoncodes/613198](https://gist.github.com/jasoncodes/613198)

Just Added some checks to wait for the Disc contening the VM(s) to be mounted first.

##Clone the repo.

```
git clone https://github.com/oemden/Headless_VMware
cd Headless_VMware
```

##Edit the files

###The Script

Edit the variables.

I like to name the script like the VM name - Here the VM name is **VM1**.

```
VMNAME="VM1" # Name fo the VM.
HDDNAME="DATAS" # Name of the Volume where the VM is.
TRYOUTS=2 #Edit the trying counts before giving up.

VMHDD="/Volumes/${HDDNAME}" ## The Volume where the VM is - Needed to check the disc is mounted. 
VMX="$VMHDD/VMs/${VMNAME}/${VMNAME}.vmwarevm/${VMNAME}.vmx" ## Edit according to your path
```

###The Agent

You'll have to edit the .plist accordingly to your needs.

As any launchd .plist, the **file Name** as to be the same as the key **label** `com.oemden.headless.VM1`

Edit the .plist file and replace `VM1_headless` with the name you gave of the above script.


###To install your HeadlessVM daemon.

- **Create an Alias of vmrun for ease of use:**

`ln -s "/Applications/VMware\ Fusion.app/Contents/Library/vmrun" /usr/local/bin/vmrun`

- **Make the file executable**
`chmod +x VM1_vmdaemon`

- **Create the executable script**
`sudo mkdir -p "/Library/Application Support/VMwareHeadless/"`

- **Copy the script**
`sudo cp ./VM1_vmdaemon "/Library/Application Support/VMwareHeadless/"`

- **Copy the Launchd**
`sudo cp ./com.oemden.headless.VM1.plist "/Library/LaunchDaemon/com.oemden.headless.VM1.plist"`

- **Launch the Daemon**
`sudo launchtl load -w /Library/LaunchDaemon/com.oemden.headless.VM1.plist`


Repeat for each VM.


###Headless script
Edit the `VM1_vmdaemon` executable and place it in : `/Library/Application Support/VMwareHeadless/`

###LaunchAgent
Edit the Launchd plist `com.oemden.headless.VM1.plist`

Replace **VM1** with your VM Name or 

###Kill Fusion GUI

Fusion May launch or you may need to launch Fusion8 to ponctually run another VM.

Save this as an Applescript :

```
set KillVMWare to "killall 'VMware Fusion'"
do shell script KillVMWare
```

and put it in the `/Library/Scripts` so you'll have it in the Menu Bar


####TODOs

Succeed to run all of this on the LoginWindow.

Note : I'd prefer it to be a Daemon but for now it is an agent.

