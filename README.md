### XMR-Stak-CPU - Monero mining software

XMR-Stak is a universal Stratum pool miner. This is the CPU-mining version, but I'm planning to release Nvidia and AMD GPU versions too.


#### Usage on Windows 
1) Edit the config.txt file to enter your pool login and password. 
2) Double click the exe file. 

XMR-Stak should compile on any C++11 compliant compiler. Windows compiler is assumed to be MSVC 2015 CE. MSVC build environment is not vendored.
```
-----BEGIN PGP SIGNED MESSAGE-----
Hash: SHA256

Windows binary release checksums

sha1sum xmr-stak-cpu.exe
32f551c891040eda2c25e18e6287665471a5a653  xmr-stak-cpu.exe

sha3sum xmr-stak-cpu.exe
ed12841738c899a3eb61f51787aa670c25b64ce3c5a626717e6a8f6b  xmr-stak-cpu.exe

date
Mon 16 Jan 15:13:25 GMT 2017
-----BEGIN PGP SIGNATURE-----
Version: GnuPG v2

iQEcBAEBCAAGBQJYfONvAAoJEPsk95p+1Bw02coH/0by+VMK76gnmpNjIxDcphkV
S1GG+f0sIAYUrGpoMCJTXbr7hU3Na+grTbt6xLM2Tb0xJjX4Mc47Cixajzy7+TTx
R2+CvBRl8LG9zob6JNiohvxD1+SK7RWDKWenFyDlr9BewgE/ArqZM+16BQBrLP9H
XIWy1wh/lcSYuS548tnUYdNOmEnR9TqA454M4r8PED85HSpNmvI+eG8fZ8OK471C
3yMupjYlAbiEBT+gE6bZwLeeCH9NO2gGeBAb31w8RBsMRjy+VvhFhTOoJwZbXj9e
sMUwNBu+fLVoilMVvp8SDpQ7Uw/WFT085N2eJiCCuEbHgFAwM3uwD6VHz3eXd0s=
=QJQj
-----END PGP SIGNATURE-----
```

#### Usage on Linux
```
    sudo apt-get install libmicrohttpd-dev
    cmake .
    make
```

GCC version 5.1 or higher is required for full C++11 support. CMake release compile scripts, as well as CodeBlocks build environment for debug builds is included.

To do a static build for a system without gcc 5.1+
```
    cmake -DCMAKE_BUILD_TYPE=STATIC
    make
```
Note - cmake caches variables, so if you want to do a dynamic build later you need to specify '-DCMAKE_BUILD_TYPE=RELEASE'

#### CPU mining performance 

Performance is nearly identical to the closed source paid miners. Here are some numbers:

* **I7-2600K** - 266 H/s
* **I7-6700** - 276 H/s (with a separate GPU miner)
* **Dual X5650** - 466 H/s (depends on NUMA)
* **Dual E5640** - 365 H/s (same as above)

#### Example reports
```
HASHRATE REPORT
| ID | 2.5s |  60s |  15m | ID | 2.5s |  60s |  15m |
|  0 | 31.7 | 30.7 | 30.5 |  1 | 30.6 | 30.6 | 30.6 |
|  2 | 30.3 | 30.6 | 30.6 |  3 | 30.6 | 30.6 | 30.6 |
|  4 | 35.3 | 35.5 | 35.6 |  5 | 35.7 | 35.7 | 35.7 |
|  6 | 35.4 | 35.6 | 35.6 |  7 | 35.7 | 35.7 | 35.7 |
|  8 | 31.7 | 30.7 | 30.5 |  9 | 30.6 | 30.6 | 30.6 |
| 10 | 30.4 | 30.6 | 30.6 | 11 | 30.6 | 30.6 | 30.6 |
-----------------------------------------------------
Totals:   388.7 388.7 388.7 H/s
Highest:  388.7 H/s
```

```
RESULT REPORT
Difficulty       : 8192
Good results     : 5825 / 5826 (100.0 %)
Avg result time  : 10.3 sec
Pool-side hashes : 22683648

Top 10 best results found:
|  0 |         15407238 |  1 |         12699745 |
|  2 |         12194202 |  3 |          6999845 |
|  4 |          5533935 |  5 |          5315338 |
|  6 |          4700351 |  7 |          4500227 |
|  8 |          4023567 |  9 |          4021473 |

Error details:
| Count |                       Error text |           Last seen |
|     1 | [NETWORK ERROR]                  | 2017-01-02 21:29:15 |
```

```
CONNECTION REPORT
Connected since : 2017-01-02 21:29:40
Pool ping time  : 288 ms

Network error log:
| Date                |                                                       Error text |
| 2017-01-02 21:29:15 | CALL error: Timeout while waiting for a reply                    |
| 2017-01-02 21:29:30 | CONNECT error: GetAddrInfo: Name or service not known            |
```



### Common Issues

**SeLockMemoryPrivilege failed**

Please see [config.txt](config.txt) under section **LARGE PAGE SUPPORT**

For Windows 7 pro, or Windows 8 and above see [this article](https://msdn.microsoft.com/en-gb/library/ms190730.aspx)  (make sure to reboot afterwards!).

For Windows 7 Home :

1) Download and install [Windows Server 2003 Resource Kit Tools](https://www.microsoft.com/en-us/download/details.aspx?id=17657).  Ignore incompatiablity warning during installation.

2) In cmd or power shell: `ntrights -u %USERNAME% +r SeLockMemoryPrivilege`  (where %USERNAME% is the user that will be running the program.  This command needs to be run as admin)

3) Reboot.

Reference: http://rybkaforum.net/cgi-bin/rybkaforum/topic_show.pl?pid=259791#pid259791

*Warning: do not download ntrights.exe from any other site other then the offical Microsoft download page.*

**VirtualAlloc failed**

If you set up the user rights properly (see above), and your system has 4-8GB of RAM (50%+ use), there is a significant chance that there simply won't be a large enough chunk of contiguous memory because Windows is fairly bad at mitigating memory fragmentation.

If that happens, disable all auto-staring applications and run the miner after a reboot.

**msvcp140.dll and vcruntime140.dll not available errors**

Download and install this [runtime package](https://www.microsoft.com/en-us/download/details.aspx?id=48145) from Microsoft.  *Warning: Do NOT use "missing dll" sites - dll's are exe files with another name, and it is a fairly safe bet that any dll on a shady site like that will be trojaned.  Please download offical runtimes from Microsoft above.*


**Error: MEMORY ALLOC FAILED: mmap failed**

From [config.txt](config.txt):

On Linux you will need to configure large page support `sudo sysctl -w vm.nr_hugepages=128` and increase your
ulimit -l. To do do this you need to add following lines to /etc/security/limits.conf:

    * soft memlock 262144
    * hard memlock 262144
    
Save file.  You WILL need to log out and log back in for these settings to take affect on your user (no need to reboot, just relogin in your session).
   
You can also do it Windows-style and simply run-as-root, but this is NOT recommended for security reasons.

**Illegal instruction (core dumped)**

This typically means you are trying to run it on a CPU that does not have [AES](https://en.wikipedia.org/wiki/AES_instruction_set).  This only happens on older version of miner, new version gives better error message (but still wont' work since your CPU doesn't support the required instructions).



