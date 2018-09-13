### Solved by Swappage

x-y-z was another misc challenge from 0ops CTF.
We were provided with a txt file (a csv actually) containing a series of numbers.

```
-4.751373,-2.622809,2.428588;-4.435134,-3.046589,2.406030;-4.788052,-2.661979,2.464709
-4.692748,-2.599611,2.629112;-4.656070,-2.560445,2.592991;-4.788052,-2.661979,2.464709
-4.692748,-2.599611,2.629112;-4.788052,-2.661979,2.464709;-4.435134,-3.046589,2.406030
-4.656070,-2.560445,2.592991;-4.516017,-2.714652,2.570303;-4.751373,-2.622809,2.428588
-4.656070,-2.560445,2.592991;-4.751373,-2.622809,2.428588;-4.788052,-2.661979,2.464709
-4.611258,-2.777269,2.405960;-4.435134,-3.046589,2.406030;-4.751373,-2.622809,2.428588
-4.572725,-2.644557,2.333280;-4.603014,-2.680354,2.364417;-4.592222,-2.663824,2.351891
-4.571442,-2.773632,2.381504;-4.564917,-2.826000,2.397583;-4.611258,-2.777269,2.405960
-4.571436,-2.742115,2.369542;-4.571442,-2.773632,2.381504;-4.611258,-2.777269,2.405960
-4.571436,-2.742115,2.369542;-4.611258,-2.777269,2.405960;-4.567214,-2.723559,2.360054
...
```

We weren't given any hint except from the challenge name, but it was easly possible to guess that the numbers in the csv file were 3d coordinates, and that each line represented a triangle that we needed to plot in a 3d environment.

The correct printing would have probably returned the flag as a 3d image.

So it's

    x,y,z;x,y,z;x,y,z -> triangle

I'm not used to plotting, so i assembled this (horrible) python script by gathering examples and code snipplets from here and there.

```python
#!/usr/bin/python

import matplotlib
import matplotlib.pyplot as plt
from matplotlib.ticker import MaxNLocator
from matplotlib import cm
from mpl_toolkits.mplot3d import Axes3D

import numpy as np
from numpy.random import randn
from scipy import array, newaxis

DATA = []
triangles = []

counter = 0


with open("x-y-z") as f:
  lines = f.readlines()
  for line in lines:
    if counter < 5:
      #counter = counter + 1
      points = line.rstrip("\r\n").split(";")
      t = []
      for point in points:
        x = float(point.split(",")[0])
        y = float(point.split(",")[1])
        z = float(point.split(",")[2])
        p = [x, y, z]
        t.append(p)

      triangles.append(t)


Xs = []
Ys = []
Zs = []

for triangle in triangles:
  #print triangle
  verts = np.array(triangle)

  Xs.append(verts[:,0])
  Ys.append(verts[:,1])
  Zs.append(verts[:,2])


fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')

surf = []

for i in range(0, len(Xs)):
  surf.append(ax.plot_trisurf(Xs[i], Ys[i], Zs[i], cmap=cm.jet, linewidth=0))

  ax.xaxis.set_major_locator(MaxNLocator(5))
  ax.yaxis.set_major_locator(MaxNLocator(6))
  ax.zaxis.set_major_locator(MaxNLocator(5))


plt.show()
```

and to be honest i was quiet surprised myself when i figured out that it worked :)

![](/images/2015/0ctf/xyz/flag.png)

