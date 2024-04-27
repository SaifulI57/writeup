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

The goal of this challange is to retrieve the file contents from package traffic given:

![](https://github.com/SaifulI57/writeup/blob/wumbo/LKSP-JAWA-TIMUR/files/1.png)

*we can see that the traffic contains file will have data len between 500 to 1004 bytes*

Add additional filter for better look:

![](https://github.com/SaifulI57/writeup/blob/wumbo/LKSP-JAWA-TIMUR/files/2.png)

as you can see in the printable text from the first data.data the file signature is jpg, and if we follow standard JPEG the content of file will start with the bytes '0xFF 0xD8' and end with the bytes '0xFF 0xD9', then it will need to remove the first 33 bytes from the first data channel 12:

```bash
tshark.exe -r transfer.pcapng -Y "btrfcomm.dlci == 0x18 and data.len > 741" -T fields -e data.data | tr -d "\n" | xxd -r -p | tail -c +33 > flag.jpg
```

![](https://github.com/SaifulI57/writeup/blob/wumbo/LKSP-JAWA-TIMUR/flag.png)

**Flag: `LKSJATIM{m1n1_8Lu3_t00th}`**
