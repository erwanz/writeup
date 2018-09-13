### Solved by Swappage

in *Peers* we were given a pcap file, and our objective was to extract the flag from it

By looking at the traffic in wireshark, and examining the streams, it really looked obvious that
it was bittorrent unencrypted traffic.

![](/images/2015/0ctf/peers/tcpstream.png)

Wireshark/Tshark has a dissector for the bittorrent protocol, but in this specific case it
wasn't detecting the protocol correctly because the communication was running on port 80 (HTTP),
while the dissector expects the bittorrent traffic to be on standard ports (like 6881).

To resolve this little issue, i've fixed the pcap file using tcprewrite, to rewrite all the
tcp connections on port 80 to 6881.

    # tcprewrite --infile=peers.pcap --outfile=fixed.pcap --portmap 80:6881

As a result, we have a wonderful pcap file containing the bittorrent traffic, correctly dissected
by wireshark.

![](/images/2015/0ctf/peers/dissected.png)

What was left to do was to extract the payload and reassemble it.

Using tshark:

    tshark -r fixed.pcap -R 'bittorrent.piece.data' -Tfields -e bittorrent.piece.index -e bittorrent.piece.data > pieces

we extract the file chunks (parts) in the same order as they were received during the file transfer:

```
0x00000000      42:4d:36:c4:09:00:00:00...
0x00000027      bf:ff:82:59:50:ff:9b:6c...
0x00000020      69:ff:ae:84:78:ff:a6:76...
0x00000017      5b:ff:96:66:5a:ff:9a:6a...
0x00000004      4f:ff:b1:87:7a:ff:a4:77...
...
```

but due to the nature of the protocol, they are not in the correct order, as bittorrent splits the file in chunks and the client receives them in a non particular order.

With the help of this python script, we can take the chunks and reassemble them in the correct order to build the file.

```python
#!/usr/bin/python2
pieces = {}

for line in open('pieces'):
    line = line.strip()

    idx, data = line.split('\t')
    data = data.replace(':','').decode('hex')

    try:
        pieces[idx] += data
    except KeyError:
        pieces[idx] = data

pieces = sorted([(int(p[0], 16), p[1]) for p in pieces.items()])

data = ''.join([p[1] for p in pieces])
open('torrent.out', 'wb').write(data)
```

The result is a bitmap image

    # file torrent.out
    torrent.out: PC bitmap, Windows 3.x format, 800 x 200 x 32

that contains the flag

![](/images/2015/0ctf/peers/flag.png)

