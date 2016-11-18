# HowTo Generate Docker Container and Install "PosgreSQL 9.6 BDR" (*)

### Reviso imagenes prexistentes y las borro de ser necesario

```
cepxio@corvette:~/tmp/CentOS-PostgreSQL-BDR_tests$ docker images
REPOSITORY                      TAG                 IMAGE ID            CREATED             SIZE
centos_pgbdr96-4                latest              243720160718        About an hour ago   361.2 MB
centosrewardcompilador_reward   latest              6ec8989e1b50        3 months ago        489.9 MB
cepxio/centos_pgbdr96           last                00d69af54adb        3 months ago        196.7 MB
centos-java1.8                  rewards             8ae950955501        3 months ago        498.5 MB
centos                          latest              50dae1ee8677        4 months ago        196.7 MB
```
```
cepxio@corvette:~/tmp/CentOS-PostgreSQL-BDR_tests$ docker ps -a
CONTAINER ID        IMAGE                           COMMAND                  CREATED             STATUS                         PORTS               NAMES
fd688199fc44        centos_pgbdr96-4                "/bin/bash"              7 minutes ago       Exited (0) 7 minutes ago                           stoic_liskov
74b2dbba448d        243720160718                    "/bin/bash"              About an hour ago   Exited (0) 33 minutes ago                          berserk_mcnulty
ae1c4fdd1d3f        00d69af54adb                    "/bin/sh -c 'yum inst"   About an hour ago   Exited (1) About an hour ago                       nauseous_hoover
425e41dbe824        centos                          "/bin/bash"              About an hour ago   Exited (0) About an hour ago                       condescending_euclid
aeb33876ba7c        00d69af54adb                    "/bin/sh -c 'yum inst"   2 hours ago         Exited (1) 2 hours ago                             jolly_gates
45023011fac8        centos-java1.8:rewards          "bash -c 'cd /usr/loc"   12 weeks ago        Exited (0) 12 weeks ago                            centosrewardcompilador_reward_run_12
f24515c00224        centos-java1.8:rewards          "bash -c 'cd /usr/loc"   3 months ago        Exited (0) 3 months ago                            centosrewardcompilador_reward_run_11
5e5f1f79824b        centos-java1.8:rewards          "bash -c 'cd /usr/loc"   3 months ago        Exited (0) 3 months ago                            centosrewardcompilador_reward_run_10
570719746ff5        centos-java1.8:rewards          "cd /usr/local/src/mg"   3 months ago        Exited (0) 3 months ago                            centosrewardcompilador_reward_run_9
8f8d1a16c430        centos-java1.8:rewards          "/bin/bash"              3 months ago        Exited (0) 3 months ago                            centosrewardcompilador_reward_run_8
fc0846a917a1        centos-java1.8:rewards          "/bin/bash"              3 months ago        Exited (0) 3 months ago                            centosrewardcompilador_reward_run_7
313702bca6c9        centos-java1.8:rewards          "/bin/bash"              3 months ago        Exited (0) 3 months ago                            centosrewardcompilador_reward_run_6
75c2373eb00a        centos-java1.8:rewards          "/bin/bash"              3 months ago        Exited (0) 3 months ago                            centosrewardcompilador_reward_run_5
e78d57bf01f9        centos-java1.8:rewards          "/bin/bash"              3 months ago        Exited (0) 3 months ago                            centosrewardcompilador_reward_run_4
8f2876c44198        centos-java1.8:rewards          "/bin/bash"              3 months ago        Exited (0) 3 months ago                            centosrewardcompilador_reward_run_3
726cd3e6e013        centos-java1.8:rewards          "/bin/bash"              3 months ago        Exited (0) 3 months ago                            rewards
4b28757c057f        centosrewardcompilador_reward   "bash"                   3 months ago        Exited (0) 3 months ago                            centosrewardcompilador_reward_run_2
c23eff75bb89        centosrewardcompilador_reward   "/bin/bash"              3 months ago        Exited (0) 3 months ago                            centosrewardcompilador_reward_run_1
```
```
cepxio@corvette:~/tmp/CentOS-PostgreSQL-BDR_tests$ sudo docker rmi --force centos_pgbdr96-4
Untagged: centos_pgbdr96-4:latest
Deleted: sha256:24372016071859b94eafa3fbf1ca82a26ac5ebd442342a74e171c9c4dffd2f90
```

### Genero el Dockerfile para levantar un CentOS.

