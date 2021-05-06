### install boost1.67 

if you already install pre boost version. erros may be happended.remove pre version

```
sudo apt autoremove libboost1.65-dev
```

then install boost1.67

```
sudo add-apt-repository ppa:mhier/libboost-latest
sudo apt-get update
sudo apt install libboost1.67-dev
```



### install openssl 1.1.0g

```
wget https://www.openssl.org/source/openssl-1.1.0g.tar.gz
tar -zxvf openssl-1.1.0g.tar.gz -C /usr/local/src
cd /usr/local/src/openssl-1.1.0g/
./config
make
sudo make install
sudo rm /usr/bin/openssl & sudo ln -s /usr/local/bin/openssl /usr/bin/openssl
sudo ln -s /usr/local/lib/libssl.so.1.1 /usr/lib/libssl.so.1.1
sudo ln -s /usr/local/lib/libcrypto.so.1.1 /usr/lib/libcrypto.so.1.1
sudo ldconfig
```

### compile



```
cd src
make -f makefile.unix
```

then run it.

```
./woodcoind
```