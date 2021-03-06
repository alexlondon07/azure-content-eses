<properties 
	pageTitle="Creación y programación de canalizaciones, actividades en cadena en Data Factory| Microsoft Azure" 
	description="Aprenda a crear una canalización de datos en Data Factory de Azure para mover y transformar datos. Crear un flujo de trabajo controlado por datos para producir información lista para usar." 
    keywords="canalización de datos, flujo de trabajo controlado por datos"
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article"
	ms.date="06/27/2016" 
	ms.author="spelluru"/>

# Canalizaciones y actividades en Data Factory de Azure: crear y programar canalizaciones y actividades en cadena
Este artículo ayuda a comprender las canalizaciones de datos y actividades en Data Factory de Azure y la forma de usarlas para construir flujos de trabajo de extremo a extremo controlados por datos para su escenario o empresa, desde recomendaciones de producto personalizadas hasta el análisis de una campaña de marketing.

> [AZURE.NOTE] En este artículo se considera que ha repasado antes los artículos [Introducción al servicio Data Factory de Azure](data-factory-introduction.md) y[Conjuntos de datos en Data Factory de Azure](data-factory-create-datasets.md). Si no tiene experiencia práctica con la creación de factorías de datos, repasar el tutorial [Creación de su primera factoría de datos](data-factory-build-your-first-pipeline.md) puede ayudarle a comprender mejor este artículo.

## ¿Qué es una canalización de datos?
**Una canalización es una agrupación lógica de actividades**. Se usan para agrupar actividades en una unidad que realiza una tarea. Para entender mejor las canalizaciones, deberá entender primero qué es una actividad.

## ¿Qué es una actividad?
Las actividades definen las acciones que se van a realizar en los datos. Cada actividad tiene cero o más [conjuntos de datos](data-factory-create-datasets.md) como entradas y genera uno o varios conjuntos de datos como salida. **Una actividad es una unidad de orquestación en Factoría de datos de Azure.**

Por ejemplo, puede utilizar una actividad de copia para organizar la copia de datos de un conjunto de datos a otro. De manera similar, puede usar una actividad de Hive de HDInsight que ejecute una consulta de Hive en un clúster de HDInsight de Azure para transformar o analizar los datos. Factoría de datos de Azure ofrece una amplia variedad de actividades de [movimiento de datos, análisis](data-factory-data-transformation-activities.md) y [transformación de datos](data-factory-data-movement-activities.md). También puede crear una actividad personalizada de .NET para ejecutar su propio código.

Tenga en cuenta los dos conjuntos de datos siguientes:

**Base de datos SQL de Azure**

La tabla "MyTable" contiene una columna "timestampcolumn" que permite especificar la fecha y hora en la que se insertaron los datos en la base de datos.

	{
	  "name": "AzureSqlInput",
	  "properties": {
	    "type": "AzureSqlTable",
	    "linkedServiceName": "AzureSqlLinkedService",
	    "typeProperties": {
	      "tableName": "MyTable"
	    },
	    "external": true,
	    "availability": {
	      "frequency": "Hour",
	      "interval": 1
	    },
	    "policy": {
	      "externalData": {
	        "retryInterval": "00:01:00",
	        "retryTimeout": "00:10:00",
	        "maximumRetry": 3
	      }
	    }
	  }
	}

**Conjunto de datos del blob de Azure**

Los datos se copian a un blob nuevo cada hora con la ruta de acceso para el blob que refleja la fecha y hora específicas con granularidad de hora.

	{
	  "name": "AzureBlobOutput",
	  "properties": {
	    "type": "AzureBlob",
	    "linkedServiceName": "StorageLinkedService",
	    "typeProperties": {
	      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
	      "partitionedBy": [
	        {
	          "name": "Year",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "yyyy"
	          }
	        },
	        {
	          "name": "Month",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%M"
	          }
	        },
	        {
	          "name": "Day",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%d"
	          }
	        },
	        {
	          "name": "Hour",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%H"
	          }
	        }
	      ],
	      "format": {
	        "type": "TextFormat",
	        "columnDelimiter": "\t",
	        "rowDelimiter": "\n"
	      }
	    },
	    "availability": {
	      "frequency": "Hour",
	      "interval": 1
	    }
	  }
	}


