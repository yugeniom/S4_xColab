-------------------
HOWTO install S4 permanently on Google Drive and run it in Cloud with Colab Notebook (~Jupyter notebook) on free Google VMs 

1) Create the "S4all" program folder on your Gdrive and a "libraries" subfolder. Please use these exact names; if you want to change the folder names, you will have to edit the Makefile)

2) Open an empty Colab notebook, type this and run (Google will ask your permission to access your Drive, grant it):
```
from google.colab import drive
import os, sys
drive.mount('/content/gdrive')
%cd /content/gdrive/MyDrive/S4all
lib_path = '/content/gdrive/MyDrive/S4all/libraries'
sys.path.insert(0,lib_path)

!apt install make git gcc g++ gfortran
!apt-get install libopenblas-dev libfftw3-dev libsuitesparse-dev
!pip3 install wheel setuptools numpy scipy matplotlib
```

2) Clone this repository, typing in a new cell:
```
git clone https://github.com/yugeniom/S4_xColab.git
```
3) cd into the S4_xColab folder and compile it all (please grab a drink, this will take "some" time):
```
%cd S4_xColab
!make boost
!make S4_pyext
```
4) Create a new file named "S4.py" with the following content, then upload it into the folder "libraries":

```
def __bootstrap__():
   global __bootstrap__, __loader__, __file__
   import sys, pkg_resources, imp
   __file__ = pkg_resources.resource_filename(__name__,'S4.cpython-38-x86_64-linux-gnu.so')
   __loader__ = None; del __bootstrap__, __loader__
   imp.load_dynamic(__name__,__file__)
__bootstrap__()
```

5) S4_xColab is now permanently installed on your Google Drive. To create a simulation, create a new Colab Notebook with the following heading cell, followed by your own simulation code:
```
from google.colab import drive
import sys
!pip install --force-reinstall scipy==1.12.0
import numpy as np
print(np.__version__)
if int(np.__version__[0]) > 1:
  import os
  os.kill(os.getpid(), 9)
drive.mount('/content/gdrive')
%cd /content/gdrive/MyDrive/S4all
sys.path.append('/content/gdrive/MyDrive/S4all/libraries/')
!apt-get install libopenblas-dev libfftw3-dev libsuitesparse-dev

import S4
```
5bis) Mind that your will be asked to grant access to GDrive and to reinstall blas and fftw libraries every time you disconnect your Colab Notebook; this normally happens automatically after a few minutes of inactivity, so you'd better trick Google into thinking that you are always active while the Notebook is open; you can do that adding this line in a cell following your whole code:
```
while True:pass
```






