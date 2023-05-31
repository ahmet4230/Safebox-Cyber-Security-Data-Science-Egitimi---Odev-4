# Safebox-Cyber-Security-Data-Science-Egitimi---Odev-4
STORED PROCEDURE
-----------------

stored procedüre veritabanı yapılarında korunan sorgulardır. Fonksiyona benzer yanları vardır
veritabanı içerisine gömülür ve ihtiyaç olunduğunda çağrılır ve istenilen işlem üzerinde 
uygulanır. Bu durum bizi tekrar tekrar sorgu yazmaktan koruyarak
zaman tasarrufu etmemizi sağlar.

---------------------------------------------------------- 

Bu durumun avantajlı yönleri;
-Store procedürler, veritabanı sunucunda çalıştığı için derleme burada olur. Bu nedenle
veritabanı istemcisiyle iletişim maliyetini azaltır. Yürütme esnasında veritabanıla arasında
daha az trafik oluşur bu da performans arttırır.
-Tekrar kullanabilirlik sağlar.
-Stored procedürler aynı zamanda güvenlikte sağlar. Veritabanı erişimini kontrol etmek için
kullanılabilir. Kullanıcların doğrudan tablolarar erişim izni vermek yerine, yalnız store
precedürlere erişim verilir ve veritabanı güveliği arttırılabilir.
-Stored procedürler aynı zamanda veri tabanı işlemlerini bir araya getirerek veri bütünlüğü
sağlanmasına yardımcı olur. Karmaşık yapılar varsa procedürle birlikte veri bütünlüğü için
kullanılabilir.

-----------------------------------------------------------

Dezavanjlı yönleri;
-Stored procedürler içinde hata bulmak zor olabilir. Bu yüzden çok fazla karmaşık yazılması
önerilmez.
-Stored procedürler, veritabanından başka bir yapıya geçirilirken sorun çıkarabilir. Her
veritabanı sistemi, bu yapıyı farklı şekilde destekleyebilir ya da hiç destekleyemez. Bu
yüzden taşınabilirlik sorunları ortaya çıkabilir.
-Sıkıştırma zorlukları yaşatabilir. Bu yapılar veritabanı sunucunda depolandığı için, sıkıştırma
ve optimize etme teknikler uygulamak zor olabilir. Bu durum veritabanı performansını sorunlarına neden olabilir.

--------------------------------------------------------------

USE [okuldb]
GO
/****** Object:  StoredProcedure [dbo].[sp_ogrenci_burs_ortalama]    Script Date: 30.05.2023 10:10:43 ******/  
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
ALTER PROC [dbo].[sp_ogrenci_burs_ortalama]
AS
BEGIN
 SELECT AVG(burs) FROM dbo.ogrenci;
END

------------------------------------------------

USE okuldb
GO
EXEC dbo.sp_ogrenci_burs_ortalama
* Burada sp_ogrenci_burs_ortalama adında bir stored procedüre oluşturulup okul databesinin, ogrenci tablosunun burs ortalamalarını alan bir yapı inşa edilmiştir.

------------------------------------------------------------------------------------------------
TRİGGER
---------------------------------------------------------------------------------------------

Triggerler veritabanı yönetim sistemlerinde herhangi bir olay gerçekleştiğinde yapılması
istenen işlemler için kullanılmaktadır. Tabloda yapılan CRUD işlemleri yapılırken(DDL TRİGGER),
başarılı giriş işlemlerinde ise "logon triggers",tetiklendiğinde belirli kod parçacıkları çalıştırılır. Avantajları ve dezavantajları vardır.
Avantajları;
-Triggerlar, veritabanında veri bütünlüğünü sağlamak için kullanılabilir. Mesela bir ekleme
silme vb. işlem yapıldığında belirli kısıtlama koyulabilir. Bu uyumsuz veri ekleme 
ya da update etme  gibi olaylar da veri bütünlüğünü sağlamaktadır
-Triggerlar ayrıca veri üzerinde denetime de sahip olmaktadırlar. Örnek vericek olursak
bir tabloya yapılan değişiklik kaydedilip loglanabilir. Bu sayede veritabanı üzerinde gerçekleşen
işlemleri takip edebilme imkanı sunmaktadır.
-Triggerlar iş kurallarını yerine getirebilir. Belirli bir işlem gerçekleştiğinde trigger
tetiklenir ve iş planına uygun hareket eder. Bu komplek süreçleri otomatize eder ve tutarlılığı
sağlar. Bu da zamandan tasarruf edilmesini mümkün kılar.
Dezavantajları;
-Triggerlar birden fazla tanımlanması triggerların birbirleriyle etkileşime
girdiği durumlarda karmaşıklık yaratabilir. Ayrıca triggerların birden fazla tanımlanması
durumunda tetiklenme sırasını takip etmek zorlaşabilir.
-Triggerlar yanlış yapılandırıldığında veya aşırı fazla tanımlandığında performans kaybına
neden olabilir. Tabloya yapılan her işlemde çalışan triggerlar işlem süresine yük bindirir 
ve süreyi arttırır.

-----------------------------------------------------------------------------------------------

USE [okuldb]
GO
/****** Object:  Trigger [dbo].[trg_Student_Insert]    Script Date: 30.05.2023 15:32:58 ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO

ALTER TRIGGER [dbo].[trg_Student_Insert]
ON [dbo].[ogrenci]
AFTER INSERT
AS
BEGIN
    INSERT INTO Mesajlar (ogrenciID, mesaj)
    SELECT ogrenciID, CONCAT('Hoş geldiniz, ', adı, ' ', soyadı, '!') FROM inserted;
END;

---------------------------------------------------------------------------

INSERT INTO ogrenci (ogrenciID, adı, soyadı, telefon, Tck_Kimli, burs, cinsiyet, okul_No)
VALUES (5, 'Ahmet', 'BATMAN', '05453', '1234567890', '5132123', '1', '12345'); 

* Bu trigger yapısında mesajlar ve ogrenci adı altında iki tablo vardır. ogrenci tablosuna yeni bir üye eklendiği zaman trigger
tetiklenecek ve mesajlar tablosuna  (Hoşgeldiniz isim soyisim şeklinde bir kayıt girecektir)

--------------------------------------------------------------------------------------

Pythondan sql servera bağlanıp stored procedür ve trigger yapılarının kullanım mantığında:
sql server bağlantı parametreleri belirttikten sonra bağlantı dize ayarları oluşturulur.
tanımlanan değişkenler ile sql servera bağlantı sağlandıktan sonra stored_precedüre adı altında
stored procedüre çağırmak için değişkene  bilgiler girilir "  stored_procedure = "{CALL sp_adi (?, ?)}"
stored procedüre parametrelerini de değişkenlere atadıktan sonra "cursor.execute(stored_procedure, (param1, param2))"
cursor.execute(stored_procedure, (param1, param2)) çağrılır.
 Yine trigger'ı çağırmak için trigger_command = f"EXEC sp_settriggerorder @triggername = '{trigger_name}', @order = 'first',
 @stmttype = 'insert', @namespace = 'database', @table_name = '{table_name}'"  gibi bir kod satırı yazıldıktan sonra 
cursor.execute(trigger_command) diyerek bağlantı atadığımız "trigger_commandı" çağırıp trigger'ı tetikleyebiliriz.
