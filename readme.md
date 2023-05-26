# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

# Proje Kurulumu
Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

## Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#Tablolar 
`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]



# Görevler
Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın. 



	1) ÖRNEK SORU: Yazar tablosunu KEMAL UYUMAZ isimli yazarı ekleyin.
	
		insert into yazar (yazarad,yazarsoyad) values ("KEMAL","UYUMAZ")
	
	2) Biyografi türünü tür tablosuna ekleyiniz.
	
 		insert into tur (turadi) values ("Biyografi")
	
	3) 10A sınıfı olan ÇAĞLAR ÜZÜMCÜ isimli erkek, sınıfı 9B olan LEYLA ALAGÖZ isimli kız ve sınıfı 11C olan Ayşe Bektaş isimli kız öğrencileri tek sorguda ekleyin. 
	
		insert into ogrenci (sinif,ograd,ogrsoyad,cinsiyet) 
		values  ("10A","Çağlar","Üzümcü","E"),	
			("9B","Leyla","Alagöz","K"),
			("11C","Ayşe","Bektaş","K")
	
	4) Öğrenci tablosundaki rastgele bir öğrenciyi yazarlar tablosuna yazar olarak ekleyiniz.

		insert into yazar (yazarad,yazarsoyad)
        select ograd, ogrsoyad from ogrenci order by rand() limit 1
	
	5) Öğrenci numarası 10 ile 30 arasındaki öğrencileri yazar olarak ekleyiniz.

		insert into yazar (yazarad,yazarsoyad)
		(select ograd,ogrsoyad from ogrenci where ogrno between 10 and 30);
	
	6) Nurettin Belek isimli yazarı ekleyip yazar numarasını yazdırınız.
	(Not: Otomatik arttırmada son arttırılan değer @@IDENTITY değişkeni içinde tutulur.)
	
		insert into yazar (yazarad,yazarsoyad) values ("Nurettin","Belek")
		select @@IDENTITY
	
	
	7) 10B sınıfındaki öğrenci numarası 3 olan öğrenciyi 10C sınıfına geçirin.
	
		update ogrenci 
		set sinif = "10C" 
		where ogrno = 3
	
	
	8) 9A sınıfındaki tüm öğrencileri 10A sınıfına aktarın
	
		update ogrenci
		set sinif = '10A'
		where sinif = '9A'
	
	
	9) Tüm öğrencilerin puanını 5 puan arttırın.
	
		update ogrenci
		set puan = puan + 5
	
	
	10) 25 numaralı yazarı silin.
		
		delete yazar
		where yazarno = 25


	11) Doğum tarihi null olan öğrencileri listeleyin. (insert sorgusu ile girilen 3 öğrenci listelenecektir)
	
		select * from ogrenci
		where dtarih is null
	
	
	12) Doğum tarihi null olan öğrencileri silin. 
		
		delete from ogrenci
	    where dtarih is null
	
	13) Kitap tablosunda adı a ile başlayan kitapların puanlarını 2 artırın.
		
		update kitap
		set puan = puan + 2
		where kitapadi like 'a%'
	
	
	14) Kişisel Gelişim isimli bir tür oluşturun.
	
		insert into tur (turadi)
		values ("Kişisel Gelişim")
		
	15) Kitap tablosundaki Başarı Rehberi kitabının türünü bu tür ile değiştirin.
	
		update kitap
		set turno=(select turno from tur where turadi = "Kişisel Gelişim")
		where kitapadi = 'Başarı Rehberi'
	
	
	16) Öğrenci tablosunu kontrol etmek amaçlı tüm öğrencileri görüntüleyen "ogrencilistesi" adında bir prosedür oluşturun.
	
		create procedure  ogrencilistesi()
		begin
		select * from ogrenci;
		end
		
	
	17) Öğrenci tablosuna yeni öğrenci eklemek için "ekle" adında bir prosedür oluşturun.
		
		create procedure ekle(
		IN ad varchar(30),
		IN soyad varchar(30),
		IN cinsiyet varchar(10),
		IN sinif varchar(6),
		IN numara smallint(10)
		)
		BEGIN
		insert into ogrenci (ograd, ogrsoyad, cinsiyet, sinif, ogrno)
		values
		(ad, soyad, cinsiyet, sinif, numara);
		END
	
	18) Öğrenci noya göre öğrenci silebilmeyi sağlayan "sil" adında bir prosedür oluşturun.
	
		create procedure sil(
		IN numara smallint(10)
		)
		BEGIN
		delete from ogrenci
		where ogrno = numara;
		END
		
	19) Öğrenci numarasını kullanarak kolay bir biçimde öğrencinin sınıfını değiştirebileceğimiz bir prosedür oluşturun.
	
		create procedure ogrenciSinifDegistir(
		IN numara smallint(10),
		IN yeniSinif varchar(6),
		)
		BEGIN
		update ogrenci
		set sinif = yeniSinif
		where ogrno = numara;
		END
	
	20) Öğrenci adı ve soyadını "Ad Soyad" olarak birleştirip, ad soyada göre kolayca arama yapmayı sağlayan bir prosedür yazın.
	
		create procedure adSoyadAra(
		IN ad varchar(30),
		IN soyad varchar(30),
		)
		BEGIN
		select concat(ograd, ogrsoyad) as 'Adi Soyadi' from ogrenci
		where ograd = ad and ogrsoyad = soyad;
		END
	
	21) Daha önceden oluşturduğunu tüm prosedürleri silin.

		DROP PROCEDURE IF EXISTS ogrencilistesi;
		DROP PROCEDURE IF EXISTS adSoyadAra;
		DROP PROCEDURE IF EXISTS ekle;
		DROP PROCEDURE IF EXISTS sil;
		DROP PROCEDURE IF EXISTS ogrenciSinifDegistir;
	
	#Esnek görevler (Esnek görevlerin hepsini Select in Select ile gerçekleştirmeniz beklenmektedir.)
	22) Select in select yöntemiyle dram türündeki kitapları listeleyiniz.
		
		select kitapadi from kitap
		where turno 
		in (select turno from tur where turadi = 'Dram')

	
	23) Adı e harfi ile başlayan yazarların kitaplarını listeleyin.
	
		
		select kitapadi from kitap
		where yazarno 
		in (select yazarno from yazar where yazarad like 'e%')
	
	
	24) Kitap okumayan öğrencileri listeleyiniz.
		
		select kitapadi 
		from ogrenci
		where ogrno 
		not in (select ogrno from islem)
		
	25) Okunmayan kitapları listeleyiniz

	
	26) Mayıs ayında okunmayan kitapları listeleyiniz.
