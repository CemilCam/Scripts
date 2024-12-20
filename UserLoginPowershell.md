# SQL Server Bağlantı Bilgileri
$SqlServer = "ServerName"           # SQL Server adı veya IP adresi
$Database = "DBName"           # Veritabanı adı
$Table = "TabloAdı"     # Tablo adı
$User = "UserName"               # SQL Kullanıcı adı
$Password = "Password"               # SQL Kullanıcı şifresi

# SQL Server Bağlantı Dizesi
$ConnectionString = "Server=$SqlServer;Database=$Database;User Id=$User;Password=$Password;"

# Kullanıcı Bilgilerini Alma
$KullaniciAdi = $env:USERNAME
$BilgisayarAdi = $env:COMPUTERNAME
$Zaman = Get-Date -Format "yyyy-MM-dd HH:mm:ss"
$OturumDurumu = "Login"

# SQL Query Oluşturma
$SqlQuery = @"
INSERT INTO $Table (KullaniciAdi, OturumDurumu, Zaman, BilgisayarAdi)
VALUES ('$KullaniciAdi', '$OturumDurumu', '$Zaman', '$BilgisayarAdi');
"@

# SQL Server'a Veri Yazma
$SqlConnection = New-Object System.Data.SqlClient.SqlConnection
$SqlConnection.ConnectionString = $ConnectionString

Try {
    $SqlConnection.Open()
    $SqlCommand = $SqlConnection.CreateCommand()
    $SqlCommand.CommandText = $SqlQuery
    $SqlCommand.ExecuteNonQuery()
    Write-Host "Veri Eklendi: $KullaniciAdi - $OturumDurumu - $Zaman"
}
Catch {
    Write-Error "SQL Hatası: $_"
}
Finally {
    $SqlConnection.Close()
}
