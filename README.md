# MSSQL Sorgu Çalışmaları ve Hastane Veritabanı Tasarımı

İki bölümden oluşuyor: Microsoft'un Northwind örnek veritabanı üzerinde
çözülmüş SQL sorguları ve sıfırdan tasarlanmış, normalizasyon kurallarına uygun
bir hastane kayıt veritabanı şeması.

Bu sorguların LINQ karşılıkları northwind-linq-api reposunda.

İTÜ Bilgi İşlem Daire Başkanlığı — Yazılım Geliştirme Grubu (YGG) Yazılım
Geliştirme Proje Sınıfı'nın dördüncü aşaması kapsamında hazırlandı.

## northwind-sorgu-cozumleri.sql
Her sorunun metni yorum satırı olarak üstünde, altında çözümü yer alıyor.


| Konu | Konu |
|---|---|
| SELECT sorguları | Aggregate fonksiyonlar (COUNT, SUM, AVG, MIN, MAX) |
| WHERE ile filtreleme | IN yapısı |
| AND / OR operatörleri | CASE WHEN |
| NULL kontrolü | GROUP BY |
| ORDER BY ile sıralama | JOIN türleri |
| TOP ile kayıt sınırlama | HAVING |
| BETWEEN AND | Alt sorgular (subquery) |
| LIKE ile arama | INSERT / UPDATE / DELETE |

Tarih fonksiyonları (`DATEDIFF`, `DATENAME`, `GETDATE`) ve metin işlemleri
(`SUBSTRING`, birleştirme) de sorgular içinde kullanılıyor.

Çalıştırmak için Northwind veritabanının kurulu olması gerekiyor. Kurulum
scriptleri Microsoft'a ait olduğu için bu repoya dahil edilmedi;
[Microsoft'un deposundan](https://github.com/microsoft/sql-server-samples)
edinilebilir.

## hastane-veritabani-semasi.sql

Hasta kayıt aşamasından reçete edilen ilaçlara kadar geçen süreci kapsayan
yedi tablolu veri modeli.

| Tablo | İçerik |
|---|---|
| `Hastalar` | TC kimlik no (UNIQUE), ad, soyad, doğum tarihi, kan grubu |
| `Klinikler` | Klinik tanımları |
| `Doktorlar` | Kliniğe bağlı doktor kayıtları |
| `Muayeneler` | Hasta, doktor, tarih ve şikâyet bilgisi |
| `Teshisler` | Muayeneye bağlı tanı ve uygulanan işlem |
| `Ilaclar` | İlaç adı ve etken madde |
| `ReceteDetay` | Muayene ile ilaç arasındaki ilişki ve dozaj |

Tüm ilişkiler adlandırılmış `FOREIGN KEY` kısıtlarıyla kuruldu.

Tablolar 3NF'e uygun ayrıştırıldı: ilaç bilgisi reçeteden, klinik bilgisi
doktordan, tanı bilgisi muayeneden ayrı tutularak tekrar eden veri ortadan
kaldırıldı. Bir muayeneye birden fazla ilaç yazılabildiği için `ReceteDetay`
ara tablo olarak konumlandırıldı.

## Çalıştırma

SSMS veya Azure Data Studio üzerinde `hastane-veritabani-semasi.sql` dosyasını
açıp çalıştırmak veritabanını ve tabloları oluşturur.
