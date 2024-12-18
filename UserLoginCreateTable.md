CREATE TABLE KullaniciOturumLoglari (
    ID INT IDENTITY(1,1) PRIMARY KEY,
    KullaniciAdi NVARCHAR(100) NOT NULL,
    OturumDurumu NVARCHAR(20) NOT NULL,
    Zaman DATETIME NOT NULL,
    BilgisayarAdi NVARCHAR(100) NOT NULL
);


# User Policy Ekran Görüntüsü

![UserPolicy](https://github.com/user-attachments/assets/c64d99eb-e52a-4034-a0a7-7e5e164a4f74)
