<properties
	pageTitle="Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure"
	description="Proporciona una lista de límites, cuotas y restricciones de suscripción y servicio comunes de Azure. Esto incluye información acerca de cómo aumentar los límites junto con los valores máximos."
	services=""
	documentationCenter=""
	authors="rothja"
	manager="jeffreyg"
	editor="monicar"/>

<tags
	ms.service="multiple"
	ms.workload="multiple"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/08/2016"
	ms.author="jroth"/>

# Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure

## Información general

En este documento se especifican algunos de los límites más comunes de Microsoft Azure. Tenga en cuenta que actualmente esto no cubre todos los servicios de Azure. Con el tiempo, estos límites se expandirán y actualizarán para abarcar más de la plataforma.

> [AZURE.NOTE] Si desea aumentar el límite por encima del **Límite predeterminado**, [abra una solicitud de servicio al cliente en línea sin cargo alguno](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). Los límites no se pueden subir por encima del valor de **Límite máximo** que figura en las tablas siguientes. Si la columna **Límite máximo** no existe, el recurso especificado no tiene límites ajustables.

## Límites y Administrador de recursos de Azure

Ahora es posible combinar varios recursos de Azure en un único grupo de recursos de Azure. Al utilizar grupos de recursos, los límites que una vez fueron globales se convierten en administrados a nivel regional con el Administrador de recursos de Azure. Para más información sobre los grupos de recursos de Azure, consulte [Información general de Azure Resource Manager](resource-group-overview.md).

En los límites siguientes, se ha agregado una nueva tabla para reflejar las diferencias en los límites cuando se usa el Administrador de recursos de Azure. Por ejemplo, hay una tabla de **Límites de suscripción** y una tabla de **Límites de suscripción - Administrador de recursos de Azure**. Cuando un límite se aplica a ambos escenarios, solo se muestra en la primera tabla. A menos que se indique lo contrario, los límites son globales en todas las regiones.

> [AZURE.NOTE] Es importante destacar que las cuotas de los recursos de los grupos de recursos de Azure son accesibles para su suscripción en función de la región y no en función de la suscripción, como las cuotas de administración de servicios. Usemos las cuotas de núcleo como ejemplo. Si necesita solicitar un aumento de cuota con compatibilidad para núcleos, deberá decidir el número de núcleos que desea usar en las distintas regiones y, a continuación, realizar una solicitud específica para las cuotas principales del grupo de recursos de Azure para las cantidades y regiones que desee. Por lo tanto, si necesita usar 30 núcleos en Europa Occidental para ejecutar la aplicación, deberá solicitar específicamente 30 núcleos en Europa Occidental. Pero no tendrá un aumento de la cuota de núcleos en ninguna otra región: solo Europa Occidental tendrá la cuota de 30 núcleos.
<!-- -->
Como resultado, puede que le resulte útil considerar decidir cuáles deben ser sus cuotas de grupos de recursos de Azure para su carga de trabajo en cada región, y solicitar esa cantidad en cada región en la que está considerando efectuar la implementación. Consulte [solucionar problemas de implementación](./resource-manager-common-deployment-errors.md) para obtener más ayuda para descubrir las cuotas actuales para regiones específicas.


## Límites específicos del servicio

