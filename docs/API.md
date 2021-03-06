![WebSqLite](http://i.hizliresim.com/1LVQY1.png)

### WebSql ve Sqlite Uyumlu SqlService Kütüphanesi
Gereksiz ve can sıkıcı SQL sorugularını yazmamak için yapılmış bir kütüphanedir. İçindeki methodlar ile basitçe gerekli olan işlerinizi halledebilirsiniz.

Toplamda 5 adet methodu var bunlar:

- [x] [select](#select)
- [x] [insert](#insert)
- [x] [update](#update)
- [x] [delete](#delete)
- [x] [query](#query)

## En Son Değişiklikler

> init(id, object) > init({id: '', dbObject: object})

## select

> table : tablonun ismini içerir.

>  field : burada çekilecek sütun isimleri yer alıyor.

>  where : Sorgunun şartı.

>  values : Sorgu şartında bulunan '?' ifadelerinin dolduracak array.

>  order : Sorguda bulunacak order işlemine ait string ifade.


```javascript
SqlService.select(table, field, where, values, order).then(res => {
  // res : dönen sonuç
});
```

basit bir select sorgusu
```javascript
SqlService.select("chatList", "*", "chatId = ?", ["C0001"]).then(res => {
    var result = res.map(x => x.message);
    console.log(result);
});
```
like kullanımına bir örnek
```javascript
SqlService.select("chatList", "*", "(message LIKE ?)", ['%avare kodcu%']).then(res => {
    var result = res.map(x => x.message);
    console.log(result);
});
```
order by kullanımı
```javascript
SqlService.select("chatList", "*", "", "", "rowid, message").then(res => {
    var result = res.map(x => x.message);
    console.log(result);
});
```

Bunlara benzer kullanımları yapabilirsiniz.

## insert

> table : tablonun ismini içerir

>  row : burada insert yapılacak sütun isimleri yer alıyor

>  values :  row içinde bulunan sütunlara ait değerler

```javascript
SqlService.insert(table, row, values).then(res => {
    //res : dönen sonuç
});
```
tekli insert işlemi
```javascript
SqlService.insert("chatList", ["message"], ["avare kodcu"]).then(res => {
    console.log(res)
});
```
çoklu insert işlemi

```javascript
SqlService.insert(
    "chatList",
    ["message"],
    [
        ["avare kodcu"],
        ["Ya olduğun gibi görün, ya göründüğün gibi ol!"]
    ]
).then(res => {
    console.log(res)
});
```

## update


> table : tablonun ismini içerir

>  row : burada insert yapılacak sütun isimleri yer alıyor

>  values : row içinde bulunan sütunlara ait değerler

>  where: Sorgu şartı

>  wValues : Sorgu şartında bulunan '?' ifadelerinin dolduracak array

```javascript
SqlService.update(table, row, values, where, wValues).then(res => {
    //res : dönen sonuç
});
```
basit bir update işlemi
```javascript
SqlService.update("chatList", ["message"], ["avare kodcu"], "chatId = ?", ["C0001"]).then(res => {
    console.log(res);
});
```
sorguları değiştirerek çeşitli update işlemi yapılabilir.

## delete


> table : tablonun ismini içerir

>  where: Sorgu şartı

>  values : Sorgu şartında bulunan '?' ifadelerinin dolduracak array

```javascript
SqlService.delete(table, where, values).then(res => {
    //res : dönen sonuç
});
```
basit bir delete işlemi
```javascript
SqlService.delete("chatList", "chatId = ?", ["C0001"]).then(res => {
    console.log(res);
});
```
ve son olarak 
## query

Özel sorgu için kullanılır.

> sql :  sorgunun tamamı

örneğiyle beraber ;
```javascript
SqlService.query("CREATE TABLE IF NOT EXISTS chatList (chatId VARCHAR(255) NOT NULL, message TEXT)").then(res => {
    console.log(res);
});
```
