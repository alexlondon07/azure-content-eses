<properties
	pageTitle="Conexión a una máquina virtual de SQL Server (clásica) | Microsoft Azure"
	description="Obtenga información sobre cómo conectarse a SQL Server en ejecución en una máquina virtual en Azure. En este tema se usa el modelo de implementación clásica. Los escenarios varían según la configuración de red y la ubicación del cliente."
	services="virtual-machines-windows"
	documentationCenter="na"
	authors="rothja"
	manager="jhubbard"
	tags="azure-service-management"/>
<tags
	ms.service="virtual-machines-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="vm-windows-sql-server"
	ms.workload="infrastructure-services"
	ms.date="06/23/2016"
	ms.author="jroth" />

# Conexión a una máquina virtual de SQL Server en Azure (implementación clásica)

> [AZURE.SELECTOR]
- [Resource Manager](virtual-machines-windows-sql-connect.md)
- [Clásico](virtual-machines-windows-classic-sql-connect.md)

## Información general

En este tema se muestra cómo conectarse a su instancia de SQL Server que se ejecuta en una máquina virtual de Azure. En él se describen algunos [escenarios de conectividad generales](#connection-scenarios) y se proporcionan los [pasos detallados para configurar la conectividad de SQL Server en una máquina virtual de Azure](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] 

Si utiliza máquinas virtuales de Resource Manager, consulte [Connect to a SQL Server Virtual Machine on Azure (Resource Manager)](virtual-machines-windows-sql-connect.md) (Conexión a una máquina virtual de SQL Server en Azure (Resource Manager)).

## Escenarios de conexión

La forma en que un cliente se conecta a SQL Server que se ejecuta en una máquina virtual varía según la ubicación del cliente y la configuración del equipo y la red. Entre los escenarios se incluyen los siguientes:

- [Conexión a SQL Server en el mismo servicio en la nube](#connect-to-sql-server-in-the-same-cloud-service)
- [Conexión a SQL Server a través de Internet](#connect-to-sql-server-over-the-internet)
- [Conexión a SQL Server en la misma red virtual](#connect-to-sql-server-in-the-same-virtual-network)

>[AZURE.NOTE] Antes de conectarse con cualquiera de estos métodos, debe seguir los [pasos de este artículo para configurar la conectividad](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

### Conexión a SQL Server en el mismo servicio en la nube

Se pueden crear varias máquinas virtuales en el mismo servicio en la nube. Para comprender el escenario de las máquinas virtuales, consulte [Cómo conectar máquinas virtuales con un servicio en la nube o red virtual](virtual-machines-windows-classic-connect-vms.md#connect-vms-in-a-standalone-cloud-service). Este escenario se produce cuando un cliente de una máquina virtual intenta conectarse a SQL Server que se ejecuta en otra máquina virtual del mismo servicio en la nube.

En dicho escenario, puede conectarse mediante la máquina virtual **Nombre** (también mostrada como **Nombre de equipo** o **nombre de host** en el portal). Este es el nombre que ha proporcionado para la máquina virtual durante su creación. Por ejemplo, si ha asignado a su máquina virtual de SQL el nombre **mysqlvm**, una máquina virtual cliente del mismo servicio en la nube podría usar la siguiente cadena de conexión para conectarse:

	"Server=mysqlvm;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

### Conexión a SQL Server a través de Internet

Si quiere conectarse a su motor de base de datos de SQL Server desde Internet, debe crear un extremo de máquina virtual para la comunicación TCP entrante. Este paso de la configuración de Azure dirige el tráfico del puerto TCP de entrada a un puerto TCP al que puede tener acceso la máquina virtual.

Para conectarse a través de internet, debe usar el nombre DNS de la máquina virtual y el número de puerto del punto de conexión de la máquina virtual (configurado más adelante en este artículo). Para buscar el nombre DNS, vaya al Portal de Azure y seleccione **Máquinas virtuales (clásico)**. Después, seleccione la máquina virtual. El **nombre DNS** se muestra en la sección **Información general**.

Por ejemplo, piense en una máquina virtual clásica denominada **mysqlvm** con un nombre DNS de **mysqlvm7777.cloudapp.net** y un punto de conexión de la máquina virtual de **57500**. Suponiendo que la conectividad se ha configurado correctamente, la cadena de conexión siguiente podría usarse para tener acceso a la máquina virtual desde cualquier lugar de Internet:

	"Server=mycloudservice.cloudapp.net,57500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Aunque esto permite a los clientes conectarse a través de Internet, esto no implica que cualquier usuario pueda conectarse a SQL Server. Los clientes externos necesitan el nombre de usuario y la contraseña correctos. Para aumentar la seguridad, no use el conocido puerto 1433 para el extremo público de la máquina virtual. Y, si es posible, considere la posibilidad también de agregar una ACL en el extremo para restringir el tráfico solo a los clientes que permita. Para obtener instrucciones sobre el uso de ACL con puntos de conexión, consulte [Administrar la ACL en un punto de conexión](virtual-machines-windows-classic-setup-endpoints.md#manage-the-acl-on-an-endpoint).

>[AZURE.NOTE] Es importante tener en cuenta que cuando se recurre a esta técnica para comunicarse con SQL Server, todos los datos salientes desde el centro de datos de Azure están sujetos a [precios de transferencia de datos salientes](https://azure.microsoft.com/pricing/details/data-transfers/) normales.

### Conexión a SQL Server en la misma red virtual

[La red virtual](../virtual-network/virtual-networks-overview.md) permite otros escenarios. Puede conectar las máquinas virtuales en la misma red virtual, aunque esas máquinas virtuales existan en servicios en la nube diferentes. Asimismo, con una [VPN de sitio a sitio](../vpn-gateway/vpn-gateway-site-to-site-create.md), puede crear una arquitectura híbrida que conecta las máquinas virtuales con redes y máquinas locales.

Las redes virtuales también permiten unir las máquinas virtuales de Azure a un dominio. Esta es la única forma de usar la autenticación de Windows para SQL Server. Los demás escenarios de conexión requieren la autenticación de SQL con nombres de usuario y contraseñas.

Si va a configurar un entorno de dominio y la autenticación de Windows, no es necesario seguir los pasos de este artículo para configurar el punto de conexión público o los inicios de sesión y la autenticación de SQL. En este escenario, puede conectarse a la instancia de SQL Server especificando el nombre de la máquina virtual de SQL Server en la cadena de conexión. En el siguiente ejemplo se da por hecho también que se configuró la autenticación de Windows y que se concedió al usuario acceso a la instancia de SQL Server.

	"Server=mysqlvm;Integrated Security=true"

## Pasos para configurar la conectividad de SQL Server en una máquina virtual de Azure

Los pasos siguientes muestran cómo conectarse a la instancia de SQL Server a través de Internet mediante SQL Server Management Studio (SSMS). Sin embargo, se aplican los mismos pasos para hacer que la máquina virtual de SQL Server sea accesible para sus aplicaciones, tanto locales como de Azure.

Antes de que pueda conectarse a la instancia de SQL Server desde otra máquina virtual o Internet, debe completar las siguientes tareas descritas en las secciones que aparecen a continuación:

- [Creación de un extremo TCP para la máquina virtual](#create-a-tcp-endpoint-for-the-virtual-machine)
- [Apertura de puertos TCP en el firewall de Windows](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
- [Configuración de SQL Server para escuchar en el protocolo TCP](#configure-sql-server-to-listen-on-the-tcp-protocol)
- [Configuración de SQL Server para autenticación de modo mixto](#configure-sql-server-for-mixed-mode-authentication)
- [Creación de inicios de sesión para la autenticación de SQL Server](#create-sql-server-authentication-logins)
- [Determinación del nombre DNS de la máquina virtual](#determine-the-dns-name-of-the-virtual-machine)
- [Conexión al motor de base de datos desde otro equipo](#connect-to-the-database-engine-from-another-computer)

El siguiente diagrama resume la ruta de conexión:

![Conexión a una máquina virtual de SQL Server](../../includes/media/virtual-machines-sql-server-connection-steps/SQLServerinVMConnectionMap.png)

[AZURE.INCLUDE [Conexión a SQL Server en un punto de conexión TCP clásico de máquina virtual](../../includes/virtual-machines-sql-server-connection-steps-classic-tcp-endpoint.md)]

[AZURE.INCLUDE [Conexión a SQL Server en una máquina virtual](../../includes/virtual-machines-sql-server-connection-steps.md)]

[AZURE.INCLUDE [Pasos clásicos para la conexión a SQL Server en una máquina virtual](../../includes/virtual-machines-sql-server-connection-steps-classic.md)]

## Pasos siguientes

Si planea usar también grupos de disponibilidad AlwaysOn para alta disponibilidad y recuperación ante desastres, considere la posibilidad de implementar un agente de escucha. Los clientes de la base de datos se conectan al agente de escucha en lugar de directamente a una de las instancias de SQL Server. El agente de escucha enruta los clientes a la réplica principal del grupo de disponibilidad. Para obtener más información, consulte [Configuración de un agente de escucha con ILB para grupos de disponibilidad AlwaysOn en Azure](virtual-machines-windows-classic-ps-sql-int-listener.md).

Es importante revisar todos los procedimientos recomendados de seguridad para SQL Server que se ejecuta en una máquina virtual de Azure. Para obtener más información, consulte [Consideraciones de seguridad para SQL Server en máquinas virtuales de Azure](virtual-machines-windows-sql-security.md).

[Explore la ruta de aprendizaje](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) para SQL Server en máquinas virtuales de Azure.

Para ver otros temas sobre la ejecución de SQL Server en máquinas virtuales de Azure, consulte [SQL Server en máquinas virtuales de Azure](virtual-machines-windows-sql-server-iaas-overview.md).

<!----HONumber=AcomDC_0629_2016-->