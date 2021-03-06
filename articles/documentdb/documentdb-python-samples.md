<properties 
	pageTitle="Ejemplos de Python NoSQL para DocumentDB | Microsoft Azure" 
	description="Encontrará ejemplos de NoSQL Python en GitHub sobre tareas comunes en DocumentDB, por ejemplo, operaciones CRUD para documentos JSON en bases de datos NoSQL." 
	keywords="Ejemplos de Python"
	services="documentdb" 
	authors="aliuy" 
	manager="jhubbard" 
	editor="monicar" 
	documentationCenter="python"/>

<tags 
	ms.service="documentdb" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="04/18/2016" 
	ms.author="andrl"/>


# Ejemplos de Python de DocumentDB

> [AZURE.SELECTOR]
- [Ejemplos de .NET](documentdb-dotnet-samples.md)
- [Ejemplos de Node.js](documentdb-nodejs-samples.md)
- [Ejemplos de Python](documentdb-python-samples.md)
- [Galería de ejemplos de código de Azure](https://azure.microsoft.com/documentation/samples/?service=documentdb)

En el repositorio de GitHub [azure-documentdb-python](https://github.com/Azure/azure-documentdb-python/tree/master/samples) se incluyen soluciones de ejemplo que realizan operaciones CRUD y otras comunes en recursos de Azure DocumentDB. Este artículo ofrece:

- Vínculos a las tareas de cada uno de los archivos de proyecto de ejemplo de Python.
- Vínculos al contenido de referencia de la API relacionada.

**Requisitos previos**

1. Para usar estos ejemplos de Python, necesita una cuenta de Azure:
    - Puede [abrir una cuenta de Azure de manera gratuita](https://azure.microsoft.com/pricing/free-trial/) - Obtiene crédito que puede usar para probar los servicios de Azure de pago, e incluso una vez agotado este, podrá mantener la cuenta y usar servicios gratuitos de Azure, como Sitios web. Nunca se la hará ningún cargo en la tarjeta de crédito, a menos que cambie explícitamente la configuración y pida que se le realice algún cargo.
   - Puede [activar los beneficios de suscriptor de Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/): su suscripción a Visual Studio le proporciona crédito todos los meses que puede usar con servicios de Azure de pago.
2. También necesita el [SDK de Python](documentdb-sdk-python.md).

    > [AZURE.NOTE] Cada ejemplo es independiente, se configura por sí mismo y se limpia solo a continuación. Por lo tanto, los ejemplos emiten varias llamadas a [document\_client.CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html). Cada vez que esto ocurre, se cobra a la suscripción una hora de uso de acuerdo con el nivel de rendimiento de la colección que se va a crear.

## Ejemplos de base de datos

El archivo [Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement/Program.py) del proyecto [DatabaseManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement) muestra cómo realizar las tareas siguientes.

Tarea | Referencia de API
--- | ---
[Creación de una base de datos](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L65-L76) | [document\_client.CreateDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Consultar una base de datos de una cuenta](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L49-L62) | [document\_client.QueryDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Leer una base de datos por identificador](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L79-L96) | [document\_client.ReadDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Enumerar bases de datos de una cuenta](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L99-L110) | [document\_client.ReadDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Eliminar una base de datos](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L113-L126) | [document\_client.DeleteDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)

## Ejemplos de colección 

El archivo [Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement/Program.py) del proyecto [CollectionManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement) muestra cómo realizar las tareas siguientes.

Tarea | Referencia de API
--- | ---
[Creación de una colección](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L84-L135) | [document\_client.CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Leer una lista de todas las colecciones en una base de datos](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L198-L225) | [document\_client.ListCollections](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Obtención de una colección por identificador](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L178-L195) | [document\_client.ReadCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Obtención del nivel de rendimiento de una colección](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L139-L161) | [DocumentQueryable.QueryOffers](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Cambio del nivel de rendimiento de una colección](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L163-L175) | [document\_client.ReplaceOffer](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Eliminación de un colección](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L212-L225) | [document\_client.DeleteCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)

<!---HONumber=AcomDC_0720_2016-->