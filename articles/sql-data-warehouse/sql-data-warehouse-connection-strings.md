<properties
   pageTitle="Controladores de Almacenamiento de datos SQL | Microsoft Azure"
   description="Cadenas de conexión y controladores de Almacenamiento de datos SQL"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/18/2016"
   ms.author="sonyama;barbkess"/>


# Controladores de Almacenamiento de datos SQL de Azure

> [AZURE.SELECTOR]
- [Información general](sql-data-warehouse-connect-overview.md)
- [Autenticación](sql-data-warehouse-authentication.md)
- [Controladores](sql-data-warehouse-connection-strings.md)


Puede conectarse a Almacenamiento de datos SQL mediante cualquiera de los siguientes protocolos de aplicación:

- [ADO.NET][]
- [ODBC][]
- [PHP][]
- [JDBC][]

A continuación se muestran algunos ejemplos de cadenas de conexión para cada protocolo. También puede usar el Portal de Azure para ayudarle a configurar la cadena de conexión. Solo tiene que acceder a la base de datos en el Portal de Azure. En *Essentials*, haga clic en *Mostrar cadenas de conexión de base de datos*.

## Cadena de conexión ADO.NET de ejemplo

```C#
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

## Cadena de conexión ODBC de ejemplo

```C#
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

## Cadena de conexión PHP de ejemplo

```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( "sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}", "{your_user_name}", "{your_password_here}");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( "Error connecting to SQL Server." );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array("UID" => "{your_user_name}", "pwd" => "{your_password_here}", "Database" => "{your_database}", "LoginTimeout" => 30, "Encrypt" => 1, "TrustServerCertificate" => 0);\r\n$serverName = "tcp:{your_server}.database.windows.net,1433";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

## Cadena de conexión JDBC de ejemplo

```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

## Pasos siguientes

Para empezar a realizar consultas en el almacenamiento de datos con Visual Studio y otras aplicaciones, consulte [Query with Visual Studio][] \(Realización de consultas con Visual Studio).

<!--Image references-->

<!--Azure.com references-->
 [Query with Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
 
<!--MSDN references-->
[ADO.NET]: https://msdn.microsoft.com/library/e80y5yhx(v=vs.110).aspx
[ODBC]: https://msdn.microsoft.com/library/jj730314.aspx
[PHP]: https://msdn.microsoft.com/library/cc296172.aspx?f=255&MSPPError=-2147217396
[JDBC]: https://msdn.microsoft.com/library/mt484311(v=sql.110).aspx

<!--Other references-->

<!---HONumber=AcomDC_0720_2016-->