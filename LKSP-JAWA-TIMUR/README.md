<h1 align="center">Writeup LKSP JAWA TIMUR</h1>


- **Network**

> 📞 Low Energy Interruption
> Point 484
>My friend, Badarawuhi from SMKN Jawa Timur transferred me a file silently without me knowing what kind of file that was transferred to my Samsung phone from his laptop and he immediately cancelled it.
>
>Luckily, I have launched my special sniffing tools to capture those transfer history. Can you get that file? He says it contain a flag inside of it!
>
>Author: aseng
>
>View Hint
>You can see that from the source and the destination it should be Samsung and Liteon laptop right? **What's that in UIH Channel=12?**

The goal of this challenge is to retrieve the file content from the given packet traffic:

![](https://github.com/SaifulI57/writeup/blob/wumbo/LKSP-JAWA-TIMUR/files/1.png)

*We can see that traffic containing files will have data length between 500 and 1004 bytes*.

Add additional filter for a better look:

![](https://github.com/SaifulI57/writeup/blob/wumbo/LKSP-JAWA-TIMUR/files/2.png)

as you can see in the printable text of the first data.data, the file signature is jpg, and if we follow the standard JPEG, the file content will start with byte '0xFF 0xD8' and end with byte '0xFF 0xD9', we have to remove the first 33 bytes of the first data channel 12. And here is the command to retrieve the JPEG file using tshark:

```bash
tshark.exe -r transfer.pcapng -Y "btrfcomm.dlci == 0x18 and data.len > 741" -T fields -e data.data | tr -d "\n" | xxd -r -p | tail -c +33 > flag.jpg
```

The result image look like this:

![](https://github.com/SaifulI57/writeup/blob/wumbo/LKSP-JAWA-TIMUR/flag.jpg)

**Flag: `LKSJATIM{m1n1_8Lu3_t00th}`**
