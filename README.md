# Setting-up-Kali-for-Analyzing-or-Debugging-Binaries

# Intro
In this write-up, we quickly install a bunch of tools to get some basics set up for debugging binaries.  All credit for the tools and techniques go to the excellent John Hammond video listed below.

## Credit
The commands and tools selected here are based on the first 8 minutes (the setup) of:

	#Binary Exploitation Deep Dive: Return to LIBC (with Matt)
	https://youtu.be/tMN5N5oid2c


## Search for `kali` tools
Optionally, you can search for any interesting `kali` tools at the following link.

	https://www.kali.org/tools/


## Install the `gdb` debugger
One of the requirements is `gdb` which is a popular open source debugger. You can optionally visit the official project page at `https://www.sourceware.org/gdb/` to learn more about the history, etc.

To install on `kali` we can use `apt install` as follows:


	#typical
	sudo apt install gdb

	#optional - support more operating systems
	sudo apt install gdb-multiarch


*Note: For more detail at about the `kali` versions shown above, see the `kali` tools page at https://www.kali.org/tools/gdb/`.*


## Install `java`


	#Optional - show what what you have or need
	sudo apt list libc6 openjdk-11-jdk-headless libgcc-s1 libstdc++6 

	#install the requirements
	sudo apt install -y libc6 openjdk-11-jdk-headless libgcc-s1 libstdc++6

	#install java
	sudo apt install openjdk-11-jdk-headless -y

	#test java
	java -version


## Install `Ghidra` Reversing Tool


	sudo apt install ghidra

*Note: The dependencies for `ghidra` should be part of `kali` by default, but for reference, the requirements are:* 
    
    libc6
    libgcc-s1
    libstdc++6
    openjdk-11-jdk-headless 


## Optional - Install `sublime` text editor


	cd Downloads
	wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
	sudo apt-get install apt-transport-https
	echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
	sudo apt-get update\nsudo apt-get install sublime-text


## Optional - Install `Eclipse` IDE
You can optionally install `Eclipse` which is an integrated development environment (IDE) for Java, C, etc.

	https://www.eclipse.org/

## Native Tools on `kali` (`python3` and `pip`)
Some things we have by default in `kali` are `python3` and `pip`, which we can confirm with `--version`.  We will use these tools later.

	python3 --version

	pip --version


## Optional - Install `bpython`

	sudo apt install bpython


## Install `pwn` with `pip`

	pip install pwn

*Note: Not to be confused with `pwninit` which we install later.*

## About `gef` Requirements
If you download the entire repo then you can just point to this file with `pip` to install all of them with `pip install -r requirements.txt` as shown in the steps later.

The following is the content of the file in case of interest.

	## example "requirements.txt" for `gef`.

			cat ┌──(tech1㉿kali001)-[~]
	└─$ cat ~/Downloads/.gef/requirements.txt
	capstone
	keystone-engine
	pylint
	ropper
	unicorn
	pytest
	pytest-xdist
	coverage


## Installing `gef`
So before we install, `gef` be sure to launch your regular `gdb` at least once so you know what it looks like.  Once you install `gef` it enhances your experience with `gdb` next time you launch it.

	#optional - launch `gdb`
	gdb


	#quit gdb
	press q


	#install gef (pick an option)

		#option 1 - entire repo
		#this is good because you can look at required packages
			
			#change directory
			cd Downloads

			#download the repo
			git clone https://github.com/hugsy/gef

			#change directory
			cd gef

			#copy the script to your home directory
			cp gef.py ~/.gef.py

			#list required packages
			cat ~/Downloads/.gef/requirements.txt

			#install required packages with pip
			pip install -r requirements.txt


		#option 2 - download just the script
		#this is good if you have all the pre-reqs or will do them later

			wget -q "https://github.com/hugsy/gef/raw/master/gef.py" -O "${HOME}/.gef.py"

	#create a .gdbinit file, if needed
	touch ~/.gdbinit
	vi ~/.gdbinit

	#contents of .gdbinit should be the following one line:
	source ~/.gef.py

	#use gef
	gdb

	#get help
	help

	#quit
	q


## Using `ldd`
Review the help information and best practices for this tool.


## Install `rust`
This install make take some time so be patient.

	curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

*Note: After the `rust` installation completes, close and relaunch your terminal*

## Confirm `rust` is installed
We can use the `cargo` command to confirm that `rust` is ready.

	cargo --version

*Note: `cargo` is the package manager for `rust`.*

## Installing `pwninit` with `cargo`
First, we install a long list of pre-requisites using `apt`, and then finally we use `cargo` to download and build the `pwninit` package.

*Note: You can optionally install multiple packages with `apt` on a single line (just add a space between), but here we show one at time.*


	#install patchelf
	sudo apt install patchelf

	#install elfutils
	sudo apt install elfutils

	#install pkg-config
	sudo apt install pkg-config

	#install lzma
	sudo apt install lzma

	#install liblzma-dev
	sudo apt install liblzma-dev

	#install libssl-dev  #note: we already have `openssl` by default but we also need this.
	sudo apt install libssl-dev

	#install pwninit
	cargo install pwninit

*Tip: If your build fails, try to resolve any dependencies mentioned in the error and then run the `cargo install pwninit` command again. All previous downloads and progress are remembered automatically, so once you have all of the requirements, the compile should be fast and all green.*.

## Running `pwnit`

	#optional - show location of binary you just built
	which pwninit

	#usage (note: only run from the directory to analyze / setup)
	#warning: this will make some bits exectuable in this directory
	cd <folder-of-some-bits-to-hack>
	/path/to/pwninit   #this generates a "solve.py"

	#optional - Review `solve.py`
	less ./solve.py

*Note: The `solve.py` would be used by the leaders of a capture the flag challenge to setup a scenario.*


## Summary
In this write-up, we got you up and running with some some binary debugging tools for `kali` Linux.

