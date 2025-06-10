# Operation Orchid

> Category: Forensics

## Description

Download this disk image and find the flag.

Note: if you are using the webshell, download and extract the disk image into `/tmp` not your home directory.
* [Download compressed disk image](https://artifacts.picoctf.net/c/212/disk.flag.img.gz)


## Solution

1. Download the provided disk image file: `wget https://artifacts.picoctf.net/c/212/disk.flag.img.gz`

2. It's compressed with `gzip`, so we decompress it with `gunzip`: `gunzip disk.flag.img.gz`

3. Let's take a look at the filetype with the `file` command

    `$ file disk.flag.img`

    `disk.flag.img: DOS/MBR boot sector; partition 1 : ID=0x83, active, start-CHS (0x0,32,33), end-CHS (0xc,223,19), startsector 2048, 204800 sectors; partition 2 : ID=0x82, start-CHS (0xc,223,20), end-CHS (0x19,159,6), startsector 206848, 204800 sectors; partition 3 : ID=0x83, start-CHS (0x19,159,7), end-CHS (0x32,253,11), startsector 411648, 407552 sectors`

    I wasn't really sure what a boot sector was, but after doing a little bit of googling, I learned that the MBR (master boot record) boot sector is the first sector (usually the first 512 bytes) of a storage device that tells the computer how to load the operating system. 

    The boot sector contains:
    1. Instructions on how to load the OS into memory
    2. Partition table (how the drive is organized)

    Knowing that, we can see that the `disk.flag.img` disk image has 3 partitions.

    * Partition 1:
        - starts at sector 2048
        - spans 204800 sectors
        - Marked as `Active` (bootable), meaning this is the partition the MBR loads and executes to start the operating system
    * Partition 2:
        - starts at sector 206848
        - spans 204800 sectors
        - ID=0x82, meaning it's a *Linux swap partition* (virtual memory) that is used when system RAM is full
    * Partition 3:
        - starts at sector 411648
        - spans 407552 sectors
        - ID=0x83, meaning it's a Linux native filesystem

    Out of the 3 partitions we have, I think it's safe to assume that the flag is somewhere in `partition 3`, since partition 1 is used to start up the OS and partition 2 is virtual memory space. Another hint would be the fact that partition 3 takes up the most space.


4. Now we need to mount partition 3 and see what's inside

    * [This stackexchange thread](https://unix.stackexchange.com/questions/316401/how-to-mount-a-disk-image-from-the-command-line) explains how to mount disk image files using the `mount` command

    * Since we want to mount partition 3, we will use the starting sector `411648` times the sector size of `512` to determine the offset since the `mount` command needs the offset in bytes

        `$ sudo mount -o loop,offset=$((411648*512)) disk.flag.img /mnt/`

    * Now we can see the mounted filesystem inside `/mnt/`.

        ```sh
        $ cd /mnt
        $ ls -la
        total 44
        drwxr-xr-x 22 root root  1024 Oct  6  2021 .
        drwxr-xr-x 19 root root  4096 Jun  5 18:37 ..
        drwxr-xr-x  2 root root  3072 Oct  6  2021 bin
        drwxr-xr-x  2 root root  1024 Oct  6  2021 boot
        drwxr-xr-x  2 root root  1024 Oct  6  2021 dev
        drwxr-xr-x 27 root root  3072 Oct  6  2021 etc
        drwxr-xr-x  2 root root  1024 Oct  6  2021 home
        drwxr-xr-x  9 root root  1024 Oct  6  2021 lib
        drwx------  2 root root 12288 Oct  6  2021 lost+found
        drwxr-xr-x  5 root root  1024 Oct  6  2021 media
        drwxr-xr-x  2 root root  1024 Oct  6  2021 mnt
        drwxr-xr-x  2 root root  1024 Oct  6  2021 opt
        drwxr-xr-x  2 root root  1024 Oct  6  2021 proc
        drwx------  2 root root  1024 Oct  6  2021 root
        drwxr-xr-x  2 root root  1024 Oct  6  2021 run
        drwxr-xr-x  2 root root  5120 Oct  6  2021 sbin
        drwxr-xr-x  2 root root  1024 Oct  6  2021 srv
        drwxr-xr-x  2 root root  1024 Oct  6  2021 swap
        drwxr-xr-x  2 root root  1024 Oct  6  2021 sys
        drwxrwxrwt  4 root root  1024 Oct  6  2021 tmp
        drwxr-xr-x  8 root root  1024 Oct  6  2021 usr
        drwxr-xr-x 11 root root  1024 Oct  6  2021 var
        ```

    * The most interesting directory is `/root`, but as we can see from the permissions, we need `sudo` permissions to access it. Let's change the directory permissions like so

        `$ sudo chmod 755 root`

    * Now we can `cd` into it just fine. 

        ```sh
        $ cd root
        $ ls -la
        total 4
        drwxr-xr-x  2 root root 1024 Oct  6  2021 .
        drwxr-xr-x 22 root root 1024 Oct  6  2021 ..
        -rw-------  1 root root  202 Oct  6  2021 .ash_history
        -rw-r--r--  1 root root   64 Oct  6  2021 flag.txt.enc
        ```

    * As we can see, there's an encrypted `flag.txt.enc` file as well as an `.ash_history` file

5. The only thing we need now is the decryption key so we can read what's inside `flag.txt.enc`. This is where the `.ash_history` file comes into play

    * Running `sudo cat .ash_history` show us the following

        ```sh
        touch flag.txt
        nano flag.txt 
        apk get nano
        apk --help
        apk add nano
        nano flag.txt 
        openssl
        openssl aes256 -salt -in flag.txt -out flag.txt.enc -k unbreakablepassword1234567
        shred -u flag.txt
        ls -al
        halt
        ```

    * As we can see, the file was encrypted with aes256 and the key is `unbreakablepassword1234567`

6. Decrypting the flag file from here is pretty simple

    `$ openssl aes256 -d -in flag.txt.enc -k unbreakablepassword1234567`


### Flag 

> picoCTF{h4un71ng_p457_0a710765}
