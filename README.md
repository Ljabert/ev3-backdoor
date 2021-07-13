# `ash` backdoor for LEGO® MINDSTORMS® EV3

## Why?

This is an alternative to the [EV3 cli](https://github.com/Ljabert/ev3-cli) using a network connection, so you don't have to plug your EV3 to your pc to execute commands.

As far as I am aware there is no ssh server installed on the EV3 by default, so the next best thing is just opening a port and piping all the data to `ash`. That is of course not very secure, as I didn't implement any login or validation, so please for the love of god don't run this on a public network if you dont want your fancy LEGO® robot hacked.

---

## How?

By using [nc](https://en.wikipedia.org/wiki/Netcat) and redirecting all data from port `1337` to `ash` it is now possible to execute commands remotely without recompiling the program.

### Preparation

Before executing any commands or running the program on your EV3, you should make sure the robot is connected to your network.

You can connect your EV3 over Wi-Fi using a supported adapter and the internal EV3 options menu.

I prefer connecting using a USB to LAN adapter. Make sure to restart the EV3 after plugging in the network cable.

Of course, you will need to have the software, taken from either the releases or manually compiled, installed on your brick, to run it. 

### Linux

To connect to the EV3 just use the (hopefully) preinstalled `nc` command:

#### Syntax:
```sh
echo "<your command here>" | nc <your EV3 IP> 1337 -N
```

#### Example:
```sh
echo "ls -lah /" | nc 192.168.0.95 1337 -N
```

Make sure not to forget the `-N`.

### Windows

Windows does not come preinstalled with a netcat command, so i recommend installing and using [ncat](https://nmap.org/ncat/).

#### Syntax:
```sh
<NUL set /p="<your command here>" | ncat <your EV3 IP> 1337
```

#### Example:
```sh
<NUL set /p="ls -lah /bin" | ncat 192.168.0.95 1337
```

### macOS

no idea, figure it out ¯\\\_(ツ)\_/¯

### Closing the backdoor

To close the program simply press any of the EV3 buttons.

This will break the loop, ending the program.

---

## Building from source

I am using [EV3Basic](https://github.com/c0pperdragon/EV3Basic), a Smallbasic dialect that can be compiled directly to `RBF`-files to run on your robot.

This `RBF`-file simply needs to be copied onto your EV3 in the `/home/root/lms2012/prjs/backdoor/` folder using the EV3Explorer bundled with EV3Basic and can then be executed from the EV3 programs-menu.
