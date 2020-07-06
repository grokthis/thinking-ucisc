# Setting up Micro-CISC

Before we can do anything else, we need to get the micro-CISC environment setup
so we can learn to program our homebrew computers. If you are familar with the
Ruby programming language, you can use the quickstart section. Otherwise, find
the section with your operating system below to get your system ready.

Quick links to sections of this document:

* [Quickstart](#Quickstart)
* [Linux](#Linux)
* [MacOS](#MacOS)
* [Windows](#Windows)

## Quickstart

### 1. Install the uCISC gem

If you are already familiar with Ruby, you can quickly get uCISC running via the
uCISC gem:

```
gem install ucisc
```

### 2. Write some code

Open a file using your favorite text editor and add the following code to it.
This code is also available in the examples folder
[examples/first.ucisc](examples/first.ucisc).

```
# first.ucisc

# Declare a variable reference to the stack
# This is purely syntax sugar so that we can refer to 1.mem as $stack everywhere
$stack as 1.mem

# Let's load 42 onto the stack (via register 3)
copy 42.imm 3.reg
copy 3.reg $stack push

# This is a special halt condition to stop execution
copy 0.reg 0.reg
```

### 3. Run some code

You can now run ucisc code using the `ucisc` command.

```
ucisc first.ucisc
```

This should give you the following output:

```
Reading first.ucisc
INFO: ADDRESS: INS-WORD  LINE#: SOURCE
INFO:  0x0000: 0x7e2a        6: copy 42.imm 3.reg
INFO:  0x0001: 0x67c0        7: copy 3.reg $stack push
INFO:  0x0002: 0x6000       10: copy 0.reg 0.reg
Running program with 3 words
INFO: HALT: 2 instructions in 1.018e-05s
INFO: Stack: FFFF => 0x002A 
Done.
```

At the end, the stack points to memory address 0xFFFF with the hex value 0x002A
which is the equivalent of 42 in decimal. We will of course cover all of the
nitty gritty details of what is going on here in the next lessons.

## Linux

You probably already have ruby installed on your linux machine. However, I
suggest using RVM to install specific versions of Ruby. If you install the same
version of Ruby that is used in these tutorials, your results will be the most
predictable.

### 1. Install RVM

Open a terminal window to run the commands below. On Ubuntu, open "Activities"
and search for "Terminal". Other linux distributions may vary, but the terminal
should be one of the prominently available applications.

More details on RVM can be found at [rvm.io](https://rvm.io/). The following
commands have been verified with Ubuntu 19.10. If you use a different linux
distribution, your setup may vary slighly. Enter the following commands:

```
gpg --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB

curl -sSL https://get.rvm.io | bash -s stable
```

### 2. Install Ruby 2.7.1

uCISC is tested on Ruby 2.7.1, so we will install it now.

```
rvm install 2.7.1

rvm --default use 2.7.1
```

### 3. uCISC Quickstart

For the remaining steps, follow the [uCISC Quickstart](#Quickstart) at the top
of this document.

## MacOS

MacOS ships with ruby already installed. However, I suggest using RVM to install
specific versions of Ruby. If you install the same version of Ruby that is used
in these tutorials, your results will be the most predictable.

### 1. Install RVM

Open the terminal application. You can do this by opening Launchpad or Siri and
searching for "Terminal".

More details on RVM can be found at [rvm.io](https://rvm.io/). The following
commands have been verified with MacOS Catalina (10.15.4). If you use any recent
version of MacOS, your setup should be the same. Enter the following commands:

```
gpg --keyserver hkp://ipv4.pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB

curl -sSL https://get.rvm.io | bash -s stable
```

### 2. Install Ruby 2.7.1

uCISC is tested on Ruby 2.7.1, so we will install it now.

```
rvm install 2.7.1

rvm --default use 2.7.1
```

### 3. uCISC Quickstart

For the remaining steps, follow the [uCISC Quickstart](#Quickstart) at the top
of this document.

## Windows

Windows is not a natural Ruby operating system because of Ruby history (it was
designed for Unix based systems like Linux and MacOS). However, windows is
still supported, there are just a few extra hoops to jump through. I've put
together a guide to get it running as predictably as possible for this project.

### 1. Install Ruby

The amazing people over at [RubyInstaller.org](https://rubyinstaller.org/) have
put together a simple installer for getting Ruby up and running on windows. Head
over to their [downloads page](https://rubyinstaller.org/downloads/) and
download the installer for Ruby 2.7.1 with Devkit. Make sure you get the one
marked "With Devkit" or you won't be able to install the uCISC VM.

Once you have downloaded the installer. Run it and use all the default options.
When the installer launches the command line installer component, press "enter"
to accept the defaults.

### 2. uCISC Quickstart

For the remaining steps, follow the [uCISC Quickstart](#Quickstart) at the top
of this document. Important note: To open a command line to run the commands in
the quickstart guide, you will need to open the Windows menu and search for CMD.
