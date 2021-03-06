<properties
	pageTitle="Habilitación de Stretch Database para una base de datos | Microsoft Azure"
	description="Obtenga información acerca de cómo configurar una base de datos para Stretch Database."
	services="sql-server-stretch-database"
	documentationCenter=""
	authors="douglaslMS"
	manager=""
	editor=""/>

<tags
	ms.service="sql-server-stretch-database"
	ms.workload="data-management"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/27/2016"
	ms.author="douglasl"/>

# Habilitación de Stretch Database para una base de datos

Para configurar una base de datos existente para Stretch Database, seleccione **Tareas | Stretch | Habilitar** en una base de datos en SQL Server Management Studio para abrir el asistente **Habilitar la base de datos para Stretch**. También puede utilizar Transact-SQL para habilitar Stretch Database para una base de datos.

Si selecciona **Tareas | Stretch | Habilitar** para una tabla y no ha habilitado todavía la base de datos para Stretch Database, el asistente configura la base de datos para Stretch Database y permite configurar tablas como parte del proceso. Siga los pasos de este tema en lugar de los pasos de [Habilitación de Stretch Database para una tabla](sql-server-stretch-database-enable-database.md).

La habilitación de Stretch Database en una base de datos o una tabla requiere permisos db\_owner. La habilitación de Stretch Database en una base de datos también requiere permisos CONTROL DATABASE.

## Antes de comenzar

-   Antes de configurar una base de datos para Stretch, se recomienda ejecutar el Asesor de Stretch Database para identificar las bases de datos y las tablas que son elegibles para el ajuste. El Asesor de Stretch Database también identifica los problemas de bloqueo. Para obtener más información, vea [Identificación de bases de datos y tablas para Stretch Database](sql-server-stretch-database-identify-databases.md).

-   Revise [Limitaciones de Stretch Database](sql-server-stretch-database-limitations.md).

