# SQL Server Bağlantı Bilgileri
$SqlServer = "ServerName"           # SQL Server adı veya IP adresi
$Database = "DBName"           # Veritabanı adı
$Table = "TableName"     # Tablo adı
$User = "UserName"               # SQL Kullanıcı adı
Password = "Password"               # SQL Kullanıcı şifresi

# SQL Server Bağlantı Dizesi
$ConnectionString = "Server=$SqlServer;Database=$Database;User Id=$User;Password=$Password;"

# Event Viewer'dan Event ID 4801 (Unlock) Olayını Okuma
$EventID = 4800
$Events = Get-WinEvent -FilterHashtable @{LogName='Security'; Id=$EventID} -MaxEvents 1

foreach ($Event in $Events) {
    # XML Formatında Event Detaylarını Al
    $EventXml = [xml]$Event.ToXml()
    $EventData = $EventXml.Event.EventData.Data

    # Kullanıcı Bilgilerini Çekme
    $Zaman = $Event.TimeCreated.ToString("yyyy-MM-dd HH:mm:ss")
    $OturumDurumu = "Unlock"
    $KullaniciAdi = $EventData | Where-Object { $_.Name -eq "TargetUserName" } | Select-Object -ExpandProperty '#text'
    $BilgisayarAdi = $env:COMPUTERNAME

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
}
