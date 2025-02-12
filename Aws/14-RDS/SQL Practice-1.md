# Veritabanı Oluşturma
CREATE DATABASE SirketVeritabani;
USE SirketVeritabani;

# Çalışanlar Tablosunu Oluşturma
CREATE TABLE Calisanlar (
    CalisanID INT AUTO_INCREMENT PRIMARY KEY,
    Ad VARCHAR(50) NOT NULL,
    Soyad VARCHAR(50) NOT NULL,
    Cinsiyet ENUM('Erkek', 'Kadın') NOT NULL,
    Yas INT NOT NULL,
    Departman VARCHAR(50) NOT NULL,
    Pozisyon VARCHAR(50) NOT NULL,
    Maas DECIMAL(10,2) NOT NULL,
    IseGirisTarihi DATE NOT NULL,
    Telefon VARCHAR(15) NOT NULL,
    Email VARCHAR(100) NOT NULL UNIQUE
);

# Çalışanlar Tablosuna Veri Ekleme
INSERT INTO Calisanlar (Ad, Soyad, Cinsiyet, Yas, Departman, Pozisyon, Maas, IseGirisTarihi, Telefon, Email) VALUES
('Ahmet', 'Yılmaz', 'Erkek', 35, 'IT', 'Yazılım Geliştirici', 15000.00, '2020-05-15', '05554443322', 'ahmet.yilmaz@example.com'),
('Mehmet', 'Demir', 'Erkek', 40, 'Muhasebe', 'Muhasebe Müdürü', 17000.00, '2018-09-20', '05556667788', 'mehmet.demir@example.com'),
('Ayşe', 'Kara', 'Kadın', 30, 'İK', 'İK Uzmanı', 14000.00, '2021-03-10', '05559998877', 'ayse.kara@example.com'),
('Fatma', 'Çelik', 'Kadın', 28, 'Satış', 'Satış Temsilcisi', 13000.00, '2022-01-05', '05551112233', 'fatma.celik@example.com'),
('Burak', 'Öztürk', 'Erkek', 45, 'IT', 'Sistem Yöneticisi', 20000.00, '2015-07-25', '05553334455', 'burak.ozturk@example.com'),
('Elif', 'Şahin', 'Kadın', 32, 'Pazarlama', 'Pazarlama Uzmanı', 13500.00, '2019-06-14', '05552221144', 'elif.sahin@example.com'),
('Cem', 'Arslan', 'Erkek', 38, 'Lojistik', 'Lojistik Müdürü', 16000.00, '2016-08-30', '05557776655', 'cem.arslan@example.com'),
('Zeynep', 'Koç', 'Kadın', 29, 'IT', 'Front-End Geliştirici', 14500.00, '2023-02-01', '05554445566', 'zeynep.koc@example.com'),
('Hakan', 'Eren', 'Erkek', 34, 'Satış', 'Satış Müdürü', 15500.00, '2020-10-12', '05556668899', 'hakan.eren@example.com'),
('Merve', 'Tan', 'Kadın', 27, 'İK', 'İşe Alım Uzmanı', 12500.00, '2021-11-03', '05551113322', 'merve.tan@example.com'),
('Emre', 'Aydın', 'Erkek', 31, 'IT', 'Back-End Geliştirici', 15500.00, '2019-04-23', '05551234567', 'emre.aydin@example.com'),
('Büşra', 'Yıldırım', 'Kadın', 26, 'Pazarlama', 'Sosyal Medya Uzmanı', 12000.00, '2022-06-15', '05559876543', 'busra.yildirim@example.com'),
('Ali', 'Tekin', 'Erkek', 50, 'Muhasebe', 'Finans Direktörü', 22000.00, '2012-01-01', '05557778899', 'ali.tekin@example.com'),
('Esra', 'Güneş', 'Kadın', 36, 'İK', 'İK Müdürü', 17500.00, '2017-05-11', '05556667744', 'esra.gunes@example.com');

# Çalışan Bilgilerini Listeleme
SELECT * FROM Calisanlar;

# Belirli Bir Departmandaki Çalışanları Listeleme
SELECT * FROM Calisanlar WHERE Departman = 'IT';

# Maaşı 15000’den Fazla Olan Çalışanları Listeleme
SELECT * FROM Calisanlar WHERE Maas > 15000;

# Belirli Bir Yaş Aralığındaki Çalışanları Listeleme
SELECT * FROM Calisanlar WHERE Yas BETWEEN 30 AND 40;

# Belirli Bir Pozisyondaki Çalışanları Listeleme
SELECT * FROM Calisanlar WHERE Pozisyon = 'Satış Müdürü';

# Çalışan Maaşlarını En Yüksekten En Düşüğe Sıralama
SELECT * FROM Calisanlar ORDER BY Maas DESC;

# En Son İşe Giren Çalışanı Listeleme
SELECT * FROM Calisanlar ORDER BY IseGirisTarihi DESC LIMIT 1;

# IT Departmanındaki Çalışanların Ortalama Maaşını Hesaplama
SELECT AVG(Maas) AS OrtalamaMaas FROM Calisanlar WHERE Departman = 'IT';

# Çalışan Sayısını Hesaplama
SELECT COUNT(*) AS CalisanSayisi FROM Calisanlar;

# Her Departmandaki Çalışan Sayısını Listeleme
SELECT Departman, COUNT(*) AS CalisanSayisi FROM Calisanlar GROUP BY Departman;

# Her Departmandaki Ortalama Maaşı Hesaplama
SELECT Departman, AVG(Maas) AS OrtalamaMaas FROM Calisanlar GROUP BY Departman;

# Belirli Bir Çalışanın Bilgilerini Güncelleme
UPDATE Calisanlar SET Maas = 18000 WHERE Ad = 'Mehmet' AND Soyad = 'Demir';

# Çalışanları Ad ve Soyada Göre Sıralama
SELECT * FROM Calisanlar ORDER BY Ad ASC, Soyad ASC;

# Belirli Bir Cinsiyetteki Çalışanları Listeleme
SELECT * FROM Calisanlar WHERE Cinsiyet = 'Kadın';

# En Düşük Maaşı Alan Çalışanı Listeleme
SELECT * FROM Calisanlar ORDER BY Maas ASC LIMIT 1;

# Çalışanların Ortalama Yaşını Hesaplama
SELECT AVG(Yas) AS OrtalamaYas FROM Calisanlar;

# 2020 Yılından Sonra İşe Girenleri Listeleme
SELECT * FROM Calisanlar WHERE YEAR(IseGirisTarihi) > 2020;

# Belirli Bir Pozisyondaki Çalışanların Sayısını Hesaplama
SELECT Pozisyon, COUNT(*) AS PozisyonSayisi FROM Calisanlar GROUP BY Pozisyon;

# Departmanlara Göre En Yüksek Maaşları Listeleme
SELECT Departman, MAX(Maas) AS EnYuksekMaas FROM Calisanlar GROUP BY Departman;

# Her Departmandaki Çalışanları Sayıya Göre Azalan Şekilde Listeleme
SELECT Departman, COUNT(*) AS CalisanSayisi FROM Calisanlar GROUP BY Departman ORDER BY CalisanSayisi DESC;

# Çalışan Silme İşlemi
DELETE FROM Calisanlar WHERE Ad = 'Fatma' AND Soyad = 'Çelik';

# Çalışanlar Tablosunu Silme
DROP TABLE IF EXISTS Calisanlar;

# Veritabanını Silme
DROP DATABASE IF EXISTS SirketVeritabani;
