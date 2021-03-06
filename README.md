## Apfree WiFiDog: Efficient captive portal solution

Apfree-WiFidog is an open source captive protal solution for wireless router which with embeddabled linux([LEDE](https://github.com/lede-project/source)/[Openwrt](https://github.com/openwrt/openwrt)). 


**[中文介绍](https://github.com/liudf0716/apfree_wifidog/blob/master/README_ZH.md)**

## Awesome

It has some awesome features:

* *Compatible with original wifodog protocol*. You can seamless migration Apfree WiFidog to connect your auth server if you runned traditional wifidog.

* *HTTPS support*. Not only `HTTP`, Apfree WiFiDog can capture `HTTPS` URL request. It's a big deference between traditional WiFiDog.

* *Efficient performance*. Run shell command `time curl --compressed` to test the Apfree WiFiDog reaction rate, `HTTP` response time is 0.05s and `HTTPS` is about 0.2s.

* *Dynamical bulk loading*. Support MAC address and IP address bulk loading with out restart Apfree WiFiDog.

* *Wide application of business*. Apfree WiFidog has been installed and used in tens of thousands routers from KunTeng.Org and partners. Users have been affirmed, fully embodies the applicability, reliability.


----

## How To Compile

Fork and clone the Apfree WiFiDog project:

    git clone https://github.com/KerwinKoo/apfree_wifidog.git 
	cd apfree_wifidog

Assuming you have a working [LEDE](https://github.com/lede-project/source)/[Openwrt](https://github.com/openwrt/openwrt) setup, taking `LEDE` as an example and assuming your LEDE root path is `LEDE_ROOT`:

	cp -r package/apfree_wifidog/ /LEDE_ROOT/package/

To support `HTTPS`, you need install `libevent` with version 2.1.7 or latest in your LEDE environment, Or using the package copied in Apfree WiFiDog git project:

    cp -r package/libevent2/ /LEDE_ROOT/package/libs/

Now Apfree WiFiDog package has been installed in LEDE packages environment.

    cd /LEDE_ROOT/
	make menuconfig

Chose your `Target System` and `ApFree --> apfree_wifidog`. `SAVE` and `EXIT`.

Do compiling:

```
make V=s
```

After Doing `make V=s`, Apfree WiFiDog `ipk` package is packed in path `bin/packages/YOUR-TARGET-ARCH/base/apfree_wifidog_VERSION-RELEASE_YOUR-TARGET-ARCH.ipk `. Push it up into your LEDE-system router, use `opkg install ` command to install this `ipk`.


**The CA-Certificate in this project is ONLY for Apfree WiFiDog HTTPS captive testing, CAN NOT be used for business scene**


--------

## Getting started

After compiling and installing Apfree WiFiDog into your local router, run the `ps | grep wifidog` command. The `ps | grep wifidog` command queries the linux system for information about Apfree WiFiDog.

```
root@lede:~# ps | grep wifidog
 1406 root      6532 S    /usr/bin/wifidog -c /tmp/wifidog.conf -f -d 0
```

In this example, we can see Apfree WiFiDog has run automatically. This command shows some useful information:

* `/usr/bin/wifidog` is the executable binary daemon program, it's named `wifidog` for compatible.
* `/tmp/wifidog.conf` is the WiFiDog's configuration file that generated by parsing `/etc/config/wifidog`. The `UCI` format file `/etc/config/wifidog` is the main configuration file for user, and it will be used by Apfree WiFidog to generate wifidog reader file `/tmp/wifidog.conf`.
* Using operations of `-c -f -d` for default parameters, and you can get their by running command `wifidog --help`.


The default UCI configuration file like this:

```
config wifidog
        option gateway_interface 'br-lan'
        option auth_server_hostname 'entrance.yourauth.org'
        option auth_server_port '80'
        option auth_server_path '/wifidog/'
        option check_interval '60'
        option client_timeout '72000'
        option httpd_max_conn '200'
        option pool_mode '1'
        option thread_number '5'
        option queue_size '20'
        option wired_passed '0'
        option trusted_domains 'www.baidu.com,www.qq.com,www.qq.com.cn,www.weixin.com'
```

Domains of `www.baidu.com,www.qq.com,www.qq.com.cn,www.weixin.com` is trusted in this default configuration file, and you can modify it to what you want.

### How To Contribute

apfree wifidog使用github托管其源代码，贡献代码使用github的PR(Pull Request)的流程，十分的强大与便利:

1. [创建 Issue](https://github.com/liudf0716/apfree_wifidog/issues/new) - 对于较大的改动(如新功能，大型重构等)最好先开issue讨论一下，较小的improvement(如文档改进，bugfix等)直接发PR即可

2. Fork [apfree_wifidog](https://github.com/liudf0716/apfree_wifidog) - 点击右上角**Fork**按钮

3. Clone你自己的fork: ```git clone https://github.com/$userid/apfree_wifidog.git```

4. 在**dev**修改并将修改push到你的fork上

5. 创建从你的fork的**dev**分支到主项目的**dev**分支的[Pull Request] -  [在此](https://github.com/vnpy/vnpy)点击**Compare & pull request**

6. 等待review, 需要继续改进，或者被Merge!

---