```
cepxio@corvette:~/tmp/CentOS-PostgreSQL-BDR_tests$ cat Dockerfile 
# ==========================================================================
# Dockerfile para generar contenedor con los paquetes 
# necesarios para la instalacion de BDR en 9.6
#
# Autor: El Ruso
# Image: CentOS 7.2
# Package to install: postgresql-bdr94-2ndquadrant-redhat-latest.noarch.rpm
# ==========================================================================

FROM centos
MAINTAINER cepxio <sostapowicz@cmd.com.ar>
RUN yum install -y http://packages.2ndquadrant.com/postgresql-bdr94-2ndquadrant/yum-repo-rpms/postgresql-bdr94-2ndquadrant-redhat-latest.noarch.rpm
RUN yum install -y vim
RUN yum install -y postgresql-bdr94-bdr
```

### Build for setup de new machine

```
cepxio@corvette:~/tmp/CentOS-PostgreSQL-BDR_tests$ sudo docker build -f Dockerfile -t centos_pgbdr96-4 .
Sending build context to Docker daemon 16.38 kB
Step 1 : FROM centos
 ---> 50dae1ee8677
Step 2 : MAINTAINER cepxio <sostapowicz@cmd.com.ar>
 ---> Using cache
 ---> 00d69af54adb
Step 3 : RUN yum install -y http://packages.2ndquadrant.com/postgresql-bdr94-2ndquadrant/yum-repo-rpms/postgresql-bdr94-2ndquadrant-redhat-latest.noarch.rpm
 ---> Running in 97abb537a47f
Loaded plugins: fastestmirror, ovl
Examining /var/tmp/yum-root-ZNpKjR/postgresql-bdr94-2ndquadrant-redhat-latest.noarch.rpm: postgresql-bdr94-2ndquadrant-redhat-1.0-3.noarch
Marking /var/tmp/yum-root-ZNpKjR/postgresql-bdr94-2ndquadrant-redhat-latest.noarch.rpm to be installed
Resolving Dependencies
--> Running transaction check
---> Package postgresql-bdr94-2ndquadrant-redhat.noarch 0:1.0-3 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

================================================================================
 Package  Arch   Version
                       Repository                                          Size
================================================================================
Installing:
 postgresql-bdr94-2ndquadrant-redhat
          noarch 1.0-3 /postgresql-bdr94-2ndquadrant-redhat-latest.noarch 2.5 k

Transaction Summary
================================================================================
Install  1 Package

Total size: 2.5 k
Installed size: 2.5 k
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : postgresql-bdr94-2ndquadrant-redhat-1.0-3.noarch             1/1 
Run:
/bin/rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-2NDQ-BDR-94
if needed. RPM should generally prompt you to accept it when you first
try to install signed packages.
  Verifying  : postgresql-bdr94-2ndquadrant-redhat-1.0-3.noarch             1/1 

Installed:
  postgresql-bdr94-2ndquadrant-redhat.noarch 0:1.0-3                            

Complete!
 ---> 3bbf366bd618
Removing intermediate container 97abb537a47f
Step 4 : RUN yum install -y vim
 ---> Running in e14f51c68588
Loaded plugins: fastestmirror, ovl
Determining fastest mirrors
 * base: centos.ar.host-engine.com
 * extras: centos.ar.host-engine.com
 * updates: centos.ar.host-engine.com
Resolving Dependencies
--> Running transaction check
---> Package vim-enhanced.x86_64 2:7.4.160-1.el7 will be installed
--> Processing Dependency: vim-common = 2:7.4.160-1.el7 for package: 2:vim-enhanced-7.4.160-1.el7.x86_64
--> Processing Dependency: which for package: 2:vim-enhanced-7.4.160-1.el7.x86_64
--> Processing Dependency: perl(:MODULE_COMPAT_5.16.3) for package: 2:vim-enhanced-7.4.160-1.el7.x86_64
--> Processing Dependency: libperl.so()(64bit) for package: 2:vim-enhanced-7.4.160-1.el7.x86_64
--> Processing Dependency: libgpm.so.2()(64bit) for package: 2:vim-enhanced-7.4.160-1.el7.x86_64
--> Running transaction check
---> Package gpm-libs.x86_64 0:1.20.7-5.el7 will be installed
---> Package perl.x86_64 4:5.16.3-286.el7 will be installed
--> Processing Dependency: perl(Socket) >= 1.3 for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(Scalar::Util) >= 1.10 for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl-macros for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(threads::shared) for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(threads) for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(constant) for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(Time::Local) for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(Time::HiRes) for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(Storable) for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(Socket) for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(Scalar::Util) for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(Pod::Simple::XHTML) for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(Pod::Simple::Search) for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(Getopt::Long) for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(Filter::Util::Call) for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(File::Temp) for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(File::Spec::Unix) for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(File::Spec::Functions) for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(File::Spec) for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(File::Path) for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(Exporter) for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(Cwd) for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(Carp) for package: 4:perl-5.16.3-286.el7.x86_64
---> Package perl-libs.x86_64 4:5.16.3-286.el7 will be installed
---> Package vim-common.x86_64 2:7.4.160-1.el7 will be installed
--> Processing Dependency: vim-filesystem for package: 2:vim-common-7.4.160-1.el7.x86_64
---> Package which.x86_64 0:2.20-7.el7 will be installed
--> Running transaction check
---> Package perl-Carp.noarch 0:1.26-244.el7 will be installed
---> Package perl-Exporter.noarch 0:5.68-3.el7 will be installed
---> Package perl-File-Path.noarch 0:2.09-2.el7 will be installed
---> Package perl-File-Temp.noarch 0:0.23.01-3.el7 will be installed
---> Package perl-Filter.x86_64 0:1.49-3.el7 will be installed
---> Package perl-Getopt-Long.noarch 0:2.40-2.el7 will be installed
--> Processing Dependency: perl(Pod::Usage) >= 1.14 for package: perl-Getopt-Long-2.40-2.el7.noarch
--> Processing Dependency: perl(Text::ParseWords) for package: perl-Getopt-Long-2.40-2.el7.noarch
---> Package perl-PathTools.x86_64 0:3.40-5.el7 will be installed
---> Package perl-Pod-Simple.noarch 1:3.28-4.el7 will be installed
--> Processing Dependency: perl(Pod::Escapes) >= 1.04 for package: 1:perl-Pod-Simple-3.28-4.el7.noarch
--> Processing Dependency: perl(Encode) for package: 1:perl-Pod-Simple-3.28-4.el7.noarch
---> Package perl-Scalar-List-Utils.x86_64 0:1.27-248.el7 will be installed
---> Package perl-Socket.x86_64 0:2.010-3.el7 will be installed
---> Package perl-Storable.x86_64 0:2.45-3.el7 will be installed
---> Package perl-Time-HiRes.x86_64 4:1.9725-3.el7 will be installed
---> Package perl-Time-Local.noarch 0:1.2300-2.el7 will be installed
---> Package perl-constant.noarch 0:1.27-2.el7 will be installed
---> Package perl-macros.x86_64 4:5.16.3-286.el7 will be installed
---> Package perl-threads.x86_64 0:1.87-4.el7 will be installed
---> Package perl-threads-shared.x86_64 0:1.43-6.el7 will be installed
---> Package vim-filesystem.x86_64 2:7.4.160-1.el7 will be installed
--> Running transaction check
---> Package perl-Encode.x86_64 0:2.51-7.el7 will be installed
---> Package perl-Pod-Escapes.noarch 1:1.04-286.el7 will be installed
---> Package perl-Pod-Usage.noarch 0:1.63-3.el7 will be installed
--> Processing Dependency: perl(Pod::Text) >= 3.15 for package: perl-Pod-Usage-1.63-3.el7.noarch
--> Processing Dependency: perl-Pod-Perldoc for package: perl-Pod-Usage-1.63-3.el7.noarch
---> Package perl-Text-ParseWords.noarch 0:3.29-4.el7 will be installed
--> Running transaction check
---> Package perl-Pod-Perldoc.noarch 0:3.20-4.el7 will be installed
--> Processing Dependency: perl(parent) for package: perl-Pod-Perldoc-3.20-4.el7.noarch
--> Processing Dependency: perl(HTTP::Tiny) for package: perl-Pod-Perldoc-3.20-4.el7.noarch
--> Processing Dependency: groff-base for package: perl-Pod-Perldoc-3.20-4.el7.noarch
---> Package perl-podlators.noarch 0:2.5.1-3.el7 will be installed
--> Running transaction check
---> Package groff-base.x86_64 0:1.22.2-8.el7 will be installed
---> Package perl-HTTP-Tiny.noarch 0:0.033-3.el7 will be installed
---> Package perl-parent.noarch 1:0.225-244.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

================================================================================
 Package                     Arch        Version                Repository
                                                                           Size
================================================================================
Installing:
 vim-enhanced                x86_64      2:7.4.160-1.el7        base      1.0 M
Installing for dependencies:
 gpm-libs                    x86_64      1.20.7-5.el7           base       32 k
 groff-base                  x86_64      1.22.2-8.el7           base      942 k
 perl                        x86_64      4:5.16.3-286.el7       base      8.0 M
 perl-Carp                   noarch      1.26-244.el7           base       19 k
 perl-Encode                 x86_64      2.51-7.el7             base      1.5 M
 perl-Exporter               noarch      5.68-3.el7             base       28 k
 perl-File-Path              noarch      2.09-2.el7             base       26 k
 perl-File-Temp              noarch      0.23.01-3.el7          base       56 k
 perl-Filter                 x86_64      1.49-3.el7             base       76 k
 perl-Getopt-Long            noarch      2.40-2.el7             base       56 k
 perl-HTTP-Tiny              noarch      0.033-3.el7            base       38 k
 perl-PathTools              x86_64      3.40-5.el7             base       82 k
 perl-Pod-Escapes            noarch      1:1.04-286.el7         base       50 k
 perl-Pod-Perldoc            noarch      3.20-4.el7             base       87 k
 perl-Pod-Simple             noarch      1:3.28-4.el7           base      216 k
 perl-Pod-Usage              noarch      1.63-3.el7             base       27 k
 perl-Scalar-List-Utils      x86_64      1.27-248.el7           base       36 k
 perl-Socket                 x86_64      2.010-3.el7            base       49 k
 perl-Storable               x86_64      2.45-3.el7             base       77 k
 perl-Text-ParseWords        noarch      3.29-4.el7             base       14 k
 perl-Time-HiRes             x86_64      4:1.9725-3.el7         base       45 k
 perl-Time-Local             noarch      1.2300-2.el7           base       24 k
 perl-constant               noarch      1.27-2.el7             base       19 k
 perl-libs                   x86_64      4:5.16.3-286.el7       base      687 k
 perl-macros                 x86_64      4:5.16.3-286.el7       base       43 k
 perl-parent                 noarch      1:0.225-244.el7        base       12 k
 perl-podlators              noarch      2.5.1-3.el7            base      112 k
 perl-threads                x86_64      1.87-4.el7             base       49 k
 perl-threads-shared         x86_64      1.43-6.el7             base       39 k
 vim-common                  x86_64      2:7.4.160-1.el7        base      5.9 M
 vim-filesystem              x86_64      2:7.4.160-1.el7        base      9.6 k
 which                       x86_64      2.20-7.el7             base       41 k

Transaction Summary
================================================================================
Install  1 Package (+32 Dependent packages)

Total download size: 19 M
Installed size: 63 M
Downloading packages:
warning: /var/cache/yum/x86_64/7/base/packages/gpm-libs-1.20.7-5.el7.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID f4a80eb5: NOKEY
Public key for gpm-libs-1.20.7-5.el7.x86_64.rpm is not installed
--------------------------------------------------------------------------------
Total                                              429 kB/s |  19 MB  00:46     
Retrieving key from file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
Importing GPG key 0xF4A80EB5:
 Userid     : "CentOS-7 Key (CentOS 7 Official Signing Key) <security@centos.org>"
 Fingerprint: 6341 ab27 53d7 8a78 a7c2 7bb1 24c6 a8a7 f4a8 0eb5
 Package    : centos-release-7-2.1511.el7.centos.2.10.x86_64 (@CentOS)
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : 2:vim-filesystem-7.4.160-1.el7.x86_64                       1/33 
  Installing : 2:vim-common-7.4.160-1.el7.x86_64                           2/33 
  Installing : gpm-libs-1.20.7-5.el7.x86_64                                3/33 
  Installing : groff-base-1.22.2-8.el7.x86_64                              4/33 
  Installing : 1:perl-parent-0.225-244.el7.noarch                          5/33 
  Installing : perl-HTTP-Tiny-0.033-3.el7.noarch                           6/33 
  Installing : perl-podlators-2.5.1-3.el7.noarch                           7/33 
  Installing : perl-Pod-Perldoc-3.20-4.el7.noarch                          8/33 
  Installing : 1:perl-Pod-Escapes-1.04-286.el7.noarch                      9/33 
  Installing : perl-Text-ParseWords-3.29-4.el7.noarch                     10/33 
  Installing : perl-Encode-2.51-7.el7.x86_64                              11/33 
  Installing : perl-Pod-Usage-1.63-3.el7.noarch                           12/33 
  Installing : 4:perl-libs-5.16.3-286.el7.x86_64                          13/33 
  Installing : 4:perl-macros-5.16.3-286.el7.x86_64                        14/33 
  Installing : perl-Storable-2.45-3.el7.x86_64                            15/33 
  Installing : perl-Exporter-5.68-3.el7.noarch                            16/33 
  Installing : perl-constant-1.27-2.el7.noarch                            17/33 
  Installing : perl-Time-Local-1.2300-2.el7.noarch                        18/33 
  Installing : perl-Socket-2.010-3.el7.x86_64                             19/33 
  Installing : perl-Carp-1.26-244.el7.noarch                              20/33 
  Installing : 4:perl-Time-HiRes-1.9725-3.el7.x86_64                      21/33 
  Installing : perl-PathTools-3.40-5.el7.x86_64                           22/33 
  Installing : perl-Scalar-List-Utils-1.27-248.el7.x86_64                 23/33 
  Installing : perl-File-Temp-0.23.01-3.el7.noarch                        24/33 
  Installing : perl-File-Path-2.09-2.el7.noarch                           25/33 
  Installing : perl-threads-shared-1.43-6.el7.x86_64                      26/33 
  Installing : perl-threads-1.87-4.el7.x86_64                             27/33 
  Installing : perl-Filter-1.49-3.el7.x86_64                              28/33 
  Installing : 1:perl-Pod-Simple-3.28-4.el7.noarch                        29/33 
  Installing : perl-Getopt-Long-2.40-2.el7.noarch                         30/33 
  Installing : 4:perl-5.16.3-286.el7.x86_64                               31/33 
  Installing : which-2.20-7.el7.x86_64                                    32/33 
install-info: No such file or directory for /usr/share/info/which.info.gz
  Installing : 2:vim-enhanced-7.4.160-1.el7.x86_64                        33/33 
  Verifying  : 2:vim-common-7.4.160-1.el7.x86_64                           1/33 
  Verifying  : perl-HTTP-Tiny-0.033-3.el7.noarch                           2/33 
  Verifying  : perl-threads-shared-1.43-6.el7.x86_64                       3/33 
  Verifying  : perl-Storable-2.45-3.el7.x86_64                             4/33 
  Verifying  : perl-Exporter-5.68-3.el7.noarch                             5/33 
  Verifying  : perl-constant-1.27-2.el7.noarch                             6/33 
  Verifying  : perl-PathTools-3.40-5.el7.x86_64                            7/33 
  Verifying  : 4:perl-libs-5.16.3-286.el7.x86_64                           8/33 
  Verifying  : 4:perl-macros-5.16.3-286.el7.x86_64                         9/33 
  Verifying  : 1:perl-parent-0.225-244.el7.noarch                         10/33 
  Verifying  : 4:perl-5.16.3-286.el7.x86_64                               11/33 
  Verifying  : which-2.20-7.el7.x86_64                                    12/33 
  Verifying  : groff-base-1.22.2-8.el7.x86_64                             13/33 
  Verifying  : perl-File-Temp-0.23.01-3.el7.noarch                        14/33 
  Verifying  : 1:perl-Pod-Simple-3.28-4.el7.noarch                        15/33 
  Verifying  : perl-Time-Local-1.2300-2.el7.noarch                        16/33 
  Verifying  : gpm-libs-1.20.7-5.el7.x86_64                               17/33 
  Verifying  : perl-Pod-Perldoc-3.20-4.el7.noarch                         18/33 
  Verifying  : perl-Socket-2.010-3.el7.x86_64                             19/33 
  Verifying  : 2:vim-filesystem-7.4.160-1.el7.x86_64                      20/33 
  Verifying  : perl-Carp-1.26-244.el7.noarch                              21/33 
  Verifying  : 2:vim-enhanced-7.4.160-1.el7.x86_64                        22/33 
  Verifying  : 4:perl-Time-HiRes-1.9725-3.el7.x86_64                      23/33 
  Verifying  : perl-Scalar-List-Utils-1.27-248.el7.x86_64                 24/33 
  Verifying  : 1:perl-Pod-Escapes-1.04-286.el7.noarch                     25/33 
  Verifying  : perl-Pod-Usage-1.63-3.el7.noarch                           26/33 
  Verifying  : perl-Encode-2.51-7.el7.x86_64                              27/33 
  Verifying  : perl-podlators-2.5.1-3.el7.noarch                          28/33 
  Verifying  : perl-Getopt-Long-2.40-2.el7.noarch                         29/33 
  Verifying  : perl-File-Path-2.09-2.el7.noarch                           30/33 
  Verifying  : perl-threads-1.87-4.el7.x86_64                             31/33 
  Verifying  : perl-Filter-1.49-3.el7.x86_64                              32/33 
  Verifying  : perl-Text-ParseWords-3.29-4.el7.noarch                     33/33 

Installed:
  vim-enhanced.x86_64 2:7.4.160-1.el7                                           

Dependency Installed:
  gpm-libs.x86_64 0:1.20.7-5.el7                                                
  groff-base.x86_64 0:1.22.2-8.el7                                              
  perl.x86_64 4:5.16.3-286.el7                                                  
  perl-Carp.noarch 0:1.26-244.el7                                               
  perl-Encode.x86_64 0:2.51-7.el7                                               
  perl-Exporter.noarch 0:5.68-3.el7                                             
  perl-File-Path.noarch 0:2.09-2.el7                                            
  perl-File-Temp.noarch 0:0.23.01-3.el7                                         
  perl-Filter.x86_64 0:1.49-3.el7                                               
  perl-Getopt-Long.noarch 0:2.40-2.el7                                          
  perl-HTTP-Tiny.noarch 0:0.033-3.el7                                           
  perl-PathTools.x86_64 0:3.40-5.el7                                            
  perl-Pod-Escapes.noarch 1:1.04-286.el7                                        
  perl-Pod-Perldoc.noarch 0:3.20-4.el7                                          
  perl-Pod-Simple.noarch 1:3.28-4.el7                                           
  perl-Pod-Usage.noarch 0:1.63-3.el7                                            
  perl-Scalar-List-Utils.x86_64 0:1.27-248.el7                                  
  perl-Socket.x86_64 0:2.010-3.el7                                              
  perl-Storable.x86_64 0:2.45-3.el7                                             
  perl-Text-ParseWords.noarch 0:3.29-4.el7                                      
  perl-Time-HiRes.x86_64 4:1.9725-3.el7                                         
  perl-Time-Local.noarch 0:1.2300-2.el7                                         
  perl-constant.noarch 0:1.27-2.el7                                             
  perl-libs.x86_64 4:5.16.3-286.el7                                             
  perl-macros.x86_64 4:5.16.3-286.el7                                           
  perl-parent.noarch 1:0.225-244.el7                                            
  perl-podlators.noarch 0:2.5.1-3.el7                                           
  perl-threads.x86_64 0:1.87-4.el7                                              
  perl-threads-shared.x86_64 0:1.43-6.el7                                       
  vim-common.x86_64 2:7.4.160-1.el7                                             
  vim-filesystem.x86_64 2:7.4.160-1.el7                                         
  which.x86_64 0:2.20-7.el7                                                     

Complete!
 ---> e04d0cb90025
Removing intermediate container e14f51c68588
Step 5 : RUN yum install -y postgresql-bdr94-bdr
 ---> Running in 6fa9e3415751
Loaded plugins: fastestmirror, ovl
Loading mirror speeds from cached hostfile
 * base: centos.ar.host-engine.com
 * extras: centos.ar.host-engine.com
 * updates: centos.ar.host-engine.com
Resolving Dependencies
--> Running transaction check
---> Package postgresql-bdr94-bdr.x86_64 0:1.0.2-1_2ndQuadrant.el7.centos will be installed
--> Processing Dependency: postgresql-bdr94(x86-64) >= 9.4.10_bdr1 for package: postgresql-bdr94-bdr-1.0.2-1_2ndQuadrant.el7.centos.x86_64
--> Processing Dependency: postgresql-bdr94-contrib(x86-64) >= 9.4.10_bdr1 for package: postgresql-bdr94-bdr-1.0.2-1_2ndQuadrant.el7.centos.x86_64
--> Processing Dependency: postgresql-bdr94-server(x86-64) >= 9.4.10_bdr1 for package: postgresql-bdr94-bdr-1.0.2-1_2ndQuadrant.el7.centos.x86_64
--> Processing Dependency: libpq.so.5()(64bit) for package: postgresql-bdr94-bdr-1.0.2-1_2ndQuadrant.el7.centos.x86_64
--> Running transaction check
---> Package postgresql-bdr94.x86_64 0:9.4.10_bdr1-1_2ndQuadrant.el7.centos will be installed
---> Package postgresql-bdr94-contrib.x86_64 0:9.4.10_bdr1-1_2ndQuadrant.el7.centos will be installed
--> Processing Dependency: libxslt.so.1(LIBXML2_1.0.11)(64bit) for package: postgresql-bdr94-contrib-9.4.10_bdr1-1_2ndQuadrant.el7.centos.x86_64
--> Processing Dependency: libxslt.so.1(LIBXML2_1.0.18)(64bit) for package: postgresql-bdr94-contrib-9.4.10_bdr1-1_2ndQuadrant.el7.centos.x86_64
--> Processing Dependency: libxslt.so.1(LIBXML2_1.0.22)(64bit) for package: postgresql-bdr94-contrib-9.4.10_bdr1-1_2ndQuadrant.el7.centos.x86_64
--> Processing Dependency: libxslt.so.1()(64bit) for package: postgresql-bdr94-contrib-9.4.10_bdr1-1_2ndQuadrant.el7.centos.x86_64
---> Package postgresql-bdr94-libs.x86_64 0:9.4.10_bdr1-1_2ndQuadrant.el7.centos will be installed
---> Package postgresql-bdr94-server.x86_64 0:9.4.10_bdr1-1_2ndQuadrant.el7.centos will be installed
--> Running transaction check
---> Package libxslt.x86_64 0:1.1.28-5.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

================================================================================
 Package          Arch   Version      Repository                           Size
================================================================================
Installing:
 postgresql-bdr94-bdr
                  x86_64 1.0.2-1_2ndQuadrant.el7.centos
                                      postgresql-bdr94-2ndquadrant-redhat 271 k
Installing for dependencies:
 libxslt          x86_64 1.1.28-5.el7 base                                242 k
 postgresql-bdr94 x86_64 9.4.10_bdr1-1_2ndQuadrant.el7.centos
                                      postgresql-bdr94-2ndquadrant-redhat 1.0 M
 postgresql-bdr94-contrib
                  x86_64 9.4.10_bdr1-1_2ndQuadrant.el7.centos
                                      postgresql-bdr94-2ndquadrant-redhat 607 k
 postgresql-bdr94-libs
                  x86_64 9.4.10_bdr1-1_2ndQuadrant.el7.centos
                                      postgresql-bdr94-2ndquadrant-redhat 205 k
 postgresql-bdr94-server
                  x86_64 9.4.10_bdr1-1_2ndQuadrant.el7.centos
                                      postgresql-bdr94-2ndquadrant-redhat 3.9 M

Transaction Summary
================================================================================
Install  1 Package (+5 Dependent packages)

Total download size: 6.2 M
Installed size: 28 M
Downloading packages:
warning: /var/cache/yum/x86_64/7/postgresql-bdr94-2ndquadrant-redhat/packages/postgresql-bdr94-bdr-1.0.2-1_2ndQuadrant.el7.centos.x86_64.rpm: Header V4 RSA/SHA1 Signature, key ID 6e192b0e: NOKEY
Public key for postgresql-bdr94-bdr-1.0.2-1_2ndQuadrant.el7.centos.x86_64.rpm is not installed
--------------------------------------------------------------------------------
Total                                               96 kB/s | 6.2 MB  01:06     
Retrieving key from file:///etc/pki/rpm-gpg/RPM-GPG-KEY-2NDQ-BDR-94
Importing GPG key 0x6E192B0E:
 Userid     : "BDR 9.4 Signing Key for 2ndQuadrant <packagers@2ndquadrant.com>"
 Fingerprint: 9793 74c1 0580 940e 9611 1be3 a879 b734 6e19 2b0e
 Package    : postgresql-bdr94-2ndquadrant-redhat-1.0-3.noarch (@/postgresql-bdr94-2ndquadrant-redhat-latest.noarch)
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-2NDQ-BDR-94
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : postgresql-bdr94-libs-9.4.10_bdr1-1_2ndQuadrant.el7.centos   1/6 
  Installing : postgresql-bdr94-9.4.10_bdr1-1_2ndQuadrant.el7.centos.x86_   2/6 
  Installing : postgresql-bdr94-server-9.4.10_bdr1-1_2ndQuadrant.el7.cent   3/6 
  Installing : libxslt-1.1.28-5.el7.x86_64                                  4/6 
  Installing : postgresql-bdr94-contrib-9.4.10_bdr1-1_2ndQuadrant.el7.cen   5/6 
  Installing : postgresql-bdr94-bdr-1.0.2-1_2ndQuadrant.el7.centos.x86_64   6/6 
  Verifying  : postgresql-bdr94-libs-9.4.10_bdr1-1_2ndQuadrant.el7.centos   1/6 
  Verifying  : postgresql-bdr94-9.4.10_bdr1-1_2ndQuadrant.el7.centos.x86_   2/6 
  Verifying  : postgresql-bdr94-server-9.4.10_bdr1-1_2ndQuadrant.el7.cent   3/6 
  Verifying  : postgresql-bdr94-bdr-1.0.2-1_2ndQuadrant.el7.centos.x86_64   4/6 
  Verifying  : postgresql-bdr94-contrib-9.4.10_bdr1-1_2ndQuadrant.el7.cen   5/6 
  Verifying  : libxslt-1.1.28-5.el7.x86_64                                  6/6 

Installed:
  postgresql-bdr94-bdr.x86_64 0:1.0.2-1_2ndQuadrant.el7.centos                  

Dependency Installed:
  libxslt.x86_64 0:1.1.28-5.el7                                                 
  postgresql-bdr94.x86_64 0:9.4.10_bdr1-1_2ndQuadrant.el7.centos                
  postgresql-bdr94-contrib.x86_64 0:9.4.10_bdr1-1_2ndQuadrant.el7.centos        
  postgresql-bdr94-libs.x86_64 0:9.4.10_bdr1-1_2ndQuadrant.el7.centos           
  postgresql-bdr94-server.x86_64 0:9.4.10_bdr1-1_2ndQuadrant.el7.centos         

Complete!
 ---> 79f22111083a
Removing intermediate container 6fa9e3415751
Successfully built 79f22111083a
```