La siguiente actividad de copia en la canalización copia los datos de SQL Azure en el almacenamiento de blobs de Azure. Toma la tabla de SQL Azure como el conjunto de datos de entrada con una frecuencia de cada hora y escribe los datos en el almacenamiento de blobs de Azure representado por el conjunto de datos "AzureBlobOutput". El conjunto de datos de salida también tiene una frecuencia de cada hora. Consulte la sección [Programación y ejecución](#scheduling-and-execution) para comprender cómo se copian los datos durante la unidad de tiempo. Esta canalización tiene un período activo de 3 horas desde "2015-01-01T08:00:00" hasta "2015-01-01T11:00:00".

**Canalización:**
	
	{  
	    "name":"SamplePipeline",
	    "properties":{  
	    "start":"2015-01-01T08:00:00",
	    "end":"2015-01-01T11:00:00",
	    "description":"pipeline for copy activity",
	    "activities":[  
	      {
	        "name": "AzureSQLtoBlob",
	        "description": "copy activity",
	        "type": "Copy",
	        "inputs": [
	          {
	            "name": "AzureSQLInput"
	          }
	        ],
	        "outputs": [
	          {
	            "name": "AzureBlobOutput"
	          }
	        ],
	        "typeProperties": {
	          "source": {
	            "type": "SqlSource",
	            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
	          },
	          "sink": {
	            "type": "BlobSink"
	          }
	        },
	       "scheduler": {
	          "frequency": "Hour",
	          "interval": 1
	        },
	        "policy": {
	          "concurrency": 1,
	          "executionPriorityOrder": "OldestFirst",
	          "retry": 0,
	          "timeout": "01:00:00"
	        }
	      }
	     ]
	   }
	}

Ahora que tenemos una breve idea de lo que es una actividad, vamos volver a la canalización.
 
Una canalización es una agrupación lógica de actividades. Se usan para agrupar actividades en una unidad que realiza una tarea. **Una canalización también es la unidad de implementación y administración de las actividades.** Por ejemplo, puede que desee reunir las actividades relacionadas lógicamente como una canalización de forma que pueden estar en estado activo o en pausa juntos.

Un conjunto de datos de salida de una actividad en una canalización puede ser el conjunto de datos de entrada a otra actividad en la misma canalización o en otra diferente mediante la definición de las dependencias entre las actividades. Consulte [Programación y ejecución](#chaining-activities) para más información.

Los pasos típicos al crear una canalización en Factoría de datos de Azure son:

1.	Cree una factoría de datos (si no se ha creado).
2.	Cree un servicio vinculado para cada almacén de datos o proceso.
3.	Cree conjuntos de datos de entrada y salida.
4.	Cree una canalización con actividades que opere en los conjuntos de datos definidos anteriormente.

![Entidades de Factoría de datos](./media/data-factory-create-pipelines/entities.png)

Vamos a fijarnos un poco más en cómo se define una canalización.

## Anatomía de una canalización  

La estructura genérica de una canalización tiene el aspecto siguiente:

	{
	    "name": "PipelineName",
	    "properties": 
	    {
	        "description" : "pipeline description",
	        "activities":
	        [
	
	        ],
			"start": "<start date-time>",
			"end": "<end date-time>"
	    }
	}

La sección **activities** puede contener una o más actividades definidas. Cada actividad tiene la siguiente estructura de nivel superior:

	{
	    "name": "ActivityName",
	    "description": "description", 
	    "type": "<ActivityType>",
	    "inputs":  "[]",
	    "outputs":  "[]",
	    "linkedServiceName": "MyLinkedService",
	    "typeProperties":
	    {
	
	    },
	    "policy":
	    {
	    }
	    "scheduler":
	    {
	    }
	}

En la tabla siguiente se describen las propiedades dentro de las definiciones de actividad y la canalización JSON:

Etiqueta | Descripción | Obligatorio
--- | ----------- | --------
name | Nombre de la actividad o la canalización. Especifique un nombre que represente la acción que la actividad o la canalización está configurada para realizar<br/><ul><li>Número máximo de caracteres: 260</li><li>Tiene que comenzar con una letra, un número o un carácter de subrayado (\_)</li><li>No se permiten caracteres siguientes: “.”, “+”, “?”, “/”, “<”,”>”,”*”,”%”,”&”,”:”,”\\”</li></ul> | Sí
description | Texto que describe para qué se usa la actividad o la canalización | Sí
type | Especifica el tipo de actividad. Consulte los artículos [Actividades de movimiento de datos](data-factory-data-movement-activities.md) y [Actividades de transformación de datos](data-factory-data-transformation-activities.md) para ver los diferentes tipos de actividades. | Sí
inputs | Tablas de entrada utilizadas por la actividad <br/><br/>/ / una tabla de entrada<br/>"inputs": [{"name": "inputtable1"}],<br/><br/>/ / dos tablas de entrada <br/>"inputs": [{"name": "inputtable1"}, {"name": "inputtable2"}], | Sí
outputs | Tablas de salida utilizadas por la actividad.// una tabla de salida<br/>"outputs": [ { "name": “outputtable1” } ],<br/><br/>//dos tablas de salida<br/>"outputs": [ { "name": “outputtable1” }, { "name": “outputtable2” } ], | Sí
linkedServiceName | Nombre del servicio vinculado utilizado por la actividad. <br/><br/>Una actividad puede requerir que especifique el servicio vinculado que enlaza con el entorno de procesos necesario. | Sí para actividad de HDInsight y actividad de puntuación por lotes de Aprendizaje automático de Azure <br/><br/>No para todos los demás
typeProperties | Propiedades en la sección typeProperties dependen del tipo de actividad. Consulte el artículo sobre cada actividad específica para más información | No
policy | Directivas que afectan al comportamiento en tiempo de ejecución de la actividad. Si no se especifica, se usan las directivas predeterminadas. Desplácese a continuación para una información más detallada | No
start | Fecha y hora de inicio para la canalización. Debe estar en [formato ISO](http://en.wikipedia.org/wiki/ISO_8601). Por ejemplo: 2014-10-14T16:32:41Z. <br/><br/>Se puede especificar una hora local, por ejemplo, una hora EST. Aquí tiene un ejemplo: "2016-02-27T06:00:00**-05:00**", que son las 6 a.m. EST.<br/><br/>Las propiedades start y end juntas especifican un período activo para la canalización. Los segmentos de salida solo se producen en este período activo. | No<br/><br/>Si especifica un valor para la propiedad end, tiene que especificar el valor de la propiedad start.<br/><br/>Las horas de inicio y finalización pueden estar vacías para crear una canalización, pero ambas tienen que tener valores para establecer un período activo para que se ejecute la canalización. Si no especifica horas de inicio y finalización al crear una canalización, puede establecerlas más adelante mediante el cmdlet Set-AzureDataFactoryPipelineActivePeriod.
end | Fecha y hora de finalización para la canalización. Si se especifica, debe estar en formato ISO. Por ejemplo: 2014-10-14T17:32:41Z <br/><br/>Es posible especificar una hora local, por ejemplo, una hora EST. Aquí tiene un ejemplo: "2016-02-27T06:00:00**-05:00**", que son las 6 a.m. EST.<br/><br/>Para ejecutar la canalización indefinidamente especifique 9999-09-09 como valor para la propiedad end. | No <br/><br/>Si se especifica un valor para la propiedad start, hay que especificar un valor para la propiedad end.<br/><br/>Consulte las notas para la propiedad **start**.
isPaused | Si se establece en true, la canalización no se ejecutará. Valor predeterminado = false. Puede utilizar esta propiedad para habilitar o deshabilitar. | No 
scheduler | La propiedad "scheduler" se utiliza para definir la programación deseada para la actividad. Sus subpropiedades son las mismas que las de la [propiedad availability de un conjunto de datos](data-factory-create-datasets.md#Availability). | No |   
| pipelineMode | El método para la programación de ejecuciones para la canalización. Los valores permitidos son: scheduled (predeterminado).<br/><br/>‘Scheduled’ indica que la canalización se ejecutará en el intervalo de tiempo especificado de acuerdo con su período activo (hora de inicio y finalización). 'Onetime' indica que la canalización se ejecutará solo una vez. Las canalizaciones creadas una sola vez no se pueden modificar ni actualizar actualmente. Consulte la sección [Canalización de una vez](data-factory-scheduling-and-execution.md#onetime-pipeline) para más información acerca de la configuración onetime. | No | 
| expirationTime | Tiempo de duración después de la creación durante el que la canalización es válida y debe permanecer aprovisionada. La canalización se elimina automáticamente una vez que alcanza la hora de caducidad si no tiene ninguna ejecución activa, con error o pendiente. | No | 
| datasets | Lista de conjuntos de datos que usarán las actividades definidas en la canalización. Puede utilizarse para definir conjuntos de datos que son específicos de esta canalización y que no se definen dentro de la factoría de datos. Los conjuntos de datos definidos dentro de esta canalización solo pueden utilizarse en ella y no se pueden compartir. Consulte la sección [Conjuntos de datos limitados](data-factory-create-datasets.md#scoped-datasets) para más información.| No |  
 

## Tipos de actividades de movimiento de datos y transformación de datos
Factoría de datos de Azure ofrece una amplia gama de actividades de [movimiento de datos](data-factory-data-movement-activities.md) y de [transformación de datos](data-factory-data-transformation-activities.md).

### Directivas
Las directivas afectan al comportamiento en tiempo de ejecución de una actividad, concretamente cuando se procesa el segmento de una tabla. En la tabla siguiente se proporciona los detalles.

Propiedad | Valores permitidos | Valor predeterminado | Descripción
-------- | ----------- | -------------- | ---------------
simultaneidad | Entero <br/><br/>Valor máximo: 10 | 1 | Número de ejecuciones simultáneas de la actividad.<br/><br/>Determina el número de ejecuciones paralelas de la actividad que pueden tener lugar en distintos segmentos. Por ejemplo, si una actividad tiene que recorrer un gran conjunto de datos disponibles, tener una mayor simultaneidad acelera el procesamiento de datos. 
executionPriorityOrder | NewestFirst<br/><br/>OldestFirst | OldestFirst | Determina el orden de los segmentos de datos que se están procesando.<br/><br/>Por ejemplo, si tiene 2 segmentos (que tienen lugar uno a las 4 p.m. y el otro a las 5 p.m.) y ambos están pendientes de ejecución. Si establece que executionPriorityOrder sea NewestFirst, se procesará primero el segmento de las 5 p.m. De forma similar, si establece que executionPriorityORder sea OldestFIrst, se procesará el segmento de las 4 p.m. 
retry | Entero<br/><br/>El valor máximo permitido es 10 | 3 | Número de reintentos antes de que el procesamiento de datos del segmento se marque como error. La ejecución de la actividad de un segmento de datos se vuelve a intentar hasta el número de reintentos especificado. El reintento se realiza tan pronto como sea posible después del error.
timeout | TimeSpan | 00:00:00 | Tiempo de espera para la actividad. Ejemplo: 00:10:00 (implica un tiempo de espera de 10 minutos)<br/><br/>Si no se especifica ningún valor o si es 0, el tiempo de espera es infinito.<br/><br/>Si el tiempo de procesamiento de los datos en un segmento supera el valor de tiempo de espera, se cancela y el sistema vuelve a intentar el procesamiento. El número de reintentos depende de la propiedad retry. Cuando se excede el tiempo de espera, el estado será TimedOut.
delay | TimeSpan | 00:00:00 | Especifica el retraso antes de iniciar el procesamiento de los datos del segmento.<br/><br/>La ejecución de la actividad de un segmento de datos se inicia una vez que transcurra el retraso más allá del tiempo de ejecución esperado.<br/><br/>Ejemplo: 00:10:00 (implica un retraso de 10 minutos)
longRetry | Entero<br/><br/>Valor máximo: 10 | 1 | Número de reintentos largos antes de que la ejecución de los segmentos produzca error.<br/><br/>Los intentos de longRetry se espacian de acuerdo a longRetryInterval. Por tanto, si necesita especificar un tiempo entre reintentos, utilice longRetry. Si se especifican Retry y longRetry, cada intento de longRetry incluirá el número de intentos de Retry y el número máximo de intentos será Retry * longRetry.<br/><br/>Por ejemplo, si tenemos lo siguiente en la directiva de la actividad:<br/>Retry: 3<br/>longRetry: 2<br/>longRetryInterval: 01:00:00<br/><br/>Se supone que existe un solo segmento para ejecutar (el estado es En espera) y la ejecución de la actividad no se puede realizar nunca. Inicialmente habría tres intentos consecutivos de ejecución. Después de cada intento, el estado del segmento sería Retry. Después de los 3 primeros intentos, el estado del segmento sería LongRetry.<br/><br/>Después de una hora (es decir, el valor de longRetryInteval), se produciría otro conjunto de 3 intentos consecutivos de ejecución. Después de eso, el estado del segmento sería Failed y ya no se realizarían más intentos. Por tanto, en total se realizaron 6 intentos.<br/><br/>Nota: si una ejecución se realiza correctamente, el estado del segmento sería Ready y no se realizaría ningún otro reintento.<br/><br/>longRetry puede usarse en situaciones donde llegan datos dependientes a horas no deterministas o el entorno general en el que se produce el procesamiento de datos es poco confiable. En esos casos es posible que realizar reintentos uno tras otro no ayude, mientras que hacerlo después de un intervalo de tiempo puede generar el resultado deseado.<br/><br/>Advertencia: no establezca valores altos para longRetry o longRetryInterval. Normalmente, los valores más altos implican otros problemas sistémicos que se eliminan con esto 
longRetryInterval | TimeSpan | 00:00:00 | El retraso entre reintentos largos 

## Encadenamiento de actividades
Si tiene varias actividades en una canalización y no dependen unas de otras (la salida de una actividad no es la entrada de otra actividad), las actividades se pueden ejecutar en paralelo si los segmentos de datos de entrada para las actividades están listos.

Puede encadenar dos actividades haciendo que el conjunto de datos de salida de una actividad sea el conjunto de datos de entrada de la otra actividad. Las actividades pueden estar en la misma canalización o en canalizaciones diferentes. La segunda actividad se ejecuta solo cuando la primera de ellas se completa correctamente.

Por ejemplo, considere el siguiente caso:
 
1.	La canalización P1 incluye la actividad A1 que requiere el conjunto de datos de entrada externo D1 y genera el conjunto de datos de **salida** **D2**.
2.	La canalización P2 incluye la actividad A2 que requiere una **entrada** del conjunto de datos **D2** y genera el conjunto de datos de salida D3.
 
En este escenario, la actividad A1 se ejecutará cuando los datos externos estén disponibles y se alcance la frecuencia de disponibilidad programada. La actividad A2 se ejecutará cuando estén disponibles los segmentos programados de D2 y se alcance la frecuencia de disponibilidad programada. Si se produce un error en uno de los segmentos del conjunto de datos D2, A2 no se ejecutará para ese segmento hasta que esté disponible.

La Vista Diagrama tendría el aspecto siguiente:

![Encadenamiento de las actividades de dos canalizaciones](./media/data-factory-create-pipelines/chaining-two-pipelines.png)

La Vista Diagrama con ambas actividades en la misma canalización tendría el aspecto siguiente:

![Encadenamiento de las actividades de la misma canalización](./media/data-factory-create-pipelines/chaining-one-pipeline.png)

## Programación y ejecución
Hasta ahora se ha descrito lo que son las canalizaciones y las actividades. También se echó un vistazo a cómo se definen y se vio con detalle las actividades en Factoría de datos de Azure. Ahora veremos cómo se ejecutan.

Una canalización solo está activa entre su hora de inicio y de finalización. No se ejecuta antes de la hora de inicio ni después de la hora de finalización. Si la canalización está en pausa, no se ejecutará, independientemente de su hora de inicio y de finalización. Para que se ejecute una canalización, no debe estar en pausa. De hecho, no se ejecuta la canalización. Se ejecutan las actividades de la canalización. No obstante, lo hacen en el contexto general de la canalización.

Consulte [Programación y ejecución](data-factory-scheduling-and-execution.md) para comprender cómo funciona la programación y la ejecución en Factoría de datos de Azure.

### Procesamiento en paralelo de segmentos
Establezca el valor de **simultaneidad** en la actividad de la definición de JSON en un valor mayor que 1, para que se procesen varios segmentos en paralelo con varias instancias de la actividad en tiempo de ejecución. Esto es realmente útil al procesar los segmentos de relleno de fondo del pasado.


## Creación y administración de una canalización
Factoría de datos de Azure proporciona varios mecanismos para crear e implementar canalizaciones (que a su vez contienen una o varias actividades).

### Uso del Portal de Azure

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com/).
2. Navegue a la instancia de Factoría de datos de Azure en la que desea crear una canalización
3. Haga clic en el icono **Crear e implementar** en la lente **Resumen**.
 
	![Icono Crear e implementar](./media/data-factory-create-pipelines/author-deploy-tile.png)

4. Haga clic en **Nueva canalización** en la barra de comandos.

	![Botón Nueva canalización](./media/data-factory-create-pipelines/new-pipeline-button.png)

5. Aparecerá la ventana del editor con una plantilla de canalización JSON.

	![Editor de canalizaciones](./media/data-factory-create-pipelines/pipeline-in-editor.png)

6. Una vez finalizada la creación de la canalización, haga clic en **Implementar** en la barra de comandos para implementar la canalización.

	**Nota:** durante la implementación, el servicio de Factoría de datos de Azure realiza algunas comprobaciones de validación para ayudar a solucionar algunos problemas comunes. Si se produce un error, se mostrará la información correspondiente. Realice las acciones correctivas y, a continuación, vuelva a implementar la canalización creada. Puede usar el editor para actualizar y eliminar una canalización.

Consulte [Compilación de la primera Data Factory de Azure con el Portal de Azure/Editor de Data Factory](data-factory-build-your-first-pipeline-using-editor.md), un tutorial completo para crear una factoría de datos con una canalización.

### Uso del complemento Visual Studio
Puede usar Visual Studio para crear e implementar canalizaciones en Factoría de datos de Azure. Consulte [Compilación de la primera Data Factory de Azure mediante Microsoft Visual Studio](data-factory-build-your-first-pipeline-using-vs.md), un tutorial completo para crear una factoría de datos con una canalización.


### Uso de Azure PowerShell
Puede usar Azure PowerShell para crear canalizaciones en Factoría de datos de Azure. Digamos que ha definido la canalización JSON en un archivo c:\\DPWikisample.json. Puede cargarlo en su instancia de Factoría de datos de Azure, tal como se muestra en el ejemplo siguiente.

	New-AzureRmDataFactoryPipeline -ResourceGroupName ADF -Name DPWikisample -DataFactoryName wikiADF -File c:\DPWikisample.json

Consulte [Compilación de la primera Data Factory de Azure mediante Azure PowerShell](data-factory-build-your-first-pipeline-using-powershell.md), un tutorial completo para crear una factoría de datos con una canalización.

### Uso del SDK de .NET
También puede crear e implementar la canalización mediante el SDK de .NET. Este mecanismo se puede aprovechar para crear canalizaciones mediante programación. Para obtener más información al respecto, vea [Creación, administración y supervisión de factorías de datos mediante programación](data-factory-create-data-factories-programmatically.md).


### Uso de una plantilla de ARM (Azure Resource Manager)
Puede crear e implementar una canalización mediante una plantilla de Azure Resource Manager (ARM). Para más información, consulte [Tutorial: Compilación de la primera Data Factory de Azure con la plantilla de Azure Resource Manager](data-factory-build-your-first-pipeline-using-arm.md).

### Uso de la API de REST
También puede crear e implementar la canalización mediante las API de REST. Este mecanismo se puede aprovechar para crear canalizaciones mediante programación. Para obtener más información sobre esto, consulte [Creación o actualización de una canalización](https://msdn.microsoft.com/library/azure/dn906741.aspx).


## Administración y supervisión  
Una vez implementada una canalización, puede administrar y supervisar la canalizaciones, segmentos y ejecuciones. Obtenga más información sobre este tema aquí: [Supervisión y administración de canalizaciones](data-factory-monitor-manage-pipelines.md).

## Pasos siguientes

- Conozca [Programación y ejecución en Factoría de datos de Azure](data-factory-scheduling-and-execution.md).
- Obtenga más información sobre las [capacidades de movimiento de datos](data-factory-data-movement-activities.md) y [transformación de datos](data-factory-data-transformation-activities.md) en Factoría de datos de Azure
- Conozca [Administración y supervisión Factoría de datos de Azure](data-factory-monitor-manage-pipelines.md).
- [Creación e implementación de su primera canalización](data-factory-build-your-first-pipeline.md).

<!---HONumber=AcomDC_0629_2016-->