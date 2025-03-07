<!-- The badge template is from https://ileriayo.github.io/markdown-badges/ -->
<div align = center>

[![ArbitrarilyTong](https://github.com/ArbitrarilyTong/.github/raw/main/github_banner_2.png)][website]

<h1>ArbitrarilyTong | 任意桐</h1>

English | [中文](./README.cn.md)

This project is developed based on [crDroid][crd]

---

[![Telegram](https://img.shields.io/badge/Telegram-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white)][telegram]
[![Twitter](https://img.shields.io/badge/Twitter-%231DA1F2.svg?style=for-the-badge&logo=Twitter&logoColor=white)][twitter]
[![Codeberg](https://img.shields.io/badge/Codeberg-2185D0?style=for-the-badge&logo=Codeberg&logoColor=white)][codeberg]
[![GitHub](https://img.shields.io/badge/Devices-%23121011.svg?style=for-the-badge&logo=github&logoColor=white)][devices]

---
</div>

## Preparation

<details>
<summary>Before officially starting the compilation work, we need to make the following preparations</summary>

### 1. Installing dependencies

Several packages are needed in order to build crDroid.

For `Ubuntu`:

```sh
sudo apt install bc bison build-essential ccache curl flex g++-multilib gcc-multilib git git-lfs gnupg gperf imagemagick lib32ncurses5-dev lib32readline-dev lib32z1-dev liblz4-tool libncurses5 libncurses5-dev libsdl1.2-dev libssl-dev libwxgtk3.0-gtk3-dev libxml2 libxml2-utils lzop pngcrush rsync schedtool squashfs-tools xsltproc zip zlib1g-dev
```

For `ArchLinux`:

```sh
yay -S aosp-devel lineageos-devel
```

For other distributions, please go online and find the relevant packages yourself.

### 2. Get repo

[Repo](http://source.android.com/source/developing.html) is a tool provided by Google that
simplifies using [Git](http://git-scm.com/book) in the context of the Android source.

On all distributions, you can get the `repo` from the command line by executing:

```bash
# Make a directory where Repo will be stored and add it to the path
mkdir ~/bin
PATH=~/bin:$PATH

# Download Repo itself
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo

# Make Repo executable
chmod a+x ~/bin/repo
```

For `ArchLinux`, you can also install via `yay`:

```sh
yay -S repo
```
</details>



## Initializing Repo

The following branches can be used to build:

- ArbitrarilyTong-13.0 (Based on crDroid)

Enter the following to initialize the repository:

```bash
# Create a directory for the source files
# This can be located anywhere (as long as the fs is case-sensitive)
mkdir ArbitrarilyTong
cd ArbitrarilyTong

# Enable LFS features
git lfs install

# Install Repo in the created directory
repo init -u https://github.com/ArbitrarilyTong/android.git -b ArbitrarilyTong-13.0 --git-lfs
```

## Sync the source code

This is what you will run each time you want to pull in upstream changes. Keep in mind that on your
first run, it is expected to take a while as it will download all the required Android source files
and their change histories.

```bash
# Let Repo take care of all the hard work
repo sync -c -j$(nproc --all) --force-sync --no-clone-bundle --no-tags
# If you need to check error logs,you can use -v
# repo sync -c -j$(nproc --all) --force-sync --no-clone-bundle --no-tags -v
```

## Prepare the device-specific code

The following devicecodename can be used to build:

- thyme (Xiaomi 10S)

Enter the following to initialize the repository:

```bash
# Run to prepare our devices list
source build/envsetup.sh
# ... now run
lunch lineage_devicecodename-buildtype
```

## Turn on caching to speed up build

Make use of `ccache` if you want to speed up subsequent builds by running:

```sh
export USE_CCACHE=1
export CCACHE_EXEC=/usr/bin/ccache
```

and adding that line to your `~/.bashrc` file. Then, specify the maximum amount of disk space you want ccache to use by typing this:

```sh
ccache -M 50G
```

where `50G` corresponds to 50GB of cache. This needs to be run once. Anywhere from 25GB-100GB will result in very noticeably increased build speeds (for instance, a typical 1hr build time can be reduced to 20min). If you’re only building for one device, 25GB-50GB is fine. If you plan to build for several devices that do not share the same kernel source, aim for 75GB-100GB. This space will be permanently occupied on your drive, so take this into consideration.

You can also enable the optional `ccache` compression. While this may involve a slight performance slowdown, it increases the number of files that fit in the cache. To enable it, run:

```sh
ccache -o compression=true
```

## Start the build

Time to start building! Now, type:

```sh
croot
mka bacon -j$(nproc --all)
```

The build should begin.

## Install the build

Assuming the build completed without errors (it will be obvious when it finishes), type the following in the terminal window the build ran in:

```SH
cd $OUT
```

There you’ll find all the files that were created. The two files of more interest are:

- `boot.img`, which is the PixelExperience boot image, and contains the recovery-ramdisk.
- A zip file whose name starts with `ArbitrarilyTong-`, which is the crDroid installer package.

<!-- reference infos -->
[crd]: https://github.com/crdroidandroid
<!-- project infos -->
[website]: https://arbitrarilytong.win
[telegram]: https://t.me/ArbitrarilyTong
[twitter]: https://twitter.com/Ninni_kiri_jp
[codeberg]: https://codeberg.org/ArbitrarilyTong
[devices]: https://github.com/ArbitrarilyTong-Devices
[manifest]: https://github.com/ArbitrarilyTong/android