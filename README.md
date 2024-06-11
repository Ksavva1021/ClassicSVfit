# ClassicSVfit
Latest version of SVfit_standalone algorithm

# Installation instructions with CMSSW
The SVfitPerformanceStudies package has been tested with CMSSW 9_4_4.
It depends on the following other packages:
	TauAnalysis/SVfitTF

In order to install the code, execute:

```
cmsrel CMSSW_9_4_4
cd CMSSW_9_4_4/src
cmsenv
git clone --recursive https://github.com/SVfit/ClassicSVfit TauAnalysis/ClassicSVfit -b  fastMTT_19_02_2019
git clone https://github.com/SVfit/SVfitTF TauAnalysis/SVfitTF
cd $CMSSW_BASE/src
scram b -j 4
```

In case of compilation problems, please sutmitt an issue on
https://github.com/SVfit/ClassicSVfit/issues

# Running instructions

- [Presentation, slides 2+3](https://indico.cern.ch/event/684622/contributions/2807248/attachments/1575090/2487044/presentation_tmuller.pdf)
- [Example(s)](https://github.com/SVfit/ClassicSVfit/blob/master/bin/testClassicSVfit.cc)


# Installation instructions without CMSSW
> Installation instructions:
It is possible to build the software without `CMSSW` framework if the following prerequisites are satisfied (oldest software version the instructions were tested with):
```bash
ROOT (6.10/3 or newer)
GCC (6.3 or newer)
```
In order to install the code, execute:
```bash
# git clone https://github.com/SVfit/ClassicSVfit TauAnalysis/ClassicSVfit -b fastMTT_19_02_2019
git clone https://github.com/oponcet/ClassicSVfit.git TauAnalysis/ClassicSVfit -b fastMTT_19_02_2019
# It will become the same as the above commented line after a PR
export LIBRARY_PATH=$LIBRARY_PATH:$PWD/TauAnalysis/ClassicSVfit/lib
make -f TauAnalysis/ClassicSVfit/Makefile -j4
```
You can try running SVFit with :
```bash
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$PWD/TauAnalysis/ClassicSVfit/lib
./TauAnalysis/ClassicSVfit/exec/testClassicSVfit
```
If everything goes right, this should produce a root file.

### `ClassicSVFit/FastMTT` in `Python` from C++ needs a *Wrapper* 🎁 


#### Installation of pybind11:

Tested with pybind11 v2.13.0 dev1.

Git clone pybind11 module if recursive mode is not used, and then compile: 

bash```
git submodule add git@github.com:pybind/pybind11.git

# to compile
mkdir build
cd build
cmake ..
make check -j 4
```


TThis wrapper has been developed with the purpose of using it with the [ColumnFlow](https://columnflow.readthedocs.io/en/latest/) columnar Python-based framework. In order to use `ClassicSVFit` in Python, the [pybind11](https://pybind11.readthedocs.io/en/stable/basics.html) wrapper has been used. The wrapper for the different classes can be found here : [pybind_wrapper.cpp](https://github.com/oponcet/ClassicSVfit/blob/fastMTT_19_02_2019/wrapper/pybind_wrapper.cpp).

The cloned `ClassicSVFit` already contains the `pybind11` and the `wrapper` itself. A few more things to modify [hard-coded, please be patient !!!]
```bash
open TauAnalysis/ClassicSVfit/wrapper/CMakeList.txt
change L10 and L33
```
Now, this wrapper needs to be compiled with :
```bash
export LIBRARY_PATH=$LIBRARY_PATH:$PWD/TauAnalysis/ClassicSVfit/lib
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$PWD/TauAnalysis/ClassicSVfit/lib
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/cvmfs/sft.cern.ch/lcg/app/releases/ROOT/6.30.04/x86_64-centosstream9-gcc113-opt/lib/
cmake -S TauAnalysis/ClassicSVfit/wrapper/ -B TauAnalysis/ClassicSVfit/wrapper/
make -C TauAnalysis/ClassicSVfit/wrapper/
```
It should produce a `.so` file which can be used as a module in Python. For example you can import it like :
```py
from modules.extern.TauAnalysis.ClassicSVfit.wrapper.pybind_wrapper import *
```


### Wrapper example :

You can run the example of fasttMTT, but before you need to export the right library like 

```bash
LD_LIBRARY_PATH=/usr/local/lib
CPPYY_BACKEND_LIBRARY=/cvmfs/sft.cern.ch/lcg/app/releases/ROOT/6.30.04/x86_64-centosstream9-gcc113-opt/lib/libcppyy_backend3_9.so
source /cvmfs/sft.cern.ch/lcg/app/releases/ROOT/6.30.04/x86_64-centosstream9-gcc113-opt/bin/thisroot.sh 
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:yourpath/TauAnalysis/ClassicSVfit/lib
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/cvmfs/sft.cern.ch/lcg/app/releases/ROOT/6.30.04/x86_64-centosstream9-gcc113-opt/lib/
export LIBRARY_PATH=$LIBRARY_PATH:$PWD/TauAnalysis/ClassicSVfit/lib
```

The try running the python example `[`svfit_wrapper_example.py`](wrapper/svfit_wrapper_example.py)`:

```bash
python3 svfit_wrapper_example.py
```

>output should looks like: 
```bash
tau1P4mtt : Name: TLorentzVector Title: A four vector with (-,-,-,+) metric
tau1P4mtt : Name: TLorentzVector Title: A four vector with (-,-,-,+) metric
ditauMass : 105.58224459511314
```


