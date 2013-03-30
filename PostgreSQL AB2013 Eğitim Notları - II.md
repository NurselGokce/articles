Akdeniz Üniversitesi [Akademik Bilişim 2013](http://ab2013.akdeniz.edu.tr/)'de, [Devrim Gündüz](http://www.gunduz.org) tarafından [PostgreSQL](http://www.postgresql.org/) eğitiminde anlatılan [konulardan](http://ab.org.tr/ab13/postgresql.html) derlenmiştir.

## Trigger

```sql
CREATE OR REPLACE FUNCTION satislar_insert_trigger() RETURNS TRIGGER AS 
  $$ 
    BEGIN 
      IF (NEW.satis_tarihi >= DATE '2013-01-01' AND NEW.satis_tarihi < DATE '2013-02-01' ) THEN 
        INSERT INTO satislar_2013_01 VALUES (NEW.*); 
      ELSIF (NEW.satis_tarihi >= DATE '2013-02-01' AND NEW.satis_tarihi < DATE '2013-03-01' ) THEN 
        INSERT INTO satislar_2013_02 VALUES (NEW.*); 
      ELSIF (NEW.satis_tarihi >= DATE '2013-03-01' AND NEW.satis_tarihi < DATE '2013-04-01' ) THEN 
        INSERT INTO satislar_2013_03 VALUES (NEW.*); 
      ELSIF (NEW.satis_tarihi >= DATE '2013-04-01' AND NEW.satis_tarihi < DATE '2013-05-01' ) THEN 
        INSERT INTO satislar_2013_04 VALUES (NEW.*); 
      ELSE 
        RAISE EXCEPTION ' Eksik tablo var! ' ; 
      END IF; 
      RETURN NULL; 
    END; 
  $$ 
LANGUAGE plpgsql;

CREATE TRIGGER satislar_insert_trigger
  BEFORE INSERT ON satislar 
    FOR EACH ROW EXECUTE PROCEDURE satislar_insert_trigger();

```

[more]

```sql
CREATE INDEX t1_c1_idx ON t1(c1) WHERE ....;
```

## Key Sensitive
```sql
WHERE il ILIKE '%Ankara%'; -- Ankara, ANKARA, ankara

CREATE INDEX t1_c1_idx ON t1(c1) WHERE upper((ad||' 'soyad)); -- Birleştirip indexler.
```

* Tüm indexleri child tablolarda oluşturacağız.

## Indexler
Indexler BTREE kullanır.

### Partial Index
Verinin sadece belirli bir bölümünü indexler. Koşul sabit geliyorsa çok kullanışlı olur.

## Configuration

* **port** Default portu değiştirmek içindir.
* **max_connections** Aynı anda kaç kişinin bağlanabileceğini anlatır.
* **superuser_reserved_connections** Sadece super user'a ayrılmış bağlantı sayısı.
 * Bir uygulamaya asla postgres kullanıcısı verilmez. Bu kullanıcı yönetim için kullanılmalıdır.
* **shared_buffers** 
 * Birinci kural RAM'ın yarısından fazla olmayacak. En az %25'i en fazla %50'si.
 * En fazla 8 GB vermek gerekir 64 Bit'ler için. 32 Bit 2.7 GB
* **work_mem** Hash operasyonları (order by ve distinct) için kullanılan ram miktarıdır. 
 * Performansı olumlu şekilde oldukça etkileyebilir.
 * Orn: Her bağlantıdaki her sql sorgusu için.
 * 1,2,4 MB başlangıç için iyidir.

### Konfigurasyon sonrası sunucuyu başlatmak
```bash
$> /usr/pgsql-9.2/bin/pg_ctl -D /pat/to/db_folder start/reload/stop(smart, fast, immediate)/start
# smart: rollback yapar
# immediate: roolback yapmadan direkt kapatır.
```

---
* http://ab.org.tr/ab13/postgresql.html

### Etiketler
postgresql, postgres, ab2013, user, role, yetkilendirme, kullanıcı, rol, pg_hba.conf