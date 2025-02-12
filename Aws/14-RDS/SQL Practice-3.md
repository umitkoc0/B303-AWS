# Veritabanı Oluşturma
CREATE DATABASE OgrenciSistemi;
USE OgrenciSistemi;

# Ogrenciler Tablosunu Oluşturma
CREATE TABLE Ogrenciler (
    OgrenciID INT AUTO_INCREMENT PRIMARY KEY,
    Ad VARCHAR(50) NOT NULL,
    Soyad VARCHAR(50) NOT NULL,
    Yas INT NOT NULL,
    Bolum VARCHAR(100) NOT NULL,
    Email VARCHAR(100) UNIQUE NOT NULL
);

# Dersler Tablosunu Oluşturma
CREATE TABLE Dersler (
    DersID INT AUTO_INCREMENT PRIMARY KEY,
    DersAdi VARCHAR(100) NOT NULL,
    Ogretmen VARCHAR(50) NOT NULL,
    Kredisi INT NOT NULL
);

# OgrenciDers Tablosu (Öğrenci-Ders İlişkisi)
CREATE TABLE OgrenciDers (
    OgrenciID INT,
    DersID INT,
    FOREIGN KEY (OgrenciID) REFERENCES Ogrenciler(OgrenciID) ON DELETE CASCADE,
    FOREIGN KEY (DersID) REFERENCES Dersler(DersID) ON DELETE CASCADE,
    PRIMARY KEY (OgrenciID, DersID)
);

# Ogrenciler Tablosuna Veri Ekleme
INSERT INTO Ogrenciler (Ad, Soyad, Yas, Bolum, Email) VALUES
('Ali', 'Yılmaz', 21, 'Bilgisayar Mühendisliği', 'ali.yilmaz@example.com'),
('Ayşe', 'Demir', 22, 'Elektrik Elektronik Mühendisliği', 'ayse.demir@example.com'),
('Mehmet', 'Kaya', 20, 'Makine Mühendisliği', 'mehmet.kaya@example.com'),
('Fatma', 'Öztürk', 23, 'İnşaat Mühendisliği', 'fatma.ozturk@example.com'),
('Zeynep', 'Çelik', 22, 'Bilgisayar Mühendisliği', 'zeynep.celik@example.com');

# Dersler Tablosuna Veri Ekleme
INSERT INTO Dersler (DersAdi, Ogretmen, Kredisi) VALUES
('Veritabanı Yönetimi', 'Prof. Dr. Hasan Kaya', 4),
('Algoritmalar', 'Doç. Dr. Mehmet Şahin', 5),
('Elektrik Devreleri', 'Dr. Ahmet Karahan', 3),
('Statik', 'Prof. Dr. Emine Yılmaz', 4),
('Makine Dinamiği', 'Doç. Dr. Ali Demir', 3);

# Öğrencilerin Derslere Kayıt Olması
INSERT INTO OgrenciDers (OgrenciID, DersID) VALUES
(1, 1), (1, 2), (2, 3), (3, 5), (4, 4), (5, 1), (5, 2);

# Tüm Öğrencileri Listeleme
SELECT * FROM Ogrenciler;

# Belirli Bir Bölümdeki Öğrencileri Listeleme
SELECT * FROM Ogrenciler WHERE Bolum = 'Bilgisayar Mühendisliği';

# Öğrenci Sayısını Hesaplama
SELECT COUNT(*) AS OgrenciSayisi FROM Ogrenciler;

# Her Bölümdeki Öğrenci Sayısını Listeleme
SELECT Bolum, COUNT(*) AS OgrenciSayisi FROM Ogrenciler GROUP BY Bolum;

# En Genç Öğrenciyi Listeleme
SELECT * FROM Ogrenciler ORDER BY Yas ASC LIMIT 1;

# En Yaşlı Öğrenciyi Listeleme
SELECT * FROM Ogrenciler ORDER BY Yas DESC LIMIT 1;

# Öğrenci Ad ve Soyada Göre Sıralama
SELECT * FROM Ogrenciler ORDER BY Ad ASC, Soyad ASC;

# Kredisi 4 ve Üzeri Olan Dersleri Listeleme
SELECT * FROM Dersler WHERE Kredisi >= 4;

# Öğrencilerin Aldıkları Dersleri Listeleme (JOIN Kullanımı)
SELECT 
    Ogrenciler.Ad, Ogrenciler.Soyad, Dersler.DersAdi, Dersler.Ogretmen
FROM OgrenciDers
INNER JOIN Ogrenciler ON OgrenciDers.OgrenciID = Ogrenciler.OgrenciID
INNER JOIN Dersler ON OgrenciDers.DersID = Dersler.DersID;

# Belirli Bir Öğrencinin Aldığı Dersleri Listeleme
SELECT 
    Ogrenciler.Ad, Ogrenciler.Soyad, Dersler.DersAdi
FROM OgrenciDers
INNER JOIN Ogrenciler ON OgrenciDers.OgrenciID = Ogrenciler.OgrenciID
INNER JOIN Dersler ON OgrenciDers.DersID = Dersler.DersID
WHERE Ogrenciler.Ad = 'Ali' AND Ogrenciler.Soyad = 'Yılmaz';

# Belirli Bir Derse Kayıtlı Öğrencileri Listeleme
SELECT 
    Dersler.DersAdi, Ogrenciler.Ad, Ogrenciler.Soyad
FROM OgrenciDers
INNER JOIN Ogrenciler ON OgrenciDers.OgrenciID = Ogrenciler.OgrenciID
INNER JOIN Dersler ON OgrenciDers.DersID = Dersler.DersID
WHERE Dersler.DersAdi = 'Veritabanı Yönetimi';

# Öğrenci Sayısı En Fazla Olan Dersleri Listeleme
SELECT Dersler.DersAdi, COUNT(OgrenciDers.OgrenciID) AS OgrenciSayisi
FROM OgrenciDers
INNER JOIN Dersler ON OgrenciDers.DersID = Dersler.DersID
GROUP BY Dersler.DersAdi
ORDER BY OgrenciSayisi DESC;

# Öğrenci Bilgilerini Güncelleme (E-posta Güncelleme)
UPDATE Ogrenciler SET Email = 'ali.yeni@example.com' WHERE Ad = 'Ali' AND Soyad = 'Yılmaz';

# Bir Öğrencinin Ders Kaydını Silme
DELETE FROM OgrenciDers WHERE OgrenciID = 1 AND DersID = 2;

# Belirli Bir Öğrenciyi Silme (Ders Kayıtları Otomatik Silinir)
DELETE FROM Ogrenciler WHERE Ad = 'Mehmet' AND Soyad = 'Kaya';

# OgrenciDers Tablosunu Silme
DROP TABLE IF EXISTS OgrenciDers;

# Dersler Tablosunu Silme
DROP TABLE IF EXISTS Dersler;

# Ogrenciler Tablosunu Silme
DROP TABLE IF EXISTS Ogrenciler;

# Veritabanını Silme
DROP DATABASE IF EXISTS OgrenciSistemi;
