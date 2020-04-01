Using [handshake](https://handshake.org/)'s [ledger-app Github repo](https://github.com/handshake-org/ledger-app-hns) as a reference, kudos!

Following the README of their repo I managed to install their app on my Ledger Nano S from my Macbook running Catalina. If you have any problems have a look at handshake's git repos README.


# Setup development environment
> ⚠️ We will only focos on using `macOS` (Catalina) as **host**, no other OS is supported.  
> ⚠️ This will only work with a _Ledger Nano S_ with firmare `1.6.0` (_Secure Element_: 1.6.0, _Microcontroller_: 1.11).  
> 💡 In the [`Makefile`](Makefile) and the [`Dockerfile.build`](`Dockerfile.build`) you will see a _python_ command named `ledgerblue`, this is the name of _Ledger HQ_'s python tool, but it is the correct tool to be using together with _Ledger Nano S_.

# Debugging (Optional)

## Delete installed app on Ledger

1. Make sure you are **not** running the macOS Desktop Ledger Manager app called _Ledger Live_ - if you are, quit it.
2. In your host shell (python virtualenv is not needed if you have installed `ledgerblue` pip package on your host machine), run:  

	`python -m ledgerblue.deleteApp --targetId 0x31100004 --appName Radix`
3. Confirm deletion on your Ledger Nano S

## Notes on using `ledger-app-hs` (Handshake)
> ⚠️ This (sub)section has nothing to do with the content of this git repo (Radix DLT's Ledger Nano S app), it is notes on how to use the Handshake app

Complete guide:

### Prerequisites
Make sure you have `Docker`, `Python`, `make`, `gcc`, `g++` installed on your host

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

### Run
(Assuming your current directory is `make docker-load` with the python `virtualenv` _activated_)
```sh
make docker-load
```