![Future Electronics](https://raw.githubusercontent.com/drgunn1/mamabear-app/main/resources/future-electronics.svg)
# Goldilocks
repo of recipes for the Goldilocks platform

Using Ubuntu 22.04:
## Install required packages:
```
sudo apt install gawk wget git diffstat unzip texinfo gcc build-essential \
chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils \
iputils-ping python3-git python3-jinja2 python3-subunit zstd liblz4-tool file \
locales libacl1
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
repo init -u https://github.com/drgunn1/gl_manifests -b 6.6.52_2.2.0 -m gl-manifest-6.6.52.xml
repo sync
```
## Create and configure the build folder:
```
MACHINE=fdc-mamabear DISTRO=fsl-imx-xwayland source imx-setup-release.sh -b mamabear-build
echo "BBLAYERS += \"\${BSPDIR}/sources/meta-goldilocks\"" >> conf/bblayers.conf
echo "BBLAYERS += \"\${BSPDIR}/sources/meta-nxp-connectivity/meta-nxp-matter-advanced\"" >> conf/bblayers.conf
echo "BBLAYERS += \"\${BSPDIR}/sources/meta-nxp-connectivity/meta-nxp-otbr\"" >> conf/bblayers.conf
echo "BBLAYERS += \"\${BSPDIR}/sources/meta-nxp-connectivity/meta-nxp-connectivity-examples\"" >> conf/bblayers.conf
echo "BBLAYERS += \"\${BSPDIR}/sources/meta-nxp-connectivity/meta-nxp-zigbee-rcp\"" >> conf/bblayers.conf
echo "LICENSE_FLAGS_ACCEPTED = \" commercial \""  >> conf/local.conf
```

## Build the Mamabear Linux image:
```
bitbake goldilocks-image-qt6
```
