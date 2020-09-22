---
title: Setup
---

Setup for TIMBER on lxplus

```bash
cmsrel CMSSW_11_0_1
cd CMSSW_11_0_1/src
cmsenv
virtualenv timber-env
source timber-env/bin/activate
git clone https://github.com/lcorcodilos/TIMBER.git
cd TIMBER
python setup.py install
cd ..
```

Test command for TIMBER/b* selection
```bash
git clone https://github.com/lcorcodilos/BstarToTW_CMSDAS2020.git
cd BstarToTW_CMSDAS2020
python bs_select.py -i /eos/home-l/lcorcodi/Storage/rootfiles/BprimeLH1200_bstar16.root -y 16
```

Setup for 2D Alphabet on lxplus 
```bash
export SCRAM_ARCH=slc7_amd64_gcc700
cmsrel CMSSW_10_6_14
cd CMSSW_10_6_14/src
cmsenv
git clone https://github.com/lcorcodilos/2DAlphabet.git
git clone https://github.com/lcorcodilos/HiggsAnalysis-CombinedLimit.git HiggsAnalysis/CombinedLimit/
bash <(curl -s https://raw.githubusercontent.com/lcorcodilos/CombineHarvester/master/CombineTools/scripts/sparse-checkout-ssh.sh)
scram b clean; scram b -j 10
cmsenv
```

Test command for 2D Alphabet
```bash
cd 2DAlphabet
python run_MLfit.py unit_tests/configs/input_lin100kv10k.json --tag=test
```
