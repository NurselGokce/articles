### Kurulum

Kuruluma başlamadan önce

```$ sudo apt-get update && sudo apt-get upgrade```

Ardından Aşağıdaki paket kuruluyor. Bu paketleri kurmamızın sebebi  ```add-apt-repository ppa:pitti/postgresql``` komutunu çalıştırabilmek içindir. Bu komut ubuntu 12.10 ile birlikte varsayılan olarak çalışmamakta.

```$ sudo apt-get install python-software-properties &&  sudo apt-get install software-properties-common```

Bu işlemlerin ardından postgresql 9.2 için gereken paketi repoya ekliyoruz. 

```$ sudo add-apt-repository ppa:pitti/postgresql```

Bu işlemlerden sonra aşağıdaki komutla repoları güncelliyoruz.

```$ sudo apt-get update```

Son olarak aşağıdaki komut ile postgresql 9.2 kurulumunu tamamlıyoruz. Ben kurulumu yaptıgım zamanda 9.2.4 kurulumunu yapmıştı.

```console
$ sudo apt-get install postgresql-9.2
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following extra packages will be installed:
  libpq5 libxml2 postgresql-client-9.2 postgresql-client-common postgresql-common sgml-base ssl-cert xml-core
Suggested packages:
  oidentd ident-server locales-all postgresql-doc-9.2 sgml-base-doc openssl-blacklist debhelper
The following NEW packages will be installed:
  libpq5 libxml2 postgresql-9.2 postgresql-client-9.2 postgresql-client-common postgresql-common sgml-base
  ssl-cert xml-core
0 upgraded, 9 newly installed, 0 to remove and 2 not upgraded.
Need to get 6,652 kB of archives.
After this operation, 26.3 MB of additional disk space will be used.
Do you want to continue [Y/n]? Y
Get:1 http://tr.archive.ubuntu.com/ubuntu/ quantal-updates/main libxml2 amd64 2.8.0+dfsg1-5ubuntu2.2 [679 kB]
Get:2 http://ppa.launchpad.net/pitti/postgresql/ubuntu/ quantal/main libpq5 amd64 9.2.4-0ppa1~quantal [536 kB]
Get:3 http://tr.archive.ubuntu.com/ubuntu/ quantal/main ssl-cert all 1.0.32 [16.4 kB]
Get:4 http://tr.archive.ubuntu.com/ubuntu/ quantal/main sgml-base all 1.26+nmu3ubuntu1 [11.5 kB]
Get:5 http://tr.archive.ubuntu.com/ubuntu/ quantal/main xml-core all 0.13+nmu1 [23.0 kB]
Get:6 http://ppa.launchpad.net/pitti/postgresql/ubuntu/ quantal/main postgresql-client-common all 140~quantal [65.5 kB]
Get:7 http://ppa.launchpad.net/pitti/postgresql/ubuntu/ quantal/main postgresql-client-9.2 amd64 9.2.4-0ppa1~quantal [1,394 kB]
Get:8 http://ppa.launchpad.net/pitti/postgresql/ubuntu/ quantal/main postgresql-common all 140~quantal [147 kB]
Get:9 http://ppa.launchpad.net/pitti/postgresql/ubuntu/ quantal/main postgresql-9.2 amd64 9.2.4-0ppa1~quantal [3,779 kB]
Fetched 6,652 kB in 2s (2,276 kB/s)         
Preconfiguring packages ...
Selecting previously unselected package libxml2:amd64.
(Reading database ... 52826 files and directories currently installed.)
Unpacking libxml2:amd64 (from .../libxml2_2.8.0+dfsg1-5ubuntu2.2_amd64.deb) ...
Selecting previously unselected package libpq5.
Unpacking libpq5 (from .../libpq5_9.2.4-0ppa1~quantal_amd64.deb) ...
Selecting previously unselected package postgresql-client-common.
Unpacking postgresql-client-common (from .../postgresql-client-common_140~quantal_all.deb) ...
Selecting previously unselected package postgresql-client-9.2.
Unpacking postgresql-client-9.2 (from .../postgresql-client-9.2_9.2.4-0ppa1~quantal_amd64.deb) ...
Selecting previously unselected package ssl-cert.
Unpacking ssl-cert (from .../ssl-cert_1.0.32_all.deb) ...
Selecting previously unselected package postgresql-common.
Unpacking postgresql-common (from .../postgresql-common_140~quantal_all.deb) ...
Adding 'diversion of /usr/bin/pg_config to /usr/bin/pg_config.libpq-dev by postgresql-common'
Selecting previously unselected package postgresql-9.2.
Unpacking postgresql-9.2 (from .../postgresql-9.2_9.2.4-0ppa1~quantal_amd64.deb) ...
Selecting previously unselected package sgml-base.
Unpacking sgml-base (from .../sgml-base_1.26+nmu3ubuntu1_all.deb) ...
Selecting previously unselected package xml-core.
Unpacking xml-core (from .../xml-core_0.13+nmu1_all.deb) ...
Processing triggers for man-db ...
Processing triggers for ureadahead ...
ureadahead will be reprofiled on next reboot
Setting up libxml2:amd64 (2.8.0+dfsg1-5ubuntu2.2) ...
Setting up libpq5 (9.2.4-0ppa1~quantal) ...
Setting up postgresql-client-common (140~quantal) ...
Setting up postgresql-client-9.2 (9.2.4-0ppa1~quantal) ...
Setting up ssl-cert (1.0.32) ...
Setting up postgresql-common (140~quantal) ...
Adding user postgres to group ssl-cert
Building PostgreSQL dictionaries from installed myspell/hunspell packages...
 * No PostgreSQL clusters exist; see "man pg_createcluster"
Setting up sgml-base (1.26+nmu3ubuntu1) ...
Updating the super catalog...
Setting up xml-core (0.13+nmu1) ...
update-catalog: Suppressing action on super catalog. Invoking trigger instead.
update-catalog: Please rebuild the package being set up with a version of debhelper fixing #477751.
Processing triggers for ureadahead ...
Setting up postgresql-9.2 (9.2.4-0ppa1~quantal) ...
Creating new cluster (configuration: /etc/postgresql/9.2/main, data: /var/lib/postgresql/9.2/main)...
Moving configuration file /var/lib/postgresql/9.2/main/postgresql.conf to /etc/postgresql/9.2/main...
Moving configuration file /var/lib/postgresql/9.2/main/pg_hba.conf to /etc/postgresql/9.2/main...
Moving configuration file /var/lib/postgresql/9.2/main/pg_ident.conf to /etc/postgresql/9.2/main...
Configuring postgresql.conf to use port 5432...
update-alternatives: using /usr/share/postgresql/9.2/man/man1/postmaster.1.gz to provide /usr/share/man/man1/postmaster.1.gz (postmaster.1.gz) in auto mode
 * Starting PostgreSQL 9.2 database server                                                               [ OK ] 
Processing triggers for libc-bin ...
ldconfig deferred processing now taking place
Processing triggers for sgml-base ...
Updating the super catalog...
```

### Konfigurasyon

İlk yapmanız gereken postgres kullanıcısına bir şifre belirlemek olmalı. Bunun için konsola aşağıdakı komutları girip posgtres kullanıcısına bir şifre belirlemek olmalı.

```bash
$ su - postgres
$ passwd postgres
Enter new unix password:
```

Ardından Psql e geçin ve ```\password``` komutunu çalıştırın. Az once belirlediğiniz şifreyi tekrar girin. ; 
```bash
$ su - postgres psql
```

Artık postgres kullanıcısına şifre belirlemiş oldunuz.

Bu aşamadan sonra detaylı ayarları yapmak için  configurasyon dosyalarına nasıl müdehale etmeniz gerektiğini anlatan ikinci bir makale daha hazırlayacağım.
