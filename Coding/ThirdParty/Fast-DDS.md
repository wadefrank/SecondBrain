# Fast-DDS-2.8.0+Ubuntu20.04

[https://fast-dds.docs.eprosima.com/en/v2.8.0/installation/sources/sources_linux.html](https://fast-dds.docs.eprosima.com/en/v2.8.0/installation/sources/sources_linux.html)

## 安装依赖库

```shell
sudo apt install cmake g++ python3-pip wget git
sudo apt install libasio-dev libtinyxml2-dev
sudo apt install libssl-dev
sudo apt install libp11-dev libengine-pkcs11-openssl
sudo apt install softhsm2
sudo usermod -a -G softhsm qxit-ln-002408
sudo apt install libengine-pkcs11-openssl
# check p11kit is able to find the SoftHSM module use:
p11-kit list-modules
# check if OpenSSL is able to access PKCS#11 engine use
openssl engine pkcs11 -t
```

### Colcon installation

```shell
# Install the ROS 2 development tools (colcon and vcstool)
pip3 install -U colcon-common-extensions vcstool
```

## 编译安装

```shell
cd ~/packages/eProsima_Fast-DDS-v2.8.0-Linux
sudo ./install.sh
```