### Run the new machine

```
cepxio@corvette:~/tmp/CentOS-PostgreSQL-BDR_tests$ docker run -t -i centos_pgbdr96-4
[root@49dffc23fab1 /]# 
[root@49dffc23fab1 /]# 
[root@49dffc23fab1 /]# su - postgres
-bash-4.2$ /usr/pgsql-9.4/bin/initdb -D /var/lib/pgsql/9.4-bdr/data/ -A trust -U postgres
The files belonging to this database system will be owned by user "postgres".
This user must also own the server process.

The database cluster will be initialized with locale "C".
The default database encoding has accordingly been set to "SQL_ASCII".
The default text search configuration will be set to "english".

Data page checksums are disabled.

fixing permissions on existing directory /var/lib/pgsql/9.4-bdr/data ... ok
creating subdirectories ... ok
selecting default max_connections ... 100
selecting default shared_buffers ... 128MB
selecting dynamic shared memory implementation ... posix
creating configuration files ... ok
creating template1 database in /var/lib/pgsql/9.4-bdr/data/base/1 ... ok
initializing pg_authid ... ok
initializing dependencies ... ok
creating system views ... ok
loading system objects' descriptions ... ok
creating collations ... ok
creating conversions ... ok
creating dictionaries ... ok
setting privileges on built-in objects ... ok
creating information schema ... ok
loading PL/pgSQL server-side language ... ok
vacuuming database template1 ... ok
copying template1 to template0 ... ok
copying template1 to postgres ... ok
syncing data to disk ... ok

Success. You can now start the database server using:

    /usr/pgsql-9.4/bin/postgres -D /var/lib/pgsql/9.4-bdr/data/
or
    /usr/pgsql-9.4/bin/pg_ctl -D /var/lib/pgsql/9.4-bdr/data/ -l logfile start

-bash-4.2$ /usr/pgsql-9.4/bin/pg_ctl -D /var/lib/pgsql/9.4-bdr/data/ -l logfile start
server starting
-bash-4.2$ psql
psql (9.4.10)
Type "help" for help.

postgres=# show server_version;
 server_version 
----------------
 9.4.10
(1 row)

postgres=# 
```

### Con esto tenemos un laboratorio con Docker listo para probar BDR
### Ademas, esto muestra que aun no hay BDR para 9.6, es 9.4.