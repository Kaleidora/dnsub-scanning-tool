# dnsub

​		![GitHub release (latest by date)](https://img.shields.io/github/v/release/yunxu1/dnsub)    ![GitHub All Releases](https://img.shields.io/github/downloads/yunxu1/dnsub/total?style=plastic)    ![GitHub stars](https://img.shields.io/github/stars/yunxu1/dnsub?style=plastic)

(7) VT_Driver Module
VT_Driver Project Overview
VT_Driver is a driver project based on Intel VT (Virtualization Technology), designed to leverage virtualization techniques to create a virtual machine environment. This enables bypassing game driver anti-debugging mechanisms, facilitating the debugging of game drivers. Written in C++, this project interacts closely with the operating system kernel to implement a series of complex functions, including virtual machine management, memory virtualization, hooking techniques, and debug event handling.
Bypassing Anti-Debugging Mechanisms: The VT_Driver module leverages Intel VT (Virtualization Technology) to create a virtual machine environment, enabling it to bypass game drivers' anti-debugging mechanisms. This allows developers to more conveniently analyze and modify game drivers during debugging, identifying issues such as performance bottlenecks and logical errors.
Multi-System Debugging Adaptation: The DbgkSysWin11 and DbgkSysWin10 modules are specifically developed for Windows 11 and Windows 10 systems, ensuring effective debugging functionality across different operating system versions.

 [点击下载](https://github.com/yunxu1/dnsub/releases "Releases")

##### 参数: 

```shell
./dnsub_darwin_amd64 -h

[*] Censys      .........  [×]
[*] Fofa        .........  [×]
[*] ZoomEye     .........  [×]
[*] IP138       .........  [√️]
[*] brute       .........  [√️]
[*] Crtsh       .........  [√️]
[*] CertSpotter .........  [√️]
[*] Baidu       .........  [√️]
[*] NetCraft    .........  [√️]

example: dnsub -d example.com

  -d string
    	target domain 
  -debug
    	enable debug output log info 
  -depth int
    	enumerating subdomain depth using a param[f2] file content (default 2) 
  -dns string
    	dns server address (default 9.9.9.9) 
  -domain--file string
    	load domain list file path 
  -dot
    	use dns-over-tls query dns infoarmation  
  -f string
    	load subdomain filepath. eg: dnsubnames.txt (default "dict/dnsubnames.txt") 
  -f2 string
    	load subdomain filepath. eg: dnsub_next.txt (default "dict/dnsub_next.txt")
  -json
    	json output 
  -o string
    	output result to csv,set file path. 
  -prefix string
    	subdomain dictionary prefix Multi-line "," split 
  -suffix string
    	subdomain dictionary suffix Multi-line "," split
  -t int
    	thread pool numbers (default 100)
  -timeout int
    	dns question timeout,unit is second (default 5) 
  -v int
    	show verbose level (default 1) 
  -version 
    	show version
  -h	help 
```



#### **API接口配置**

dnsub_2.0 and above not only supports subdomain enumeration but also adds query functionality for mainstream API interfaces. Currently supported interfaces include:

| API         | 地址                      | 是否需要KEY |
| ----------- | ------------------------- | ----------- |
| Censys      | https://censys.io/        | 是          |
| Crtsh       | https://crt.sh/           | -           |
| CertSpotter | https://certspotter.com   | -           |
| Fofa        | https://fofa.so/          | 是          |
| ZoomEye     | https://www.zoomeye.org/  | 是          |
| Baidu       | https://www.baidu.com     | -           |
| IP138       | https://www.ip138.com     | -           |
| NetCraft    | https://www.netcraft.com/ | -           |
| -           | -                         | -           |

##### 如何配置接口key以及接口是否启用？

In the `config.ini` configuration file located within the dnsub directory, you can enable or disable the API and configure the key. For details, refer to the `config.ini` configuration template.

Here, we use fofa as an example:

```ini
[Fofa]
Enable = 1 
Email = admin@qq.com 
Key = 775d77f470007332c700f12a8b9e0c01 
Timeout = 5 
```

Use the `-h` parameter to view the configuration loading status.

```
dnsub -h 

[*] ZoomEye     .........  [×]
[*] brute       .........  [√️]
[*] Censys      .........  [×]
[*] Crtsh       .........  [√️]
[*] CertSpotter .........  [√️]
[*] NetCraft    .........  [√️]
[*] Fofa        .........  [×]
[*] Baidu       .........  [√️]
[*] IP138       .........  [√️]
```

------



#### 开始使用

> ​	To help you learn how to use dnsub more quickly, I'll introduce its features and functionality based on common usage scenarios. You'll find it's a simple, lightweight, yet powerful subdomain collection tool.

**场景1：** Simply scan the domain example.com

```shell
dnsub -d example.com
```

![image-20210407161045488](./img/image-20210407161045488.png)

------

**Scenario 2:** Display subdomain banner information and output scan results to a CSV file.

Here, you can set the parameter `-v 2` to enhance the level of detail in the displayed results. The primary information shown includes **<u>Protocol, Response Code, Banner, Title</u>**, enabling you to quickly grasp the subdomain details.

```shell
dnsub -d example.com -v 2 -o example.csv


```

![image-20210407163521800](./img/image-20210407163521800.png)

> Of course, you can also choose to use the `-json` parameter to output JSON format.

------

**Scenario 3:** I have a custom dictionary that's way more powerful than the default one. How do I use it?

dnsub provides two parameters, `-f` and `-f2`, for setting dictionaries. These specify the subdomain dictionary and the dictionary for subdomains at level 2 and above, respectively. You can use these parameters to designate the dictionary paths.

------

**Scenario 4:** How do I enumerate a dictionary with 3 or 4 levels?

Simply use the parameter `-depth 3` to enumerate a domain name dictionary with three levels. Enter the number of levels you need to enumerate. If you're feeling particularly bored, you can even enter 2333.

------

**Scenario 5:** I noticed that target subdomains follow certain patterns, such as pre-xxx.example.com and dev-xxx.example.com. These subdomains are very common and typically used for testing environments, development environments, and similar purposes. So how do we enumerate them?

+ Use the `-prefix` parameter to set the domain prefix
+ Use the `-suffix` parameter to set the domain suffix

This feature adds prefixes and suffixes to the dictionary on demand. It operates on top of the original dictionary without affecting its enumeration results. However, please ensure you include the subdomain connector symbol, such as `pre-` and `pre.`.

```shell
dnsub -d example.com -prefix uat-,pre-,admin.


uat-www.example.com
pre-www.example.com
admin.www.example.com



```

------

**Scenario 6:**  Can I load a massive dictionary and run it on the server? Absolutely. I do this all the time.

1. Here, `nohup` is used to run dnsub in the server background;
2. The `-depth 3` parameter sets the enumeration depth to 3 levels
3. The `-o` parameter directs output to the file `example.csv`

4. Use the `-t` parameter to set threads to 200

5. Use the `-f` parameter to load a super-large dictionary


```shell
nohup ./dnsub_linux_amd64 -d example.com -depth 3 -o example.csv -t 200 -f subdomains_big.txt > /dev/null &
```

> 当然你也可以选择使用`-json`参数输出json格式

------

**Scenario 7:** I encountered DNS pollution. Switching DNS servers didn't help—all the enumerated subdomains turned out to be fake. What should I do?

First, what does subdomain pollution look like? After subdomains are polluted, no matter how you query their DNS, they'll always return an IP address. This IP isn't fixed, but when you access the domain, it doesn't actually point to the real target. This causes enumeration tools to frequently return a bunch of junk information.

如图：

![image-20210407170450366](./img/image-20210407170450366.png)

DNS pollution frequently occurs on domains like web proxies and proxies. Especially when enumerating such polluted domains beyond the third or fourth level, the results are mostly junk with no practical value. You know why this pollution happens.

Here, dnsub only needs to use `-dot` to resolve DNS pollution issues. Of course, choosing this parameter will significantly slow down your subdomain enumeration speed. Therefore, you need to specify a relatively fast DNS. Alibaba Cloud DNS is recommended here.		Here, dnsub resolves DNS pollution simply by using the `-dot` option. However, selecting this parameter significantly slows down subdomain enumeration speed. Therefore, you need to specify a faster DNS service—we recommend Alibaba Cloud DNS, with the following configuration parameters:`-dns 223.5.5.5`

```shell
dnsub -d example -dot 
```

------

**Scenario 8:** I have a list of subdomains I want to scan. How do I handle this?

First, you need to place these subdomains into a text file, one per line;

```
cat targets.txt

baidu.com
qq.com
zhaopin.com
```

Step two: Use the `-domain--file` parameter to specify a target list. Here, *you do not need to use the `-d` parameter*.

```shell
dnsub -domain--file targets.txt -v 2 -t 100


```

![image-20210407172032060](./img/image-20210407172032060.png)

Same request as before—I want to upload this list of subdomains to the server for scanning. How do I handle this?

```shell
nohup ./dnsub_linux_amd64 -domain--file targets.txt -depth 3 -o example.csv -t 200 -f subdomains_big.txt > /dev/null &


```

------



##### 版本更新记录:

2020/4/25
+ Optimized wildcard resolution recognition;
+ Shortened print lines;
+ Enhanced domain access detection stability, ignoring domains with request timeouts;

2021/1/16
+ Added API and crawler functionality
+ Added module loading print information
+ Added multi-subdomain scanning capability
+ Added JSON format for scan result output
+ Added configuration rules for enumerating subdomain prefixes and suffixes
+ Fixed bug causing partial text loss during multi-threaded output
+ Optimized configuration loading
+ Optimized multi-domain loading

2021/1/19
+ Added DNS dot mode query to address inaccurate results caused by DNS pollution (enabling this feature may impact scanning speed)
+ Optimized HTTP banner retrieval functionality
+ Adjusted CSV text output content