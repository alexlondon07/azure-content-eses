<properties
	pageTitle="Habilitar las métricas de almacenamiento en el Portal de Azure | Microsoft Azure"
	description="Cómo habilitar las métricas de almacenamiento para los servicios BLOB, Cola, Tabla y Archivo"
	services="storage"
	documentationCenter=""
	authors="robinsh"
	manager="carmonm"
	editor="tysonn"/>

<tags
	ms.service="storage"
	ms.workload="storage"
	ms.tgt_pltfrm="na"
	ms.devlang="dotnet"
	ms.topic="article"
	ms.date="07/05/2016"
	ms.author="robinsh"/>

# Habilitar las métricas de almacenamiento de Azure y ver sus datos

[AZURE.INCLUDE [storage-selector-portal-enable-and-view-metrics](../../includes/storage-selector-portal-enable-and-view-metrics.md)]

## Información general

Las Métricas de almacenamiento no están habilitadas de forma predeterminada para los servicios de almacenamiento. Puede habilitar la supervisión a través del [Portal de Azure](https://portal.azure.com) o Windows PowerShell o mediante programación a través de la biblioteca del cliente de almacenamiento.

Al habilitar las Métricas de almacenamiento, debe elegir un período de retención de datos: este período determina el tiempo que el servicio de almacenamiento mantiene las métricas y cobra por el espacio necesario para almacenarlas. Generalmente, usará un período de retención más corto para la métrica por minuto que para la métrica por hora debido al considerable espacio adicional requerido para la métrica por minuto. Debe elegir un período de retención durante el cual tenga suficiente tiempo para analizar los datos y descargar todas las métricas que quiera mantener para los informes o el análisis sin conexión. Recuerde que también se le facturará por la descarga de los datos de métricas desde su cuenta de almacenamiento.

## Cómo habilitar las métricas mediante el Portal de Azure

Siga estos pasos para habilitar las métricas en el [Portal de Azure](https://portal.azure.com):

1. Vaya a la cuenta de almacenamiento.
1. Abra la hoja **Configuración** y seleccione **Diagnósticos**.
1. Asegúrese de que la opción **Estado** está establecida en **Activada**.
1. Seleccione las métricas para los servicios que desea supervisar.
2. Especifique una directiva de retención para indicar cuánto tiempo se conservarán las métricas y los datos de registro.

Tenga en cuenta que el [Portal de Azure](https://portal.azure.com) no permite actualmente configurar métricas por minuto en su cuenta de almacenamiento; debe habilitar las métricas por minuto con PowerShell o mediante programación.

## Cómo habilitar las métricas mediante PowerShell

Puede usar PowerShell en el equipo local para configurar las Métricas de almacenamiento en su cuenta de almacenamiento con el cmdlet Get-AzureStorageServiceMetricsProperty de Azure PowerShell para recuperar la configuración actual y el cmdlet Set-AzureStorageServiceMetricsProperty para cambiar la configuración actual.

Los cmdlets que controlan las Métricas de almacenamiento usan los siguientes parámetros:

- Los valores posibles de MetricsType son Hour y Minute.

- Los valores posibles de ServiceType son Blob, Queue y Table.

- Los valores posibles de MetricsLevel son None, Service y ServiceAndApi.

Por ejemplo, el siguiente comando activa las métricas por minuto para el servicio BLOB en su cuenta de almacenamiento predeterminada con un período de retención establecido en cinco días:

`Set-AzureStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5`

El comando siguiente recupera el nivel de métricas por hora actual y los días de retención para el servicio BLOB en su cuenta de almacenamiento predeterminada:

`Get-AzureStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob`

Para obtener información sobre cómo configurar los cmdlets de Azure PowerShell para que funcionen con su suscripción de Azure y cómo seleccionar la cuenta de almacenamiento predeterminada que quiere usar, vea: [Cómo instalar y configurar Azure PowerShell](../powershell-install-configure.md).

## Cómo habilitar las Métricas de almacenamiento mediante programación

El siguiente fragmento de C# muestra cómo habilitar el registro y las métricas para el servicio BLOB mediante la biblioteca del cliente de almacenamiento de .NET:

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

    // Create service client for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Enable Storage Analytics logging and set retention policy to 10 days.
    ServiceProperties properties = new ServiceProperties();
    properties.Logging.LoggingOperations = LoggingOperations.All;
    properties.Logging.RetentionDays = 10;
    properties.Logging.Version = "1.0";

    // Configure service properties for metrics. Both metrics and logging must be set at the same time.
    properties.HourMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
    properties.HourMetrics.RetentionDays = 10;
    properties.HourMetrics.Version = "1.0";

    properties.MinuteMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
    properties.MinuteMetrics.RetentionDays = 10;
    properties.MinuteMetrics.Version = "1.0";

    // Set the default service version to be used for anonymous requests.
    properties.DefaultServiceVersion = "2015-04-05";

    // Set the service properties.
    blobClient.SetServiceProperties(properties);


## Mostrar Métricas de almacenamiento

Cuando haya configurado las métricas del Análisis de almacenamiento para supervisar su cuenta de almacenamiento, el Análisis de almacenamiento se encargará de registrar las métricas en un conjunto de tablas conocidas en su cuenta de almacenamiento. Puede configurar gráficos para ver las métricas por hora en el [Portal de Azure](https://portal.azure.com):

1. Vaya a la cuenta de almacenamiento en el [Portal de Azure](https://portal.azure.com).
2. En la sección **Supervisión**, haga clic en **Agregar iconos** para agregar un nuevo gráfico. En la **Galería de iconos**, seleccione la métrica que desea ver y arrástrela hasta la sección **Supervisión**.
3. Para modificar las métricas que se muestran en un gráfico, haga clic en el vínculo **Editar**. Puede agregar o quitar métricas individuales seleccionándolas o anulándolas.
4. Haga clic en **Guardar** cuando haya terminado de modificar las métricas.

Si quiere descargar las métricas para su almacenamiento de larga duración o para analizarlas localmente, deberá:

- Usar una herramienta que reconozca estas tablas y que le permita poder verlas y descargarlas.
- Escribir una aplicación personalizada o un script para leer y almacenar las tablas.

Muchas herramientas de exploración de almacenamiento de terceros reconocen estas tablas y permiten que las pueda ver directamente. Consulte [Exploradores de Almacenamiento de Azure](storage-explorers.md) para obtener una lista de herramientas disponibles.

> [AZURE.NOTE] A partir de la versión 0.8.0 del [Explorador de almacenamiento de Microsoft Azure](http://storageexplorer.com/), podrá ver y descargar las tablas de métricas de análisis.

Para tener acceso a las tablas de análisis mediante programación, tenga en cuenta que estas no aparecerán si enumera todas las tablas en la cuenta de almacenamiento. Puede tener acceso a ellas directamente mediante el nombre o utilizando la [API de CloudAnalyticsClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.analytics.cloudanalyticsclient.aspx) en la biblioteca de cliente .NET para consultar los nombres de tabla.

### Métricas por hora
- $MetricsHourPrimaryTransactionsBlob
- $MetricsHourPrimaryTransactionsTable
- $MetricsHourPrimaryTransactionsQueue

### Métricas por minuto
- $MetricsMinutePrimaryTransactionsBlob
- $MetricsMinutePrimaryTransactionsTable
- $MetricsMinutePrimaryTransactionsQueue

### Capacity
- $MetricsCapacityBlob

Puede encontrar detalles completos de los esquemas para estas tablas en [Esquema de las tablas de métricas del análisis de almacenamiento](https://msdn.microsoft.com/library/azure/hh343264.aspx). Las filas de ejemplo que tiene a continuación muestran solo un subconjunto de columnas disponibles, pero ilustran algunas características importantes acerca de la manera en que las Métricas de almacenamiento guardan estas métricas:

| PartitionKey | RowKey | Timestamp | TotalRequests | TotalBillableRequests | TotalIngress | TotalEgress | Disponibilidad | AverageE2ELatency | AverageServerLatency | PercentSuccess |
|---------------|:------------------:|-----------------------------:|---------------|-----------------------|--------------|-------------|--------------|-------------------|----------------------|----------------|
| 20140522T1100 | user;All | 2014-05-22T11:01:16.7650250Z | 7 | 7 | 4003 | 46801 | 100 | 104\.4286 | 6\.857143 | 100 |
| 20140522T1100 | user;QueryEntities | 2014-05-22T11:01:16.7640250Z | 5 | 5 | 2694 | 45951 | 100 | 143\.8 | 7\.8 | 100 |
| 20140522T1100 | user;QueryEntity | 2014-05-22T11:01:16.7650250Z | 1 | 1 | 538 | 633 | 100 | 3 | 3 | 100 |
| 20140522T1100 | user;UpdateEntity | 2014-05-22T11:01:16.7650250Z | 1 | 1 | 771 | 217 | 100 | 9 | 6 | 100 |

En este ejemplo de datos de métricas por minuto, la clave de partición usa el tiempo en la resolución de minutos. La clave de fila identifica el tipo de información que se almacena en la fila y se compone de dos fragmentos de información, el tipo de acceso y el tipo de solicitud:

- El tipo de acceso es user o system, donde user hace referencia a todas las solicitudes de usuario al servicio de almacenamiento y system hace referencia a las solicitudes realizadas por el Análisis de almacenamiento.

- El tipo de solicitud es all en cuyo caso es una línea de resumen, o identifica una API específica como QueryEntity o UpdateEntity.


Los datos de ejemplo anteriores muestran todos los registros para un único minuto (que empieza el a las 11:00 AM), por lo que el número de solicitudes QueryEntities más el número de solicitudes QueryEntity más el número de solicitudes UpdateEntity suman siete, que es el total que se muestra en la fila user:All. De forma similar, puede derivar la latencia promedio de un extremo a otro 104,4286 en la fila user:All con el cálculo ((143,8 * 5) + 3 + 9)/7.

Debe considerar la posibilidad de configurar alertas en la página “Supervisar” del [Portal de Azure](https://portal.azure.com), para que las Métricas de almacenamiento puedan notificarle automáticamente cualquier cambio importante en el comportamiento de sus servicios de almacenamiento. Si usa una herramienta de exploración de almacenamiento para descargar estos datos de métricas en un formato específico, puede usar Microsoft Excel para analizar los datos. Consulte la publicación sobre los[Exploradores de almacenamiento de Microsoft Azure](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) que encontrará en el blog para conseguir una lista de herramientas de exploradores de almacenamiento disponibles.



## Acceso a los datos de métricas mediante programación

El listado siguiente muestra código C# de ejemplo que accede a las métricas por minuto para un intervalo de minutos y muestra los resultados en una ventana de la consola. Usa Azure Storage Library versión 4 que incluye la clase CloudAnalyticsClient que simplifica el acceso a las tablas de métricas en almacenamiento.

    private static void PrintMinuteMetrics(CloudAnalyticsClient analyticsClient, DateTimeOffset startDateTime, DateTimeOffset endDateTime)
    {
    // Convert the dates to the format used in the PartitionKey
    var start = startDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");
    var end = endDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");

    var services = Enum.GetValues(typeof(StorageService));
    foreach (StorageService service in services)
    {
    Console.WriteLine("Minute Metrics for Service {0} from {1} to {2} UTC", service, start, end);
    var metricsQuery = analyticsClient.CreateMinuteMetricsQuery(service, StorageLocation.Primary);
    var t = analyticsClient.GetMinuteMetricsTable(service);
    var opContext = new OperationContext();
    var query =
    from entity in metricsQuery
    // Note, you can't filter using the entity properties Time, AccessType, or TransactionType
    // because they are calculated fields in the MetricsEntity class.
    // The PartitionKey identifies the DataTime of the metrics.
    where entity.PartitionKey.CompareTo(start) >= 0 && entity.PartitionKey.CompareTo(end) <= 0
    select entity;

    // Filter on "user" transactions after fetching the metrics from Table Storage.
    // (StartsWith is not supported using LINQ with Azure table storage)
    var results = query.ToList().Where(m => m.RowKey.StartsWith("user"));
    var resultString = results.Aggregate(new StringBuilder(), (builder, metrics) => builder.AppendLine(MetricsString(metrics, opContext))).ToString();
    Console.WriteLine(resultString);
    }
    }

    private static string MetricsString(MetricsEntity entity, OperationContext opContext)
    {
    var entityProperties = entity.WriteEntity(opContext);
    var entityString =
    string.Format("Time: {0}, ", entity.Time) +
    string.Format("AccessType: {0}, ", entity.AccessType) +
    string.Format("TransactionType: {0}, ", entity.TransactionType) +
    string.Join(",", entityProperties.Select(e => new KeyValuePair<string, string>(e.Key.ToString(), e.Value.PropertyAsObject.ToString())));
    return entityString;

    }




## ¿Qué se le facturará cuando habilite las métricas de almacenamiento?

Las solicitudes por escrito para crear entidades de tabla para métricas se cobran con las tarifas estándar aplicables a todas las operaciones de Almacenamiento de Azure.

La lectura y eliminación de solicitudes por un cliente para datos de métricas también se facturan con las tarifas estándar. Si ha configurado una directiva de retención de datos, no se le cobrará cuando Almacenamiento de Azure elimine datos de métricas antiguos. Sin embargo, si elimina datos de análisis, se cobrará a su cuenta las operaciones de eliminación.

La capacidad de las tablas de métricas también es facturable: puede usar las opciones siguientes para calcular la cantidad de capacidad usada para almacenar datos de métricas:

- Si cada hora un servicio utiliza todas las API en todos los servicios, se almacenan aproximadamente 148 KB de datos cada hora en las tablas de transacciones de métricas si se ha habilitado tanto el resumen de nivel servicio como de nivel de API.

- Si cada hora un servicio utiliza todas las API en todos los servicios, se almacenan aproximadamente 12 KB de datos cada hora en las tablas de transacciones de métricas si ha habilitado solo el resumen de nivel de servicio.

- Se agregan dos filas cada día a la tabla de capacidad para blobs (siempre que el usuario haya optado por los registros): esto implica que todos los días el tamaño de la tabla aumenta en unos 300 bytes, aproximadamente.

## Pasos siguientes:
[Habilitar el registro de almacenamiento y acceso a los datos del registro](https://msdn.microsoft.com/library/dn782840.aspx)

<!---HONumber=AcomDC_0713_2016-->