-   Stretch Database migra los datos a Azure. Por lo tanto, es necesario tener una cuenta de Azure y una suscripción para la facturación. Para obtener una cuenta de Azure, [haga clic aquí](http://azure.microsoft.com/pricing/free-trial/).

-   Tenga a mano la información de inicio de sesión y conexión que necesita para crear un nuevo servidor de Azure o seleccionar uno que ya exista.

## <a name="EnableTSQLServer"></a>Requisito previo: habilitar Stretch Database en el servidor
Para poder habilitar Stretch Database en una base de datos o una tabla, tendrá que habilitarlo en el servidor local. Esta operación requiere permisos sysadmin o serveradmin.

-   Si tiene los permisos administrativos necesarios, el asistente para **habilitar la base de datos para Stretch** configura el servidor para Stretch.

-   Si no tiene los permisos necesarios, un administrador debe habilitar manualmente la opción ejecutando **sp\_configure** para ejecutar el asistente, o un administrador debe ejecutar el asistente.

Para habilitar Stretch Database en el servidor manualmente, ejecute **sp\_configure** y active la opción de **archivo de datos remotos**. En el ejemplo siguiente se habilita la opción de **archivo de datos remotos** estableciendo su valor en 1.

```
EXEC sp_configure 'remote data archive' , '1';
GO

RECONFIGURE;
GO
```
Para obtener más información, vea [Establecimiento de la opción de configuración del servidor del archivo de datos remotos](https://msdn.microsoft.com/library/mt143175.aspx) y [sp\_configure (Transact-SQL)](https://msdn.microsoft.com/library/ms188787.aspx).

## <a name="Wizard"></a>Uso del asistente para habilitar Stretch Database en una base de datos
Para más información sobre el asistente Habilitar la base de datos para Stretch, incluida la información que se debe especificar y las opciones que se deben seleccionar, consulte [Ejecución del asistente para habilitar la base de datos para Stretch](sql-server-stretch-database-wizard.md).

## <a name="EnableTSQLDatabase"></a>Uso de Transact-SQL para habilitar Stretch Database en una base de datos
Para poder habilitar Stretch Database en tablas individuales, tendrá que habilitarlo en la base de datos.

La habilitación de Stretch Database en una base de datos o una tabla requiere permisos db\_owner. La habilitación de Stretch Database en una base de datos también requiere permisos CONTROL DATABASE.

1.  Antes de comenzar, elija un servidor de Azure existente para los datos que migra Stretch Database, o bien cree un nuevo servidor.

2.  En el servidor de Azure, cree una regla de firewall con el intervalo de direcciones IP de SQL Server que permita a esta herramienta comunicarse con el servidor remoto.

    Puede intentar conectarse al servidor de Azure desde el Explorador de objetos de SQL Server Management Studio (SSMS) para encontrar fácilmente los valores que necesita y crear la regla de firewall. Para ayudarle a crear la regla, SSMS abre el siguiente cuadro de diálogo que ya incluye los valores de dirección IP necesarios.

	![Creación de una regla de firewall en SSMS][FirewallRule]

3.  Para configurar una base de datos de SQL Server para Stretch Database, la base de datos debe tener una clave maestra de base de datos. La clave maestra de base de datos protege las credenciales que Stretch Database usa para conectarse a la base de datos remota. A continuación, se muestra un ejemplo que crea una nueva clave maestra de base de datos.

    ```tsql
    USE <database>;
    GO

    CREATE MASTER KEY ENCRYPTION BY PASSWORD ='<password>';
	GO
    ```

    Para más información sobre la clave maestra de base de datos, consulte [CREATE MASTER KEY (Transact-SQL)](https://msdn.microsoft.com/library/ms174382.aspx) y [Crear la clave maestra de una base de datos](https://msdn.microsoft.com/library/aa337551.aspx).

4.  Cuando configure una base de datos para Stretch Database, tendrá que proporcionar una credencial para Stretch Database use para la comunicación entre el servidor de Azure remoto y SQL Server local. Tiene dos opciones:

    -   Puede proporcionar una credencial de administrador.

        -   Si habilita Stretch Database ejecutando el asistente, puede crear la credencial en ese momento.

        -   Si planea habilitar Stretch Database ejecutando **ALTER DATABASE**, tendrá que crear manualmente la credencial antes de ejecutar **ALTER DATABASE** para habilitar Stretch Database.

		A continuación, se muestra un ejemplo que crea una nueva credencial.

        ```tsql
        CREATE DATABASE SCOPED CREDENTIAL <db_scoped_credential_name>
            WITH IDENTITY = '<identity>' , SECRET = '<secret>';
        GO
        ```

		Para más información sobre la credencial que crea el asistente, consulte [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](https://msdn.microsoft.com/library/mt270260.aspx). La creación de la credencial requiere permisos ALTER ANY CREDENTIAL.

    -   Puede usar una cuenta de servicio federado para que SQL Server se comunique con el servidor remoto de Azure cuando se cumplan todas las condiciones siguientes.

        -   La cuenta de servicio en la que se ejecuta la instancia de SQL Server es una cuenta de dominio.

        -   La cuenta de dominio pertenece a un dominio cuyo Active Directory está federado con Azure Active Directory.

        -   El servidor remoto de Azure está configurado para admitir la autenticación de Azure Active Directory.

        -   La cuenta de servicio en la que se ejecuta la instancia de SQL Server debe configurarse como una cuenta dbmanager o sysadmin en el servidor remoto de Azure.

5.  Para configurar una base de datos para Stretch Database, ejecute el comando ALTER DATABASE.

    1.  Para el argumento SERVER, proporcione el nombre de un servidor existente de Azure, incluida la parte `.database.windows.net` del nombre; por ejemplo, `MyStretchDatabaseServer.database.windows.net`.

    2.  Proporcione una credencial de administrador existente con el argumento CREDENTIAL o especifique FEDERATED\_SERVICE\_ACCOUNT = ON. En el ejemplo siguiente se proporciona una credencial existente.

    ```tsql
    ALTER DATABASE <database name>
        SET REMOTE_DATA_ARCHIVE = ON
            (
                SERVER = '<server_name>',
                CREDENTIAL = <db_scoped_credential_name>
            ) ;
    GO
    ```

## Pasos siguientes
-   [Habilitación de Stretch Database para una tabla](sql-server-stretch-database-enable-table.md) para habilitar tablas adicionales.

-   [Supervisión de Stretch Database](sql-server-stretch-database-monitor.md) para ver el estado de la migración de datos.

-   [Pausa y reanudación Stretch Database](sql-server-stretch-database-pause.md)

-   [Administración y solución de problemas de Stretch Database](sql-server-stretch-database-manage.md)

-   [Copia de seguridad y restauración de bases de datos habilitadas para Stretch](sql-server-stretch-database-backup.md)

## Consulte también

[Identify databases and tables for Stretch Database by running Stretch Database Advisor (Identificación de bases de datos y tablas para Stretch Database mediante la ejecución de Stretch Database Advisor)](sql-server-stretch-database-identify-databases.md)

[Opciones de ALTER DATABASE SET (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx)

[FirewallRule]: ./media/sql-server-stretch-database-enable-database/firewall.png

<!---HONumber=AcomDC_0706_2016-->