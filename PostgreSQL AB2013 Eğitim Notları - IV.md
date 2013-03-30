## Replication (Replikasyon)

Master ve Slave sunucular ayarlayarak birbirlerini nasıl replike edeceğini ve configurasyonun nasıl yapılacağını üç ana aşamada göreceğiz.

[more]

### Konfigurasyon

```bash
# Arşivlemeyi açar
archive_mode = on

# Kopyalamayı hangi linux komutuyla yapacağını belirler
# %P: Path, Arşivlenmesi gereken XLOG'un tam yolu
# %f: File name 
archive_command = 'cp %P /var/lib/pgsql/archive/%f'

# En fazla aynı anda kaç tane wall sender process'inin çalışacağını belirler
# Standby varsa +1,  ön tanımlı değeri 0'dır.
max_wal_senders = 2 # Örneğimizde 2 tane = 1 standby + 1 yedek

# Transaction dosyası içerisine yazılacak bilginin yoğunluğunu belirler.
# minimal, archive, hot_standby
# minimal: en temel özelliğidir. En az bilgi yazılır.
# hot_standby: çalışma esnasında sorgulanabilmesi. En çok az bilgi yazılır.
# archive: Büyük yedeklerin geri alınması için
wal_level = hot_standby

# Eğer aradaki network sık sık kopuyorsa 1000 segment verebilirisiniz.
# Her bir segment 16 MB'dır. 0 bırakırsanız disable edilir.
wal_keep_segments = 256 
```

#### Workshop
* Konsoldan `$>mkdir /var/lib/pgsql/archive` adında bir dizin oluşturun.
* Konsoldan `$>nano postgresql.conf` komutu ile, dosyasını yukarıdaki parametrelere göre düzenleyin.
* Konsoldan `$>nano pg_hba.conf` komutu ile ile **replication** DATABASE'ine, **replication** USER'ı olarak **trust** METHOD'unu verin.
* Şimdi replication adında bir kullanıcı oluşturun.
* Konsoldan `$>psql` ile sunucuya bağlanın
* Konsoldan `CREATE USER replication ENCRYPTED PASSWORD 'rep';` ile **replication** adında ve **rep** şifresine sahip bir kullanıcı oluşturun.
* Konsoldan `ALTER USER replication REPLICATION;` komutunu girerek **replication** kullanıcısına **REPLICATION** haklarını atayın.
* Tekrar root kullanıcısına geri dönün.
* PostgreSQL servisini konsoldan `systemctl restart postgresql-9.2.service;` komutu ile yeniden başlatın.
* postgres kullanıcısna geçin.
* psql ile sunucuya bağlanın.
* Şu komutla `CREATE TABLE t11 (c1 int);` t11 adında bir tablo oluşturun. 
* Bu tabloya 10 milyon rastgele veri girin. Bunun için şu SQL'i kullanabilirsiniz. `INSERT INTO t11 (SELECT generate_series(1, 10000000));` (Bu işle bilgisayarınızın performansına göre biraz zaman alabilir. Bu aşamada girilen kayıtlar bir taraftan da **archive_command** parametresinde belirtilen dizine arşivlenmektedir.
* Konsoldan `$>ls /var/lib/pgsql/archive/` ile oluşturulan arşivleri gözlemleyin. Eğer aşiv dosyalar oluşturulmamıs ise yukarıdaki işlemlerden birisinde hata yapmış olabilirsiniz. *Örneğin ben archive_command parametresini hatalı giriğim inin arşıvleme yapmamıştı. Devrim Gündüz' ün yardımıyla ilgili parametreyi düzeltip, servisi yeniden başlattık. Ardından t11 tablosundaki tüm verileri silip (TRUNCATE t11;) tekrar 10 milyon satırı veri tabanına eklediğimizde arşivlerin oluştuğunu gözlemledik. Teşekkürler Devrim üstad.*

### Base Backup Almak
`/var/lib/pgsql/9.2/data` dizinindeki dosyaları Replikasyon sunucusuna göndermek. Bu işlem için genelde kullanılan harika komut `rsync` dir. Ancak biz uygulamada (replikasyon aynı sucunuda ayarlandıgı için) `cp` kullanacağız.

#### Workshop
* cd /var/lib/pgsql/9.2
* Konsoldan `$>psql -c "SELECT pg_start_backup('ab2013')"` komutunu çalıştırın. 
* Konsoldan `$>cp -r data/ standbydata` ile data dizinini ve tüm alt dizinlerin **standbydata** dizinine klonlayın.
* Konsoldan tekrar `$>psql -c "SELECT pg_stop_backup()"` komutunu çalıştırın.

### Standby Sunucuyu Ayaklandırmak

Standy sunucusunu hazırlamak için aşağıdaki workshop işlemlerini yapıyoruz. Mevcut master postgresql sunucuyu klonladıktan sonra yeni olusturulan standbydata dizininde bulunan slave postgresql sunucusunun hot_standby'da calısması için gerekli ayarlamaları aşağıda yapacağız.

#### Workshop (a)
* cd standbydata/
* rm postmaster.pid
* rm -f pg_xlog/00*
* nano postgresql.conf dosyasını açıyoruz ve yukarıdaki 5 parametreyi geri alıyoruz.
 * Bu dosya master sunucudan klonlanan ayarları içermektedir. Bu ayar standby sunucusu ıcın bu ayarları geri default'a almak için yapılıyor.
 * postgresql.conf içindeki hot_standby = on parametresini off'dan on'a getirin.
 * port'u değiştirin. Örn: port = 5440
 * dosyayı kaydedin ve kapatın.

#### Workshop (b)
* Slave postgresql sunucusunun standbydata dizininin içine `$>nano recovery.conf` adında bir dosya oluşturacağız.
* Bu dosyanın içine aşağıdaki satırları yazın ve dosyayı kaydedip çıkın.
* Standy modunu aç/kapa:  `standby_mode = on`
* Bağlantı bilgileri: `primary_coninfo = 'host=localhost port=5432 user=replication password=rep'`
* Dosyaları geri kopyalama komutu: `restore_command = 'cp /var/lib/pgsql/archive/%f %p'`
* Master sunucuda problem oldugunda yada diğer replikasyonu devreye almak istediğinizde bu dizine belirtilen isimde bir dosya oluşturulur ve slave sunucu ayaklanır. `trigger_file = '/var/lib/pgsql/finish.replication'`
* Bu komutu uygulamamızda yazmıyoruz sadece not amaçlı olarak ekliyorum.. `#archive_cleanup_command = '/usr/pgsql-9.2/bin/pg_archivecleanup /var/lib/pgsql/archive %r`

### Etiketler
postgresql, postgres, ab2013, replication, replikasyon