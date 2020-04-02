Using [handshake](https://handshake.org/)'s [ledger-app Github repo](https://github.com/handshake-org/ledger-app-hns) as a reference, kudos!

Following the README of their repo I managed to install their app on my Ledger Nano S from my Macbook running Catalina. If you have any problems have a look at handshake's git repos README.


# Setup development environment
> ⚠️ We will only focos on using `macOS` (Catalina) as **host**, no other OS is supported.  
> ⚠️ This will only work with a _Ledger Nano S_ with firmare `1.6.0` (_Secure Element_: 1.6.0, _Microcontroller_: 1.11).  
> 💡 In the [`Makefile`](Makefile) and the [`Dockerfile.build`](`Dockerfile.build`) you will see a _python_ command named `ledgerblue`, this is the name of _Ledger HQ_'s python tool, but it is the correct tool to be using together with _Ledger Nano S_.

# Debugging (Optional)

## Notes on using `ledger-app-hs` (Handshake)
> ⚠️ This (sub)section has nothing to do with the content of this git repo (Radix DLT's Ledger Nano S app), it is notes on how to use the Handshake app

Complete guide:

### Prerequisites

#### Brew
Install [brew](https://brew.sh)

#### `make` `gcc`

If you have _Xcode_ and _Command Line Tools_ you can probably skip this step:

```sh
brew install make gcc
```

#### Docker

Install [Docker](https://www.docker.com/get-started)
Start Docker  (requires privilages to be setup first time it is run)

#### Python, pip, virtualenv

```sh
brew install python3
```

This will also install `pip`, but you might wanna update it

```sh
sudo -H python3 -m pip install --upgrade pip
```

##### Virtualenv

```sh
sudo -H python3 -m pip install virtualenv
```

#### `~/Developer` dir

Make sure you have a `~/Developer` directory (if not, you should create one and move all your code to it, _Finder_ renders it with a nice hammer icon, so is for sure the intended directory for your code 😀)

### Download Ledger ("BOLOS") SDK 
```sh
git clone https://github.com/ledgerhq/nanos-secure-sdk.git ~/Developer/nanos-secure-sdk
```

```sh
cd ~/Developer/nanos-secure-sdk
```

##### Get Ledger Nano S firmware 1.6.0 version
```sh
git checkout og-1.6.0-1
```

Setup ENV `BOLOS_SDK` to this directory, this variable is required by Makefile build script.
```sh
export BOLOS_SDK=$(pwd)
```


### Download `ledger-app-hs` (Handshake)

```sh
git clone https://github.com/handshake-org/ledger-app-hns.git ~/Developer/ledger-app-hns
```

```sh
cd ~/Developer/ledger-app-hns
```

### Setup `virtualenv`
```sh
virtualenv ledger
```

```sh
source ledger/bin/activate
```

```sh
pip install ledgerblue
```

### Build and install
(Assuming your current directory is `~/Developer/ledger-app-hns` with the python `virtualenv` _activated_)

> ☣️ Make sure you are **not** running the macOS Desktop Ledger Manager app called _Ledger Live_ - if you are, quit it.  
> 💡 Running this command takes 5 minutes and at the end manual input in your Ledger Nano S is required. This command is verbose and you will see lots of red warnings shown, ignore those, even though some might sounds scary (e.g. _fatal error_)
> 💡 Second time you run the command it takes about 1 minute.

```sh
make docker-load
```


You will see output like this:

```bash
Generated random root public key : b'0484cbc761c387c27c1b8126664b6b65bb5f706014c406ce592ab7e41b1d2e0849338f920e754615c36cd6a37cec08ce2bbf949bea4d5f524d1d726cd0616547d0'
Using test master key b'0484cbc761c387c27c1b8126664b6b65bb5f706014c406ce592ab7e41b1d2e0849338f920e754615c36cd6a37cec08ce2bbf949bea4d5f524d1d726cd0616547d0'
Using ephemeral key b'04662028f12f8a1ebf814ba8f9f6a3794c078416f32c275f17b651adf12c39e62ee27d4e9be42cf3d801c90af0ff7b9626bbd836d3188d0db8c0b5c21372cc5bc9'
Broken certificate chain - loading from user key
```

Nevermind the warning: _Broken certificate chain_

At end of build, manual input is needed on your _Ledger Nano S_, navigate one screen **left** (or many screens right until you see option _Allow unsafe manager_), click both bottons and then navigate **two** screens left (or many screens _right_ until _Perform installation_ option), click both buttons. Finalize by entering your PIN code. `make docker-load` command should now have finished in your shell.

### Uninstall
```sh
make delete
```

Manual input is needed on your _Ledger Nano S_, navigate one screen **left** (or many screens right until you see option _Allow unsafe manager_), click both bottons and then navigate **two** screens left (or many screens _right_ until _Perform deletion_ option), click both buttons. Finalize by entering your PIN code.


#### Uninstall globally
Install `ledgerblue` globally (to be able to uninstall apps on Ledger independently on any other code...): 

```sh
sudo -H python3 -m pip install ledgerblue
```

Run:  

```sh
python -m ledgerblue.deleteApp --targetId 0x31100004 --appName <NAME_OF_APP>
```

Where `<NAME_OF_APP>` should be replaced with the name of the app you wanna uninstall (as you see it on your Ledger).