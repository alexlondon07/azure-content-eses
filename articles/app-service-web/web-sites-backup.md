<properties 
	pageTitle="Realizar una copia de seguridad de la aplicación en Azure" 
	description="Obtenga información sobre cómo crear copias de seguridad de sus aplicaciones en el Servicio de aplicaciones de Azure." 
	services="app-service" 
	documentationCenter="" 
	authors="cephalin" 
	manager="wpickett" 
	editor="jimbe"/>

<tags 
	ms.service="app-service" 
	ms.workload="na" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="07/06/2016" 
	ms.author="cephalin"/>

# Realizar una copia de seguridad de la aplicación en Azure


La característica Copia de seguridad y restauración del [Servicio de aplicaciones de Azure](../app-service/app-service-value-prop-what-is.md) le permite crear fácilmente las copias de seguridad de la aplicación manual o automáticamente. Puede restaurar su aplicación a un estado anterior o crear una nueva aplicación basada en una de las copias de seguridad de la aplicación original.

Para obtener información sobre cómo restaurar una aplicación desde la copia de seguridad, vea [Restauración de una aplicación en el Servicio de aplicaciones de Azure](web-sites-restore.md).

<a name="whatsbackedup"></a>
## ¿Qué se incluye en la copia de seguridad? 
El Servicio de aplicaciones puede realizar una copia de seguridad de la siguiente información:

* Configuración de la aplicación
* Contenido del archivo
* Todas las Bases de datos SQL de Azure o las Base de datos MySQL en Azure (ClearDB) conectadas a su aplicación (puede elegir las que quiere incluir en la copia de seguridad)

Esta información se guarda en una copia de seguridad en la cuenta de almacenamiento y el contenedor de Azure que especifique.

> [AZURE.NOTE] Cada copia de seguridad es una copia completa sin conexión de su aplicación, no una actualización incremental.

<a name="requirements"></a>
## Requisitos y restricciones

