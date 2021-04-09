# Libc.so.6
> Fupping */lib/libc.so.6

I accidentally deleted `/usr/lib/libc.so.6` while working on my [HackPack](https://github.com/owenCocjin/HackPack) project. This library is used by literally any default/standard binary that isn't a bash builtin.

#### Bash Builtins:
> These are only the builtins that I think are the most important. Check `man bash` in the <b>SHELL BUILTIN COMMANDS</b> section for a list of all of them.
- `source`
- `alias`
- `cd`
- `echo`
- `exit`
- `export`
- `kill`
- `logout`
- `pwd`

#### NOT Bash Builtins:
> You're gonna be messed up without these commands
- `cp`
- `ln`
- `ls`
- `mkdir`
- `nano`
- `sudo`

## Problem
The issue is most all binaries will use `libc.so.6`, and would be required to re-create the file. `libc.so.6` is a symbolic link to another library (at the writing of this post it points to `/usr/lib/libc-2.33.so`). This is good news because it makes fixing a bit easier as you won't need to source a new file!

![libc.so.6 link](https://github.com/owenCocjin/Linux-Fups/raw/master/Fups/Libc.so.6/Pics/liblink.png "libc.so.6 link")

The bad news is you need the library to create a sym link. The other bad news is unless you like running everything as root, you probably aren't root which is required to write in your lib directory. To run a command as root you would use `sudo` which requires the missing library.

## Solution
Essentially what I'm saying is to reinstate the lib you're gonna need to use a live USB!

Assuming you've already got a live USB kickin' around, this is how you fix it:

1. Boot to your live USB. Use `lsblk` to list your connected devices. Try to determine which is your main/bricked drive. It won't be the device with anything mounted on `/home`, `/`, etc....

2. Determine which partition is your `root` partition. This is where the libs are gonna be stored. For me, I always give myself plenty of `/home` storage and about half the `/` storage, so for me my root partition is gonna be the somewhat smaller partition.

![lsblk command](https://github.com/owenCocjin/Linux-Fups/raw/master/Fups/Libc.so.6/Pics/lsblk.png "lsblk")

<i>I've intentionally hid the mountpoints. Here I know my root partition is on /dev/sda2 because it is one of the bigger partitions, but is half the size of /dev/sda4.</i>

3. Mount the root partition. Normally you can mount it in /mnt. Move into /mnt. <b>NOTE:</b> You won't need to chroot into your partition. In fact, it probably won't even work.

4. Determine where your libraries are stored. For me it was /usr/lib/. Create a symlink FROM /usr/lib/libc-2.33.so TO /usr/lib:
```
sudo ln -s /mnt/usr/lib/libc-2.33.so /mnt/usr/lib/libc.so.6
```

5. Move out of and unmount /mnt and reboot without the live USB! Done done!

## Additional
I've added a copy of libc-2.33.so just in case