- [Active Directory](#active-directory-limits)
- [Administración de API](#api-management-limits)
- [Servicio de aplicaciones](#app-service-limits)
- [Application Insights](#application-insights-limits)
- [Automatización](#automation-limits)
- [Caché en Redis de Azure](#azure-redis-cache-limits)
- [Azure RemoteApp](#azure-remoteapp-limits)
- [Copia de seguridad](#backup-limits)
- [Lote](#batch-limits)
- [Servicios de BizTalk](#biztalk-services-limits)
- [SERVICIO CDN](#cdn-limits)
- [Servicios en la nube](#cloud-services-limits)
- [Factoría de datos](#data-factory-limits)
- [Análisis de Data Lake](#data-lake-analytics-limits)
- [DNS](#dns-limits)
- [DocumentDB](#documentdb-limits)
- [Centros de eventos](#event-hubs-limits)
- [Centro de IoT](#iot-hub-limits)
- [Almacén de claves](#key-vault-limits)
- [Servicios multimedia](#media-services-limits)
- [Mobile Engagement](#mobile-engagement-limits)
- [Servicios móviles](#mobile-services-limits)
- [Multi-Factor Authentication](#multi-factor-authentication)
- [Redes](#networking-limits)
- [Servicio Centro de notificaciones](#notification-hub-service-limits)
- [Visión operativa](#operational-insights-limits)
- [Grupo de recursos](#resource-group-limits)
- [Programador](#scheduler-limits)
- [Search](#search-limits)
- [Bus de servicio](#service-bus-limits)
- [Recuperación de sitios](#site-recovery-limits)
- [Base de datos SQL](#sql-database-limits)
- [Almacenamiento](#storage-limits)
- [Sistema de StorSimple](#storsimple-system-limits)
- [Análisis de transmisiones](#stream-analytics-limits)
- [Suscripción](#subscription-limits)
- [Administrador de tráfico](#traffic-manager-limits)
- [Máquinas virtuales](#virtual-machines-limits)
- [Conjuntos de escalado de máquina virtual](#virtual-machine-scale-sets-limits)


### Límites de suscripción
#### Límites de suscripción
[AZURE.INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### Límites de suscripción: Administrador de recursos de Azure

Los límites siguientes se aplican al usar el Administrador de recursos de Azure y los grupos de recursos de Azure. Los límites que no han cambiado con el Administrador de recursos de Azure no se enumeran a continuación. Consulte la tabla anterior para obtener información acerca de esos límites.

[AZURE.INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]


### Límites de grupos de recursos

[AZURE.INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### Límites de máquinas virtuales
#### Límites de máquinas virtuales
[AZURE.INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]


#### Límites de máquinas virtuales: Administrador de recursos de Azure

Los límites siguientes se aplican al usar el Administrador de recursos de Azure y los grupos de recursos de Azure. Los límites que no han cambiado con el Administrador de recursos de Azure no se enumeran a continuación. Consulte la tabla anterior para obtener información acerca de esos límites.

[AZURE.INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### Límites de los conjuntos de escalas de máquinas virtuales

[AZURE.INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### Límites de red

[AZURE.INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### Límites de red
[AZURE.INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### Límites de Administrador de tráfico

[AZURE.INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### Límites de DNS

[AZURE.INCLUDE [dns-limits](../includes/dns-limits.md)]

### Límites de almacenamiento

Para más información sobre los límites de la cuenta de almacenamiento, vea [Objetivos de escalabilidad y rendimiento de Almacenamiento de Azure](../articles/storage/storage-scalability-targets.md).

#### Límites del servicio de almacenamiento

[AZURE.INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

#### Límites de discos de máquinas virtuales

[AZURE.INCLUDE [azure-storage-limits-vm-disks](../includes/azure-storage-limits-vm-disks.md)]

Consulte [Tamaños de máquina virtual](../articles/virtual-machines/virtual-machines-linux-sizes.md) para obtener información adicional.

**Cuentas de almacenamiento estándar**

[AZURE.INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

**Cuentas de almacenamiento premium**

[AZURE.INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

#### Límites de proveedor de recursos de almacenamiento

[AZURE.INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]


### Límites de Servicios en la nube

[AZURE.INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]


### Límites de Servicio de aplicaciones
Entre los siguientes límites del Servicio de aplicaciones se incluyen límites para aplicaciones web, aplicaciones móviles, aplicaciones de la API y aplicaciones lógicas.

[AZURE.INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### Límites de Programador

[AZURE.INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### Límites de lote

[AZURE.INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

###Límites de los servicios de BizTalk
La tabla siguiente muestra los límites de los servicios de Biztalk de Azure.

[AZURE.INCLUDE [biztalk-services-service-limits](../includes/biztalk-services-service-limits.md)]


### Límites de DocumentDB

[AZURE.INCLUDE [azure-documentdb-limits](../includes/azure-documentdb-limits.md)]

Las cuotas que aparecen con un asterisco (*) [se pueden ajustar poniéndose en contacto con el soporte técnico de Azure](./documentdb/documentdb-increase-limits.md).

### Límites de Mobile Engagement

[AZURE.INCLUDE [azure-mobile-engagement-limits](../includes/azure-mobile-engagement-limits.md)]


### Límites de búsqueda

Los planes de tarifa determinan la capacidad y los límites de su servicio de búsqueda. Los planes incluyen:

- El servicio multiempresa *Gratis*, compartido con otros suscriptores de Azure, se ha diseñado para proyectos de evaluación y de desarrollo de pequeña envergadura.
- El nivel *Básico* proporciona recursos informáticos dedicados para cargas de trabajo de producción en una escala menor, con hasta 3 réplicas para cargas de trabajo de consulta de alta disponibilidad.
- El nivel *Estándar (S1, S2, S3, S3 de alta densidad)* es para mayores cargas de trabajo de producción. Existen varios niveles en el nivel estándar para que pueda elegir una configuración de recursos para escenarios específicos.

**Límites por suscripción**

[AZURE.INCLUDE [azure-search-limits-per-subscription](../includes/azure-search-limits-per-subscription.md)]

**Límites por servicio de búsqueda**

[AZURE.INCLUDE [azure-search-limits-per-service](../includes/azure-search-limits-per-service.md)]

Para más información sobre otros límites, incluido el tamaño de documento, las consultas por segundo, las claves, las solicitudes y las respuestas, consulte [Límites de servicio en la Búsqueda de Azure](search/search-limits-quotas-capacity.md).

### Límites de Servicios multimedia

[AZURE.INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### Límites de red CDN

[AZURE.INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### Límites de Servicios móviles

[AZURE.INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### Límites de servicio de Centro de notificaciones

[AZURE.INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]

### Límites de los Centros de eventos

[AZURE.INCLUDE [azure-servicebus-limits](../includes/event-hubs-limits.md)]

### Límites de Bus de servicio

[AZURE.INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### Límites de Centro de IoT

[AZURE.INCLUDE [azure-iothub-limits](../includes/iot-hub-limits.md)]

### Límites de Factoría de datos

[AZURE.INCLUDE [azure-data-factory-limits](../includes/azure-data-factory-limits.md)]

### Límites de Análisis de Data Lake
[AZURE.INCLUDE [azure-data-lake-analytics-limits](../includes/azure-data-lake-analytics-limits.md)]

### Límites de Análisis de transmisiones

| Identificador de límites | Límite | Comentarios |
|----------------- | ------------|--------- |
| Número máximo de unidades de streaming por suscripción por región | 50 | Se puede realizar una solicitud para aumentar las unidades de streaming de su suscripción más allá de 50 poniéndose en contacto con [Soporte técnico de Microsoft](https://support.microsoft.com/es-ES). |
| Rendimiento máximo de una unidad de streaming | 1MB/s* | El rendimiento máximo por unidad de streaming depende del escenario. El rendimiento real puede ser inferior y depende de la complejidad y partición de la consulta. Encontrará más detalles en el artículo [Escalación de trabajos de Azure Stream Analytics](../articles/stream-analytics/stream-analytics-scale-jobs.md). |

### Límites de Active Directory

[AZURE.INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]


### Límites de Azure RemoteApp

[AZURE.INCLUDE [azure-remoteapp-limits](../includes/azure-remoteapp-limits.md)]

### Límites del sistema StorSimple

[AZURE.INCLUDE [storsimple-limits-table](../includes/storsimple-limits-table.md)]


### Límites de Visión operativa

[AZURE.INCLUDE [operational-insights-limits](../includes/operational-insights-limits.md)]

### Límites de Copia de seguridad

[AZURE.INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### Límites de Site Recovery

[AZURE.INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### Límites de Application Insights

[AZURE.INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### Límites de Administración de API

[AZURE.INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### Límites de Caché en Redis de Azure

[AZURE.INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### Límites del Almacén de claves

[AZURE.INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### Multi-Factor Authentication
[AZURE.INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### Límites de automatización
[AZURE.INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### Límites de Base de datos SQL

Para conocer los límites de Base de datos SQL, vea [Límites de recursos de Base de datos SQL](sql-database/sql-database-resource-limits.md).

## Consulte también

[Concepto de límites de Azure y aumento de los mismos](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)

[Tamaños de máquinas virtuales y servicios en la nube de Azure](virtual-machines/virtual-machines-linux-sizes.md)

[Tamaños de servicios en la nube](cloud-services/cloud-services-sizes-specs.md)

<!---HONumber=AcomDC_0720_2016-->