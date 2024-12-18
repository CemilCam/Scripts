CREATE TABLE KullaniciOturumLoglari (
    ID INT IDENTITY(1,1) PRIMARY KEY,
    KullaniciAdi NVARCHAR(100) NOT NULL,
    OturumDurumu NVARCHAR(20) NOT NULL,
    Zaman DATETIME NOT NULL,
    BilgisayarAdi NVARCHAR(100) NOT NULL
);
