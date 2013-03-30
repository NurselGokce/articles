Akdeniz Üniversitesi [Akademik Bilişim 2013](http://ab2013.akdeniz.edu.tr/)'de, [Devrim Gündüz](http://www.gunduz.org) tarafından [PostgreSQL](http://www.postgresql.org/) eğitiminde anlatılan [konulardan](http://ab.org.tr/ab13/postgresql.html) derlenmiştir.

## User ve Role

* **Role** User
 * Ön tanımlı olarak login olma hakkına sahip değildir.
* **Role** User + Role
 * Ön tanımlı olarak login olma hakkına sahip değildir.

[more]

```sql
CREATE ROLE user_name; -- Role oluşturma
DROP ROLE user_name; -- Role silme
-- ---
CREATE USER user_name;
```

### Parametreler
* CREATEDB: Rol ile aynı isimde veri tabanı oluşturur.
* INHERIT: Bir rolün başka bir rolden miras alıp almayacağını belirler.
* REPLICATION: Kullanıcının replication yapıp yapamayacagı
* CONNECTION: Kullanıcıya bağlantı sınırlaması vermemizi sağlar.
* ENCRYPTED: Şifre belrilemenizi sağlar.

```sql
-- Oluşturma
CREATE ROLE tayfun 
  CREATEDB 
  ENCRYPTED PASSWORD 'ab2013';

-- Düzenleme
ALTER ROLE tayfun 
  ENCRYPTED PASSWORD 'ab2013';
```

### pg_hba.conf

* **TYPE** 
 * local: Local sunucuya bağlanmak
 * host: TCP/IP uzerınden baska sunucuya bağlanmak (SSL kullanılabilir)
* **DATABASE** 
 * all 
 * db1, db2, ...
* **USER**
 * all
 * user1, user2, ...
* **ADDRESS**
 * 127.0.0.1/32
 * 10.1.1.0/24 -> 1.255, C Class
 * 10.1.1.0/16 -> B Class
 * 10.1.1.0/8 -> A Class
 * 0.0.0.0/0 -> Tüm networkler
* **METHOD**
 * peer - Postgres kullanıcısı dışında kimse bağlanamaz
 * trust
 * password
 * reject - Bu kuralla eşleşen kişiyi alma
 * ldap
 * radius

```bash
# TYPE  DATABASE        USER            ADDRESS                 METHOD
# "local" is for Unix domain socket connections only
local   all             all                                     peer
# IPv4 local connections:
host    all             all             127.0.0.1/32            ident
# IPv6 local connections:
host    all             all             ::1/128                 ident
# Allow replication connections from localhost, by a user with the
# replication privilege.
#local   replication     postgres                                peer
#host    replication     postgres        127.0.0.1/32            ident
#host    replication     postgres        ::1/128                 ident

```

## Kullanıcı Şifresini Belirlemek

* root kullanıcısına geçin `$>su -`
* Konsola `$>su - postgres` yazarak postgres kullanıcısına geçin 
* Bir editörle (vi, nano) `pg_hba.conf` dosyasındaki local satırında bulunan `MD5` ayarını `peer` olarak değiştirin
* Konsoldan `$>psql` kullanarak postgres kullanıcısı ile oturum açın.
* `ALTER USER postgres password 'ab2013';` ile istediğiniz bir şifre belirleyin.
* \q ile psql'den çıkın.
* `pg_hba.conf` dosyasındaki local satırında bulunan `peer` satırını `MD5` olarak değiştirin. 
* root kullanıcısına geri dönün `$>exit`
* Konsolda `$>systemctl reload postgresql-9.2.service` komutu ile servisi yenileyin.
* postgres kullanıcısına tekrar geçin `$>su - postgres`
* `psql` ile oturum açın. Bu sefer size şifre soracaktır.
* `postgres` kullanıcısı için az önce yazdığınız şifreyi tekrar girin.

---
* http://ab.org.tr/ab13/postgresql.html

### Etiketler
postgresql, postgres, ab2013, user, role, yetkilendirme, kullanıcı, rol, pg_hba.conf