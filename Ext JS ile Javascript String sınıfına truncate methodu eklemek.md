Merhabalar,

Stringler için Rails'te bulunan Truncate fonksiyonuna benzer bir methodu javascript ile kullanmak için, aşağıdaki singleton sınıfı kullanabilirsiniz. Bu sınıf runtime'da String sınıfına Trunacate methodunu eklemektedir. Bu yöntemle String sınıfına başka ek özellikler kazandırabilir, Ext JS projelerinizde özellikle de XTemplate kullanırken bu özelliklerden faydalanabilirsiniz.

```javascript
Ext.define('App.lib.String', {
    singleton: true,

    constructor: function() {
        // add Truncate
        String.prototype.truncate = function(limit) {
            return this.substring(0, limit || 10).concat(this.length > (limit || 10) ? ".." : "");
        };
    }
});

// Kullanımı
// "text".truncate(); --> text
// "text".truncate(2): --> te...
```


İyi çalışmalar dilerim.

### Etiketler
string, extjs, javascript, truncate