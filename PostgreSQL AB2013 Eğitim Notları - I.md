Akdeniz Üniversitesi [Akademik Bilişim 2013](http://ab2013.akdeniz.edu.tr/)'de, [Devrim Gündüz](http://www.gunduz.org) tarafından [PostgreSQL](http://www.postgresql.org/) eğitiminde anlatılan [konulardan](http://ab.org.tr/ab13/postgresql.html) derlenmiştir.

## Yedekleme / Geri yükleme (Backup / Restore)

### Yedekleme

#### pg_dump ( --help)

* 1 DB yedeği alır
* Yedeği sıkıştırabilir
* Global yedekleri almaz
 * User, tablespace etc.
* Sıkıştırılmış yedeği tablo bazında açma şansı verir. (Sadece sıkıştırılmış formatta ise alır)

*: Kullanın

#### pg_dumpall ( --help)

* Tüm veritabanların aynı anda yedeğini alır (cluster v.s.)
* Sadece düz metin olarak yedek alır (Text metin)
* Global bilgileri yedekler
* Özel işlemler yapamaz (tek bir tabloyu geri açma yada veri tabanı geri açma gibi)
* Global bilgileri ayrıca yedeklemek için bir seçenek mevcuttur
* INSERT yerine ön tanımlı olarak COPY yönetmi ile verileri yedekler. Sebebi ise COPY de Transaction oluşturulmaması, INSERTE ise oluşturulmasıdır.

[more]

#### Best Practices

* pg_dump ile yedekleyin, pg_dumpall ile global verileri yedekleyin.
* Önce globalleri yükle sonra veritabanı yada tablolaları yükleyin.

*pg_dump*

```bash
$> pg_dump DB_NAME -f FILE_NAME.EXT 
# DB_NAME test, FILE_NAME.EXT = backup.dump
$> pg_dump -Fc DB - f FILE 
# Custom sıkıştırılmış yedek.
$> pg_dump -t "TABLE_NAME" 
# Sadece belirtilen tabloyu yedekler.
$> pg_dump -T "TABLE_NAME" 
# Sadece belirtilen tabloların haricindekileri yedekler.
$> pg_dump -s
# Sadece STRUCTURE yani DDL
$> pg_dump -a
# Sadece DATA yani DQL yedekler
```

*pg_dumpall*

```bash
$> pg_dumpall -g -f "FILE"
# -g: sadece globalleri alır
# -r: Sadece kullanıcıları alır
# -a: DQL
# -s: DDL
```

### Geri Yükleme

#### pg_restore ( --help)

* Sadece sıkıştırılmış yedekleri geri yükleyebilir.
* -j: Kaç parçaya bölerek yüklemesi gerektiğini belirler. Tabloları paylaşır. (Jobs)

```bash
$> pg_restore -d "DB_NAME" "FILE_NAME.EXT" -j4
# pg_restore -d pagila pagila.pgdump -j4
```

---
* http://ab.org.tr/ab13/postgresql.html

### Etiketler
postgresql, postgres, ab2013, backup, restore, pg_dump, pg_dumpall, pg_restore