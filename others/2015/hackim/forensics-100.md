### Solved by Swappage

This is the first and supposely easier forensics problem in the NullCon CTF 2015

![](/images/2015/hackim/forensics100/problem.png)

we were provided with a pcap file, and were asked to identify the hideout of a wanted suspect.

I started looking at the pcap file in wireshark, and noticed that there were a bunch of HTTP sessions, at this point i decided to load the pcap file in xplico to make my life easier and to reconstruct the web pages.

![](/images/2015/hackim/forensics100/urls.png)

By opening the url

    http://10.20.31.84/locationlogs/

we can notice that it's an http server directory listing and that there are a bunch of files in there.

![](/images/2015/hackim/forensics100/locationlogs_index.png)

I proceded to save the files on disk and noticed that they were a mix of KML and KMZ files that i could import in google earth.

Many of them were simply a disguise, we were asked to look for a cave, and obviously there was a file *dams_forests_caves* to attract our attention.

I spent a couple of time trying to figure out which files were there just to increase the *noise* and which one was the really interesting one.

Since the problem statement said something about monitoring the movements of the suspect, eventually the *trace log* caught my attention.

It's definitely a KML file, but it's intentionally broken as it's missing the XML headers needed for the KML specification.

```xml
    <?xml version="1.0" encoding="utf-8"?>
      <Document>
        <name>Timetimetime</name>
        <Style id="paddle-a">
          <IconStyle>
            <Icon>
              <href>http://maps.google.com/mapfiles/kml/paddle/A.png</href>
            </Icon>
            <hotSpot x="32" y="1" xunits="pixels" yunits="pixels" />
          </IconStyle>
        </Style>
    ...
```
I quickly fixed it and tried to import it in google Earth for further inspection.

I've added the appropriate headers to the file:

```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <kml xmlns="http://www.opengis.net/kml/2.2" xmlns:gx="http://www.google.com/kml/ext/2.2" xmlns:kml="http://www.opengis.net/kml/2.2" xmlns:atom="http://www.w3.org/2005/Atom">
```

and what i got as result is the following:

![](/images/2015/hackim/forensics100/tracelog.png)

That placeholder i've marked in red seemed really suspicious, and in fact, if you look at the picture that google earth shows nearby we can see

![](/images/2015/hackim/forensics100/cave.png)

the flag is

    flag{md5("pirates cave")}