* La característica Copia de seguridad y restauración requiere que el plan Servicio de aplicaciones tenga el nivel **estándar** o superior. Para obtener más información sobre cómo escalar el plan Servicio de aplicaciones para usar un nivel superior, vea [Escalación de una aplicación web en el Servicio de aplicaciones de Azure](web-sites-scale.md). Tenga en cuenta que el nivel **premium** permite realizar un mayor número de copias de seguridad diarias que el nivel **estándar**.
* Necesita una cuenta de almacenamiento de Azure y un contenedor en la misma suscripción que la aplicación de la que quiere realizar una copia de seguridad. Para obtener más información acerca de las cuentas de almacenamiento de Azure, consulte los [vínculos](#moreaboutstorage) al final de este artículo.
* Puede realizar una copia de seguridad de hasta 10 GB de contenido de base de datos y aplicaciones. Obtendrá un error si el tamaño de la copia de seguridad supera este límite.

<a name="manualbackup"></a>
## Crear una copia de seguridad manual

2. En el [Portal de Azure](https://portal.azure.com), vaya a la hoja de la aplicación, seleccione **Configuración** y **Copias de seguridad**. Se mostrará la hoja **Copias de seguridad**.
	
	![Página Copias de seguridad][ChooseBackupsPage]

	>[AZURE.NOTE] Si ve el mensaje siguiente, haga clic en él para actualizar su plan del Servicio de aplicaciones antes de continuar con las copias de seguridad. Vea [Escalación de una aplicación web en el Servicio de aplicaciones de Azure](web-sites-scale.md) para obtener más información. ![Selección de la cuenta de almacenamiento](./media/web-sites-backup/01UpgradePlan.png)

3. En la hoja **Copias de seguridad**, haga clic en **Almacenamiento: no configurado** para configurar una cuenta de almacenamiento.

	![Selección de la cuenta de almacenamiento][ChooseStorageAccount]
	
4. Elija el destino de copia de seguridad; para ello, seleccione una **Cuenta de almacenamiento** y un **Contenedor**. La cuenta de almacenamiento debe pertenecer a la misma suscripción que la aplicación de la que quiere realizar una copia de seguridad. Si lo desea, puede crear una nueva cuenta de almacenamiento o un nuevo contenedor en las hojas correspondientes. Cuando haya terminado, haga clic en **Seleccionar**.
	
	![Selección de la cuenta de almacenamiento](./media/web-sites-backup/02ChooseStorageAccount1.png)
	
5. En la hoja **Establecer la configuración de la copia de seguridad** que está abierta aún, haga clic en **Configuración de base de datos**, seleccione las bases de datos que desea incluir en las copias de seguridad (base de datos SQL o MySQL) y después haga clic en **Aceptar**.

	![Selección de la cuenta de almacenamiento](./media/web-sites-backup/03ConfigureDatabase.png)

	> [AZURE.NOTE] 	Para que una base de datos aparezca en esta lista, su cadena de conexión debe existir en la sección **Cadenas de conexión** de la hoja **Configuración de la aplicación** de la aplicación.

6. En la hoja **Establecer la configuración de la copia de seguridad**, haga clic en **Guardar**.

7. En la barra de comandos de la hoja **Copias de seguridad**, haga clic en **Realizar copia de seguridad ahora**.
	
	![Botón Backup Now][BackUpNow]
	
	Verá un mensaje de progreso durante el proceso de realización de la copia de seguridad.

Después de que haya configurado una cuenta de almacenamiento y un contenedor para las copias de seguridad, puede realizar una copia de seguridad manual en cualquier momento.

<a name="automatedbackups"></a>
## Configuración de copias de seguridad automatizadas

1. En la hoja **Copias de seguridad**, haga clic en **Programación: no configurada**.

	![Selección de la cuenta de almacenamiento](./media/web-sites-backup/05ScheduleBackup.png)
	
1. En la hoja **Configuración de programación de copia de seguridad**, establezca **Copia de seguridad programada** en **Activada**, configure la programación de la copia de seguridad como desee y haga clic en **Aceptar**.
	
	![Activación de las copias de seguridad automatizadas][SetAutomatedBackupOn]
	
4. En la hoja **Establecer la configuración de la copia de seguridad** que está abierta aún, haga clic en **Configuración de almacenamiento** y después elija un destino de copia de seguridad seleccionando una **Cuenta de almacenamiento** y un **Contenedor**. La cuenta de almacenamiento debe pertenecer a la misma suscripción que la aplicación de la que quiere realizar una copia de seguridad. Si lo desea, puede crear una nueva cuenta de almacenamiento o un nuevo contenedor en las hojas correspondientes. Cuando haya terminado, haga clic en **Seleccionar**.
	
	![Selección de la cuenta de almacenamiento](./media/web-sites-backup/02ChooseStorageAccount1.png)
	
5. En la hoja **Establecer la configuración de la copia de seguridad**, haga clic en **Configuración de base de datos**, seleccione las bases de datos que desea incluir en las copias de seguridad (base de datos SQL o MySQL) y después haga clic en **Aceptar**.

	![Selección de la cuenta de almacenamiento](./media/web-sites-backup/03ConfigureDatabase.png)

	> [AZURE.NOTE] 	Para que una base de datos aparezca en esta lista, su cadena de conexión debe existir en la sección **Cadenas de conexión** de la hoja **Configuración de la aplicación** de la aplicación.

6. En la hoja **Establecer la configuración de la copia de seguridad**, haga clic en **Guardar**.

<a name="partialbackups"></a>
## Realizar una copia de seguridad de una parte de la aplicación

En ocasiones, no querrá realizar una copia de seguridad de todo el contenido de la aplicación. Estos son algunos ejemplos:

-	[Configure copias de seguridad semanales](web-sites-backup.md#configure-automated-backups) de la aplicación que contiene contenido estático que nunca cambia, como entradas de blog antiguas o imágenes.
-	Su aplicación tiene más de 10 GB de contenido (que es la cantidad máxima de la que puede realizar una copia de seguridad a la vez).
-	No desea realizar copias de seguridad de los archivos de registro.

Las copias de seguridad parciales le permitirá elegir exactamente de qué archivos desea realizar la copia de seguridad.

### Exclusión de los archivos de la copia de seguridad

Para excluir archivos y carpetas de las copias de seguridad, cree un archivo `_backup.filter` en la carpeta D:\\home\\site\\wwwroot de la aplicación y especifique la lista de archivos y carpetas que quiere excluir. Es una manera fácil de obtener acceso a ella a través de la [consola Kudu](https://github.com/projectkudu/kudu/wiki/Kudu-console).

Suponga que tiene una aplicación que contiene archivos de registro e imágenes estáticas de los últimos años que nunca van a cambiar. Ya tiene una copia de seguridad completa de la aplicación que incluye las imágenes anteriores. Ahora le gustaría hacer una copia de seguridad de la aplicación todos los días, pero no quiere pagar para almacenar los archivos de registro o los archivos de imagen estática que no van a cambiar.

![Carpeta de registros][LogsFolder] ![Carpeta de imágenes][ImagesFolder]
	
Los pasos siguientes muestran cómo podría excluir estos archivos de la copia de seguridad.

1. Vaya a `http://{yourapp}.scm.azurewebsites.net/DebugConsole` e identifique las carpetas que desee excluir de las copias de seguridad. En este ejemplo, desearía excluir los siguientes archivos y carpetas que aparecen en la interfaz de usuario:

		D:\home\site\wwwroot\Logs
		D:\home\LogFiles
		D:\home\site\wwwroot\Images\2013
		D:\home\site\wwwroot\Images\2014
		D:\home\site\wwwroot\Images\brand.png

	[AZURE.NOTE] La última línea muestra que puede excluir archivos de individuales, así como carpetas.

2. Cree un archivo llamado `_backup.filter` y ponga la lista anterior en el archivo, pero quite `D:\home`. Enumere un directorio o archivo por línea. Por lo tanto, el contenido del archivo debe ser:

    \\site\\wwwroot\\Logs \\LogFiles \\site\\wwwroot\\Images\\2013 \\site\\wwwroot\\Images\\2014 \\site\\wwwroot\\Images\\brand.png

3. Cargue este archivo en el directorio `D:\home\site\wwwroot` de su sitio con [ftp](web-sites-deploy.md#ftp) o cualquier otro método. Si lo desea, puede crear el archivo directamente en `http://{yourapp}.scm.azurewebsites.net/DebugConsole` e insertar el contenido.

4. Ejecute copias de seguridad de la misma forma que lo haría normalmente, [manual](#create-a-manual-backup) o [automáticamente](#configure-automated-backups).

Ahora, los archivos y carpetas que se especifican en `_backup.filter` se excluirán de la copia de seguridad. En este ejemplo, ya no se realizará la copia de seguridad de los archivos de registro y los archivos de imagen de 2013 y 2014 , ni de brand.png.

>[AZURE.NOTE] La restauración de las copias de seguridad parciales del sitio se realizan de la misma manera que se hace para [restaurar una copia de seguridad regular](web-sites-restore.md). El proceso de restauración hará lo correcto.
>
>Cuando se restaura una copia de seguridad completa, se reemplaza todo el contenido en el sitio por lo que haya en la copia de seguridad. Si un archivo está en el sitio pero no en la copia de seguridad, se elimina. Sin embargo, cuando se restaura una copia de seguridad parcial, cualquier contenido que se encuentre en uno de los directorios en la lista negra, o cualquier archivo en la lista negra, permanecerá tal y como está.

<a name="aboutbackups"></a>

## Cómo se almacenan las copias de seguridad

Después de realizar una o varias copias de seguridad de la aplicación, las copias estarán visibles en la hoja **Contenedores** de la cuenta de almacenamiento, así como la aplicación. En la cuenta de almacenamiento, cada copia de seguridad consta de un archivo .zip que contiene los datos de copia de seguridad y el archivo .xml que contiene un manifiesto del contenido del archivo zip. Puede descomprimir y examinar estos archivos si quiere disponer de acceso a las copias de seguridad sin tener que realizar una restauración de la aplicación.

La copia de seguridad de la base de datos para la aplicación se almacena en la raíz del archivo .zip. En bases de datos de SQL, este es un archivo BACPAC (sin extensión de archivo) y se puede importar. Para crear una nueva base de datos SQL basándose en la exportación del BACPAC, vea [Importación de un archivo de BACPAC para crear una nueva base de datos de usuario](http://technet.microsoft.com/library/hh710052.aspx).

> [AZURE.WARNING] La modificación de los archivos del contenedor **websitebackups** puede ocasionar que la base de datos deje de ser válida y, por lo tanto, no se pueda restaurar.

<a name="nextsteps"></a>
## Pasos siguientes
Para obtener información sobre cómo restaurar una aplicación desde una copia de seguridad, vea [Restauración de una aplicación en el Servicio de aplicaciones de Azure](web-sites-restore.md). También puede crear una copia de seguridad y restaurar las aplicaciones del Servicio de aplicaciones mediante la API de REST (vea [Uso de REST para copia de seguridad y restauración de aplicaciones del Servicio de aplicaciones](websites-csm-backup.md)).

>[AZURE.NOTE] Si desea empezar a trabajar con el Servicio de aplicaciones de Azure antes de inscribirse para abrir una cuenta de Azure, vaya a [Prueba del Servicio de aplicaciones](http://go.microsoft.com/fwlink/?LinkId=523751), donde podrá crear inmediatamente una aplicación web de inicio de corta duración en el Servicio de aplicaciones. No es necesario proporcionar ninguna tarjeta de crédito ni asumir ningún compromiso.


<!-- IMAGES -->
[ChooseBackupsPage]: ./media/web-sites-backup/01ChooseBackupsPage.png
[ChooseStorageAccount]: ./media/web-sites-backup/02ChooseStorageAccount.png
[IncludedDatabases]: ./media/web-sites-backup/03IncludedDatabases.png
[BackUpNow]: ./media/web-sites-backup/04BackUpNow.png
[BackupProgress]: ./media/web-sites-backup/05BackupProgress.png
[SetAutomatedBackupOn]: ./media/web-sites-backup/06SetAutomatedBackupOn.png
[Frequency]: ./media/web-sites-backup/07Frequency.png
[StartDate]: ./media/web-sites-backup/08StartDate.png
[StartTime]: ./media/web-sites-backup/09StartTime.png
[SaveIcon]: ./media/web-sites-backup/10SaveIcon.png
[ImagesFolder]: ./media/web-sites-backup/11Images.png
[LogsFolder]: ./media/web-sites-backup/12Logs.png
[GhostUpgradeWarning]: ./media/web-sites-backup/13GhostUpgradeWarning.png
 

<!---HONumber=AcomDC_0713_2016-->