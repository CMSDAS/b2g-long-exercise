---
title: Setup
---

## Logging in and creating a working directory
```bash
ssh -Y <username>@lxplus.cern.ch
# You can also log into the CMSLPC with ssh -Y <username>@cmslpc-sl7.fnal.gov
mkdir ~/CMSVDAS2020
# At the LPC you can use the ~/nobackup/ space 
voms-proxy-init --voms cms #It's best to have all your credentials ready
```


## Create a public/private key on lxplus for use with GitHub.
### WARNING: Never put your private key `id_rsa` in any public space!
```bash
ssh-keygen
# enter optional file name and password
# press enter with empty to create the key without a password and default name
cat ~/.ssh/ida_rsa.pub #this is your public key
```
Now copy the public key content and store it on GitHub under "settings -> SSH and GPG keys"


## Setup for TIMBER on lxplus (Should also work on CMSLPC)
```bash
cd ~/CMSVDAS2020
export SCRAM_ARCH=slc7_amd64_gcc820
cmsrel CMSSW_11_0_1 #Only need to do this once
cd CMSSW_11_0_1/src
cmsenv
virtualenv timber-env
source timber-env/bin/activate
git clone https://github.com/lcorcodilos/TIMBER.git #Only need to do this once
cd TIMBER
python setup.py install
cd ..
```

### Test command for TIMBER/b* selection
```bash
git clone https://github.com/lcorcodilos/BstarToTW_CMSDAS2020.git #Only need to do this once
cd BstarToTW_CMSDAS2020
python bs_select.py -i /eos/user/c/cmsdas/long-exercises/bstarToTW/rootfiles/signalLH1200_bstar16.root -y 16
# Also possible from lxplus, replace sample path with root://cmsxrootd.fnal.gov//store/user/lcorcodi/bstar_nano/rootfiles/BprimeLH1200_bstar16.root
# For LPC, replace sample path with /store/user/lcorcodi/bstar_nano/rootfiles/BprimeLH1200_bstar16.root
```


## Setup for 2D Alphabet on lxplus 
```bash
cd ~/CMSVDAS2020
export SCRAM_ARCH=slc7_amd64_gcc700
cmsrel CMSSW_10_6_14 #Only need to do this once
cd CMSSW_10_6_14/src
cmsenv
git clone https://github.com/lcorcodilos/2DAlphabet.git #Only need to do this once
git clone https://github.com/lcorcodilos/HiggsAnalysis-CombinedLimit.git HiggsAnalysis/CombinedLimit/ #Only need to do this once
bash <(curl -s https://raw.githubusercontent.com/lcorcodilos/CombineHarvester/master/CombineTools/scripts/sparse-checkout-ssh.sh) #Only need to do this once
scram b clean; scram b -j 10
cmsenv
```

### Test command for 2D Alphabet
```bash
cd 2DAlphabet
python run_MLfit.py unit_tests/configs/input_lin100kv10k.json --tag=test
```

## Update Code Every Start of Session
When updating TIMBER, it's best to also redo the environment:
```bash
cd ~/CMSVDAS2020/CMSSW_11_0_1/src/
rm -rf timber-env
cmsenv
python -m virtualenv timber-env
source timber-env/bin/activate
```
Then navigate to the TIMBER git space to pull the recent changes:
```bash
cd ../TIMBER
git pull origin master
source setup.sh
```
We should do the same for the bstar analysis code:
```bash
cd ~/CMSVDAS2020/CMSSW_11_0_1/src/BstarToTW_CMSDAS2020
git pull origin master
```
To update the 2DAlphabet package, switch CMSSW relase spaces
```bash
cd ~/CMSVDAS2020/CMSSW_10_6_14/src/
cmsenv
cd 2DAlphabet
git pull origin master
```
