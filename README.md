![Future Electronics](https://raw.githubusercontent.com/drgunn1/mamabear-app/main/resources/future-electronics.svg)
# Goldilocks
repo of recipes for the Goldilocks platform

Using Ubuntu 20.04:
## Install required packages:
```
sudo apt-get install gawk wget git-core diffstat unzip texinfo gcc-multilibbuild-essential chrpath \
socat cpio pylint3 xterm rsync curl zstd pzstd xz-utils \
python python3 python3-pip python3-pexpect python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev debianutils iputils-ping \
lz4c lz4 libssl-dev
```

## Setup the repo utility:
```
mkdir ~/bin (this step may not be needed if the bin folder already exists)
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
chmod a+x ~/bin/repo
export PATH=~/bin:$PATH && echo export PATH=~/bin:$PATH >> ~/.bashrc
```
## Configure Git:
```
git config --global user.name "Your Name"
git config --global user.email "Your Email"
git config --list
```
## Create the BSP directory and download the Yocto BSP:
```
mkdir ~/mamabear-yocto-bsp
cd ~/mamabear-yocto-bsp
repo init -u https://github.com/drgunn1/gl_manifests -b main -m gl-manifest-6.1.1.xml
repo sync
```
## Create and configure the build folder:
```
MACHINE=imx8mp-ddr4-evk DISTRO=fsl-imx-xwayland source imx-setup-release.sh -b mamabear-build
echo "BBLAYERS += \"\${BSPDIR}/sources/meta-goldilocks\"" >> conf/bblayers.conf
```

## Build the Mamabear Linux image:
```
bitbake goldilocks-image-qt6
```
