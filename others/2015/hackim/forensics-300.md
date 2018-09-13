###Solved by bitvijays

>Find the hidden key

Flag format: flag{flag}

We are provided with a file FOR-300. 

Let's check with file command what's the filetype.
```
file FOR-300 
FOR-300: Linux Compressed ROM File System data, little endian size 9367552 version #2 sorted_dirs CRC 0x8d1dae50, edition 0, 2795 blocks, 4298 files
```

Let's mount it 
```
mount /home/bitvijays/Desktop/CTF/Nullcon/forensics/FOR-300 /media/bitvijays/
mount: warning: /media/bitvijays/ seems to be mounted read-only.
```

We are provided with a lot of random file names with lot of empty files and directories. Let's copy the contents just to a different location so that we can delete the empty files/directories.

Deleted empty file and directories with
```
find -empty -type d -delete 
find -empty -type f -delete
```

Garbage got removed from 
![](/images/2015/hackim/forensics300/for3001.png)
to 
![](/images/2015/hackim/forensics300/for3002.png)

ran2.sh looked interested and had a entry "FILEEXT=".JPG"", May be our flag is jpg.
```
find . -type f -exec file {} + | grep JPEG
./poiuy7Xdb/7yknXuW/VXIXNxl/wmDKAM 1/lkjhwerle.jpg:                             JPEG image data, JFIF standard 1.01

//Filename contains a space so we have to unescape it
display ./poiuy7Xdb/7yknXuW/VXIXNxl/wmDKAM\ 1/lkjhwerle.jpg
```
![](/images/2015/hackim/forensics300/for3003.jpg)

The flag is **flag{f0rens!cs!sC00L}**

