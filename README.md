# centos7-linux


## 1. /etc/hosts 설정
```
  vi /etc/hosts
  127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
  ::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
  [설치될 PC 아이피] beyond
```

## 2. /etc/hostname 설정
```
  vi /etc/hostname
  beyond
```

## 3. /etc/sysctl.conf 안에 내용 추가
```
  vi /etc/sysctl.conf
  fs.file-max = 6815744
  kernel.sem = 250 32000 100 128
  kernel.shmmni = 4096
  kernel.shmall = 1073741824
  kernel.shmmax = 4398046511104
  kernel.panic_on_oops = 1
  net.core.rmem_default = 262144
  net.core.rmem_max = 4194304
  net.core.wmem_default = 262144
  net.core.wmem_max = 1048576
  net.ipv4.conf.all.rp_filter = 2
  net.ipv4.conf.default.rp_filter = 2
  fs.aio-max-nr = 1048576
  net.ipv4.ip_local_port_range = 9000 65500
```

## 4. 파일 생성 후 내용 추가
```
  vi /etc/security/limits.d/oracle-database-preinstall-19c.conf
  oracle   soft   nofile    1024
  oracle   hard   nofile    65536
  oracle   soft   nproc    16384
  oracle   hard   nproc    16384
  oracle   soft   stack    10240
  oracle   hard   stack    32768
  oracle   hard   memlock    134217728
  oracle   soft   memlock    134217728
```

## 5. 사용자 및 그룹 생성
```
  groupadd -g 54321 oinstall
  groupadd -g 54322 dba
  groupadd -g 54323 oper
  groupadd -g 54324 backupdba
  groupadd -g 54325 dgdba
  groupadd -g 54326 kmdba
  groupadd -g 54327 asmdba
  groupadd -g 54330 racdba

  useradd -u 54321 -g oinstall -G dba,oper,backupdba,dgdba,kmdba,asmdba,racdba oracle
```

## 6. 오라클 패스워드 설정
```
  passwd oracle
```
<img src="/img/1.png"></img>

## 7. selinux desabled 설정
```
  vi /etc/selinux/config
  SELINUX=disabled
```
<img src="/img/2.png"></img>

## 8. 방화벽 내리기
```
  systemctl disable firewalld
```

## 9. 설치 디렉토리 생성 및 권한 설정
```
  mkdir -p /home/app/oracle/product/19.3.0/db_1/
  mkdir -p /home/app/oradata
  chown -R oracle:oinstall /home
  chmod -R 775 /home
```

## 10. 패키지 설치
```
  yum -y install ksh
  yum -y install libaio-devel
  yum -y install compat-libcap1
  yum -y install compat-libstdc++-33
  yum -y install glibc-devel
  yum -y install libstdc++-devel
  yum -y install gcc-c++
```

## 11. 오라클 계정 이동
```
  su - oracle
```

## 12. .bash_profile 내용 추가 및 편집
```
  vi .bash_profile

  export ORACLE_HOSTNAME=orcl
  export ORACLE_UNQNAME=orcl
  export ORACLE_BASE=/home/app/oracle
  export ORACLE_HOME=$ORACLE_BASE/product/19.3.0/db_1
  export ORA_INVENTORY=/home/oraInventory
  export ORACLE_SID=orcl
  export DATA_DIR=/home/app/oradata
  export PATH=/usr/sbin:/usr/local/bin:$PATH
  export PATH=$ORACLE_HOME/bin:$PATH
  export LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib
  export CLASSPATH=$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib
```
<img src="/img/3.png"></img>

## 13. 오라클 패키지 파일로 이동 및 압축풀기
```
  cd /home/app/oracle/product/19.3.0/db_1
  unzip LINUX.X64_193000_db_home.zip
```
<img src="/img/4.png"></img>

## 14. 오라클 설치
```
  ./runInstaller
```
<img src="/img/5.png"></img>
<img src="/img/6.png"></img>
<img src="/img/7.png"></img>
<img src="/img/8.png"></img>
<img src="/img/9.png"></img>

# 완료~!
* 참고 자료1 : <https://x486.tistory.com/150#recentComments><br>
* 참고 자료2 : <https://velog.io/@junbeomm-park/Linux에-Oracle19c-설치-방법-with-VMware-CentOS>
