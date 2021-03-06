<properties
	pageTitle="Guía de seguridad de Almacenamiento de Azure | Microsoft Azure"
	description="Detalles de los distintos métodos de seguridad de Almacenamiento de Azure, incluidos, entre otros, el control de acceso basado en rol, el Cifrado del servicio de almacenamiento, el Cifrado de cliente, SMB 3.0 y el Cifrado de discos de Azure."
	services="storage"
	documentationCenter=".net"
	authors="robinsh"
	manager="carmonm"
	editor="tysonn"/>

<tags
	ms.service="storage"
	ms.workload="storage"
	ms.tgt_pltfrm="na"
	ms.devlang="dotnet"
	ms.topic="article"
	ms.date="06/16/2016"
	ms.author="robinsh"/>

#Guía de seguridad de Almacenamiento de Azure

##Información general

Almacenamiento de Azure pone a su disposición diferentes funciones de seguridad que, al usarlas en conjunto, permiten a los desarrolladores crear aplicaciones seguras. La propia cuenta de almacenamiento se puede proteger mediante el control de acceso basado en rol y Azure Active Directory. Los datos se pueden proteger en tránsito entre una aplicación y Azure usando [cifrado de cliente](storage-client-side-encryption.md), HTTPS o SMB 3.0. Los datos se pueden configurar para que se cifren automáticamente cuando se escriben en Almacenamiento de Azure mediante el [cifrado del servicio de almacenamiento (SSE)](storage-service-encryption.md). Se puede establecer el cifrado de los discos de datos y del sistema operativo utilizados por máquinas virtuales mediante el [Cifrado de discos de Azure](../azure-security-disk-encryption.md). Se puede conceder acceso delegado a los objetos de datos de Almacenamiento de Azure mediante las [Firmas de acceso compartido](storage-dotnet-shared-access-signature-part-1.md).

Este artículo ofrece una visión general de cada una de estas características de seguridad que se pueden usar con Almacenamiento de Azure. Se incluyen vínculos a artículos en los que se detalla cada una de ellas, así que puede examinar un tema determinado con mayor detalle si así lo desea.

Estos son los temas que se tratarán en este artículo:

-   [Seguridad en el plano de la administración](#management-plane-security): protección de la cuenta de almacenamiento

    El plano de la administración está compuesto por los recursos que se usan para administrar la cuenta de almacenamiento. En esta sección, hablaremos sobre el modelo de implementación de Azure Resource Manager y cómo usar el control de acceso basado en rol (RBAC) para controlar el acceso a sus cuentas de almacenamiento. También hablaremos acerca de cómo administrar las claves de la cuenta de almacenamiento y cómo volver a generarlas.

-   [Seguridad en el plano de los datos](#data-plane-security): protección del acceso a los datos

    En esta sección, analizaremos el modo de permitir el acceso a los objetos de datos reales de la cuenta de almacenamiento, como blobs, archivos, colas y tablas, y el uso de Firmas de acceso compartido y Directivas de acceso almacenadas. Nos fijaremos en las Firmas de acceso compartido tanto a nivel de servicio como a nivel de cuenta. También veremos cómo limitar el acceso a una dirección IP específica (o un intervalo de direcciones IP), cómo limitar el protocolo utilizado para HTTPS y cómo revocar una Firma de acceso compartido sin esperar a que expire.

-   [Cifrado en tránsito](#encryption-in-transit)

    En esta sección se describe cómo proteger los datos cuando se transfieren a o desde Almacenamiento de Azure. Hablaremos sobre el uso recomendado de HTTPS y el cifrado usado por SMB 3.0 para recursos compartidos de archivos de Azure. También echaremos un vistazo al Cifrado de cliente, que permite cifrar los datos antes de transferirlos al Almacenamiento en una aplicación cliente y descifrarlos una vez transferidos desde este servicio.

-   [Cifrado en reposo](#encryption-at-rest)

    Hablaremos acerca del cifrado del servicio de almacenamiento (SSE) y el modo de habilitarlo para una cuenta de almacenamiento, lo que conlleva que los blobs en bloques, los blobs en páginas y los blobs en anexos se cifren automáticamente al escribir en Almacenamiento de Azure. También veremos cómo puede usar Cifrado de discos de Azure y exploraremos los casos y las diferencias básicas entre Cifrado de discos, Cifrado del servicio de almacenamiento y Cifrado en el cliente. Asimismo, estudiaremos brevemente el cumplimiento de la norma FIPS para equipos del Gobierno de EE. UU.

-   Uso de [Análisis de almacenamiento](#storage-analytics) para auditar el acceso de Almacenamiento de Azure

    En esta sección se describe cómo buscar información en los registros de análisis de almacenamiento para una solicitud. Echaremos un vistazo a los datos de registro del análisis de almacenamiento real y aprenderemos a discernir si una solicitud se realiza con una clave de cuenta de Almacenamiento, una Firma de acceso compartido o de manera anónima, y si se pudo completar o no.

-   [Habilitación de clientes basados en explorador mediante el uso compartido de recursos entre orígenes](#Cross-Origin-Resource-Sharing-CORS)

    En esta sección se analiza cómo permitir el uso compartido de recursos entre orígenes. Abordaremos el acceso entre dominios y cómo administrarlo con las funcionalidades del uso compartido de recursos entre orígenes integradas en Almacenamiento de Azure.

##Seguridad en el plano de la administración

El plano de la administración consta de las operaciones que afectan a la propia cuenta de almacenamiento. Por ejemplo, puede crear o eliminar una cuenta de almacenamiento, obtener una lista de cuentas de almacenamiento en una suscripción, recuperar las claves de la cuenta de almacenamiento o volver a generar las claves de la cuenta de almacenamiento.

Cuando se crea una nueva cuenta de almacenamiento, se selecciona un modelo de implementación clásico o de Resource Manager. El modelo clásico de creación de recursos de Azure únicamente permite acceder a toda la suscripción en su conjunto o bien a nada de ella, y lo mismo ocurre con la cuenta de almacenamiento.

Esta guía se centra en el modelo de Resource Manager, que es el medio recomendado para crear cuentas de almacenamiento. Con las cuentas de almacenamiento de Resource Manager, en lugar de proporcionar acceso a toda la suscripción, puede controlar el acceso en un nivel más detallado en el plano de administración mediante el control de acceso basado en rol (RBAC).

###Protección de su cuenta de almacenamiento con el control de acceso basado en rol (RBAC)

Vamos a ver qué es el control de acceso basado en rol y cómo puede utilizarlo. Cada una de las suscripciones de Azure está asociada a una instancia de Azure Active Directory. Es posible conceder acceso a los usuarios, los grupos y las aplicaciones desde ese directorio a la administración de recursos de la suscripción de Azure que usan el modelo de implementación de Resource Manager. Esto se conoce como control de acceso basado en rol (RBAC). Para administrar este acceso, puede utilizar el [Portal de Azure](https://portal.azure.com/), [las herramientas de la CLI de Azure](../xplat-cli-install.md), [PowerShell](../powershell-install-configure.md) o [las API de REST del proveedor de recursos de Almacenamiento de Azure](https://msdn.microsoft.com/library/azure/mt163683.aspx).

Con el modelo de Resource Manager, se coloca la cuenta de almacenamiento en un grupo de recursos y se controla el acceso al plano de la administración de dicha cuenta de almacenamiento específica mediante Azure Active Directory. Por ejemplo, puede proporcionar a usuarios específicos la posibilidad de tener acceso a las claves de la cuenta de almacenamiento, mientras que otros usuarios pueden ver información sobre la cuenta de almacenamiento pero no pueden tener acceso a sus claves.

####Concesión de acceso

El acceso se concede mediante la asignación de rol RBAC adecuado a los usuarios, grupos y aplicaciones en el ámbito correcto. Para conceder acceso a toda la suscripción, asigne un rol a nivel de suscripción. Puede conceder acceso a todos los recursos de un grupo de recursos mediante la concesión de permisos al propio grupo de recursos. También puede asignar roles específicos a recursos específicos, como cuentas de almacenamiento.

Estos son los puntos principales que necesita saber acerca del uso del control de acceso basado en rol para tener acceso a las operaciones de administración de una cuenta de Almacenamiento de Azure:

-   Al asignar el acceso, básicamente asigna un rol a la cuenta a la que desea tener acceso. Puede controlar el acceso a las operaciones que se utilizan para administrar esa cuenta de almacenamiento, pero no a los objetos de datos de la cuenta. Por ejemplo, puede conceder permiso para recuperar las propiedades de la cuenta de almacenamiento (por ejemplo, redundancia), pero no a un contenedor o los datos dentro de un contenedor de Almacenamiento de blobs.

-   Para que otro usuario pueda tener acceso a los objetos de datos de la cuenta de almacenamiento, puede concederle permiso para leer las claves de la cuenta de almacenamiento. De este modo, ese usuario podrá usar esas claves para tener acceso a blobs, colas, tablas y archivos.

-   Los roles pueden asignarse específicamente a una cuenta de usuario, un grupo de usuarios o una aplicación.

-   Cada rol tiene una lista de acciones y no acciones. Por ejemplo, el rol Colaborador de la máquina Virtual tiene una acción de "listKeys" que permite que las claves de la cuenta de almacenamiento se puedan leer. El colaborador tiene "no acciones", como actualizar el acceso de los usuarios en Active Directory.

-   Entre los roles para el almacenamiento se incluyen, entre otros, los siguientes:

	-	Propietario: puede administrar todo, incluido el acceso.

    -	Colaborador: puede hacer lo mismo que el propietario, salvo asignar acceso. Un usuario con este rol puede ver y volver a generar las claves de la cuenta de almacenamiento. Con las claves de la cuenta de almacenamiento, puede tener acceso a los objetos de datos.

    -	Lector: puede ver información sobre la cuenta de almacenamiento, excepto los secretos. Por ejemplo, si asigna un rol con permisos de lectura sobre la cuenta de almacenamiento a otro usuario, este puede ver las propiedades de la cuenta de almacenamiento, pero no puede realizar cambios en las propiedades ni ver las claves de la cuenta de almacenamiento.

    -	Colaborador de la cuenta de almacenamiento: puede administrar la cuenta de almacenamiento. Puede leer los grupos de recursos y los recursos de la suscripción, así como crear y administrar implementaciones de los grupos de recursos de la suscripción. También puede acceder a las claves de la cuenta de almacenamiento, lo que a su vez implica que puede acceder al plano de los datos.

    -	Administrador de acceso de usuarios: puede administrar el acceso de los usuarios a la cuenta de almacenamiento. Por ejemplo, puede conceder acceso de lectura a un usuario específico.

    -	Colaborador de la máquina virtual: puede administrar máquinas virtuales, pero no la cuenta de almacenamiento o la red virtual a la que están conectadas. Este rol puede listar las claves de la cuenta de almacenamiento, lo que significa que el usuario a quien asigne este rol puede actualizar el plano de los datos.

		Para que un usuario pueda crear una máquina virtual, tiene que poder crear el archivo VHD correspondiente en una cuenta de almacenamiento. Para ello, es necesario que pueda recuperar la clave de la cuenta de almacenamiento y pasarla a la API que está creando la máquina virtual. Por lo tanto, es necesario que disponga de este permiso para listar las claves de la cuenta de almacenamiento.

- La posibilidad de definir roles personalizados es una característica que le permite crear un conjunto de acciones desde una lista de acciones disponibles que se puede llevar a cabo sobre recursos de Azure.

- Es necesario que el usuario se haya configurado en Azure Active Directory para poder asignarle un rol.

- Puede crear un informe de quién concedió/revocó qué tipo de acceso a quién y en qué ámbito con PowerShell o la CLI de Azure.

####Recursos

-   [Control de acceso basado en roles de Azure Active Directory](../active-directory/role-based-access-control-configure.md)

    En este artículo se explica el control de acceso basado en rol de Azure Active Directory y su funcionamiento.

-   [RBAC: Roles integrados](../active-directory/role-based-access-built-in-roles.md)

    En este artículo se detalla todas las funciones integradas disponibles en el control de acceso basado en rol.

-   [Descripción de la implementación del Administrador de recursos y la implementación clásica](../resource-manager-deployment-model.md)

    En este artículo se explica el modelo de implementación clásica y de Resource Manager, y se detallan las ventajas de utilizar Resource Manager y los grupos de recursos.

-   [Proceso, red y proveedores de almacenamiento de Azure en el Administrador de recursos de Azure](../virtual-machines/virtual-machines-windows-compare-deployment-models.md)

    En este artículo se explica cómo funcionan el proceso, la red y los proveedores de almacenamiento de Azure en el modelo de Resource Manager.

-   [Administración del control de acceso basado en rol con la API de REST](../active-directory/role-based-access-control-manage-access-rest.md)

	En este artículo se muestra cómo utilizar la API de REST para administrar el control de acceso basado en rol.

-   [Referencia de API de REST de proveedor de recursos de almacenamiento de Azure](https://msdn.microsoft.com/library/azure/mt163683.aspx)

	Esta es la referencia para las API que puede usar para administrar la cuenta de almacenamiento mediante programación.

-   [Developer’s guide to auth with Azure Resource Manager API (Guía del desarrollador para la autenticación con la API de Azure Resource Manager)](http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/)

	En este artículo se muestra cómo realizar la autenticación mediante las API de Resource Manager.

-   [Role-Based Access Control for Microsoft Azure from Ignite (Control de acceso basado en rol para Microsoft Azure de Ignite)](https://channel9.msdn.com/events/Ignite/2015/BRK2707)

    Este es un vínculo a un vídeo de Channel 9 de la conferencia de MS Ignite de 2015. En esta sesión, se abordan las funciones de notificación y administración de acceso en Azure y se exploran los procedimientos recomendados en torno a la protección del acceso a suscripciones de Azure con Azure Active Directory.

###Administración de las claves de la cuenta de almacenamiento

Las claves de la cuenta de almacenamiento son cadenas de 512 bits creadas por Azure que, junto con el nombre de la cuenta de almacenamiento, pueden utilizarse para tener acceso a los objetos de datos almacenados en la cuenta de almacenamiento; por ejemplo, blobs, entidades dentro de una tabla, mensajes de una cola y archivos en un recurso compartido de archivos de Azure. El control del acceso a las claves de la cuenta de almacenamiento controla el acceso al plano de los datos de esa cuenta de almacenamiento.

Cada cuenta de almacenamiento tiene dos claves que se conocen como "Clave 1" y "Clave 2" en el [Portal de Azure](http://portal.azure.com/) y en los cmdlets de PowerShell. Es posible volver a generarlas manualmente con diferentes métodos, como el uso del [Portal de Azure](https://portal.azure.com/), PowerShell o la CLI de Azure, o mediante programación con la biblioteca de cliente de almacenamiento de .NET o la API de REST de Servicios de almacenamiento de Azure.

Hay varias razones que justifican volver a generar las claves de la cuenta de almacenamiento.

-   Puede cambiarlas de nuevo de manera periódica por motivos de seguridad.

-   Podría volver a generar las claves de la cuenta de almacenamiento si alguien consiguiera atacar una aplicación y recuperar la clave que se codificó de forma rígida o se guardó en un archivo de configuración, con lo que tendría acceso completo a la cuenta de almacenamiento.

-   También podría volver a generar la clave si el equipo utilizara una aplicación de Explorador de almacenamiento que conserve la clave de la cuenta de almacenamiento y uno de los miembros abandonara el equipo. La aplicación seguiría funcionando, permitiendo a ese antiguo miembro acceder a la cuenta de almacenamiento una vez que ya no forma parte del equipo. Este es realmente el motivo principal por el que se crean Firmas de acceso compartido de nivel de cuenta, ya que al utilizarlas no es necesario almacenar las claves de acceso en un archivo de configuración.

####Plan de regeneración de claves

No es buena idea limitarse a volver a generar la clave que está utilizando sin una determinada planificación. Si no lo hace así, podría cortar todo el acceso a esa cuenta de almacenamiento, lo que podría provocar una interrupción importante. Este es el motivo por el cual hay dos claves. Puede volver a generar una de ellas y luego la otra en vez de hacerlo de manera simultánea.

Antes de volver a generar las claves, asegúrese de que dispone de una lista de todas las aplicaciones que dependen de la cuenta de almacenamiento, así como cualquier otro servicio que se use en Azure. Por ejemplo, si usa Servicios multimedia de Azure que dependen de su cuenta de almacenamiento, debe volver a sincronizar las claves de acceso con los servicios multimedia después de volver a generar las claves. Si está utilizando otras aplicaciones, como un explorador de almacenamiento, debe proporcionarles también las nuevas claves. Observe que si tiene máquinas virtuales cuyos archivos VHD se almacenan en la cuenta de almacenamiento, estos no se verán afectados por la regeneración de las claves de la cuenta de almacenamiento.

Puede volver a generar las claves en el Portal de Azure. Una vez que se vuelven a generar, las claves pueden tardar hasta 10 minutos en sincronizarse en todos los servicios de almacenamiento.

Cuando esté listo, este es el proceso general que detalla cómo se debe cambiar la clave. En este caso, suponemos que actualmente usa la Clave 1 y que va a cambiar todo para usar la Clave 2 en su lugar.

1.  Vuelva a generar la Clave 2 para asegurarse de que sea segura. Puede hacerlo en el Portal de Azure.

2.  En todas las aplicaciones donde se almacena la clave de almacenamiento, cámbiela para usar el nuevo valor de la Clave 2. Pruebe y publique la aplicación.

3.  Una vez que todas las aplicaciones y los servicios estén activos y se estén ejecutando correctamente, vuelva a generar la Clave 1. De este modo se asegura de que nadie a quien no haya proporcionado expresamente la nueva clave seguirá teniendo acceso a la cuenta de almacenamiento.

Si actualmente utiliza la Clave 2, siga este mismo proceso pero invierta los nombres de las claves.

Puede migrar en un par de días, cambiando cada aplicación para que use la nueva clave y publicándola. Cuando lo haya hecho con todas, debe volver y generar de nuevo la clave antigua para que deje de funcionar.

Otra opción consiste en poner la clave de la cuenta de almacenamiento en un [Almacén de claves de Azure](https://azure.microsoft.com/services/key-vault/) como un secreto y hacer que las aplicaciones recuperen la clave desde esa ubicación. Así, cuando vuelva a generar la clave y actualice el Almacén de claves de Azure, no será necesario volver a implementar las aplicaciones porque tomarán la nueva clave desde dicho Almacén automáticamente. Tenga en cuenta que puede hacer que la aplicación lea la clave cada vez que la necesite, o bien puede almacenarla en la memoria caché y, si se produce un error al usarla, se recuperaría de nuevo desde el Almacén de claves de Azure.

El uso del Almacén de claves de Azure también agrega otro nivel de seguridad a las claves de almacenamiento. Si utiliza este método, nunca tendrá la clave de almacenamiento codificada de manera rígida en un archivo de configuración, lo que impide que alguien obtenga acceso a las claves sin permiso específico.

Otra ventaja de usar el Almacén de claves de Azure es que también puede controlar el acceso a las claves con Azure Active Directory. Esto significa que puede conceder acceso a la serie de aplicaciones que necesitan recuperar las claves del Almacén de claves de Azure, y sabe que otras aplicaciones no podrán tener acceso a las claves sin concederles el permiso específicamente.

Nota: Se recomienda usar solo una de las claves en todas las aplicaciones al mismo tiempo. Si utiliza la Clave 1 en algunos lugares y la Clave 2 en otros, no podrá rotar las claves sin que alguna aplicación pierda el acceso.

####Recursos

-   [Acerca de las cuentas de almacenamiento de Azure](storage-create-storage-account.md#regenerate-storage-access-keys)

	En este artículo se ofrece una visión general de las cuentas de almacenamiento y se trata la visualización, la copia y la regeneración de las claves de acceso de almacenamiento.

-   [Referencia de API de REST de proveedor de recursos de almacenamiento de Azure](https://msdn.microsoft.com/library/mt163683.aspx)

	Este artículo contiene vínculos a artículos específicos sobre la recuperación de las claves de la cuenta de almacenamiento y su regeneración para una cuenta de Azure que usa la API de REST. Nota: Se trata de cuentas de almacenamiento de Resource Manager.

-   [Operaciones en cuentas de almacenamiento](https://msdn.microsoft.com/library/ee460790.aspx)

    Este artículo de la referencia de API de REST del administrador de servicios de almacenamiento contiene vínculos a artículos específicos sobre la recuperación y la regeneración de las claves de la cuenta de almacenamiento mediante la API de REST. Nota: Esto se dirige a las cuentas de almacenamiento estándar.

-   [Say goodbye to key management – manage access to Azure Storage data using Azure AD (Adiós a administración de claves: administre el acceso a los datos de Almacenamiento de Azure con Azure AD)](http://www.dushyantgill.com/blog/2015/04/26/say-goodbye-to-key-management-manage-access-to-azure-storage-data-using-azure-ad/)

	Este artículo muestra cómo utilizar Active Directory para controlar el acceso a las claves de Almacenamiento de Azure en el Almacén de claves de Azure. También muestra cómo utilizar un trabajo de Automatización de Azure para regenerar las claves cada hora.

##Seguridad en el plano de los datos

La seguridad en el plano de los datos hace referencia a los métodos utilizados para proteger los objetos de datos almacenados en Almacenamiento de Azure: blobs, colas, tablas y archivos. Hemos visto métodos para cifrar los datos y la seguridad durante el tránsito de los datos, pero ¿qué hay sobre permitir el acceso a los objetos?

Básicamente, existen dos métodos para controlar el acceso a los propios objetos de datos. El primero es controlar el acceso a las claves de la cuenta de almacenamiento, y el segundo es usar Firmas de acceso compartido para conceder acceso a objetos de datos específicos durante un período de tiempo determinado.

Debe observar una excepción: puede permitir el acceso público a los blobs estableciendo el nivel de acceso para el contenedor que almacena los blobs en consecuencia. Si configura el acceso para un contenedor en Blob o Contenedor, permitirá el acceso de lectura público a los blobs en ese contenedor. Esto significa que cualquier usuario que tenga una dirección URL que apunte a un blob de ese contenedor podrá abrirlo en un explorador sin necesidad de una Firma de acceso compartido o las claves de la cuenta de almacenamiento.

###Claves de la cuentas de almacenamiento

Las claves de la cuenta de almacenamiento son cadenas de 512 bits creadas por Azure que, junto con el nombre de la cuenta de almacenamiento, pueden utilizarse para tener acceso a los objetos de datos almacenados en la cuenta de almacenamiento.

Por ejemplo, puede leer blobs, escribir en colas, crear tablas y modificar los archivos. Muchas de estas acciones pueden realizarse a través del Portal de Azure, o usando una de las muchas aplicaciones de Explorador de almacenamiento. También puede escribir código para usar la API de REST o una de las bibliotecas de cliente de almacenamiento para realizar estas operaciones.

Como se describe en la sección sobre la [Seguridad en el plano de la administración](#management-plane-security), el acceso a las claves de almacenamiento de una cuenta de almacenamiento clásica se puede conceder proporcionando acceso completo a la suscripción de Azure. El acceso a las claves de almacenamiento para una cuenta de almacenamiento mediante el modelo de Azure Resource Manager puede controlarse mediante el control de acceso basado en rol.

###Delegación del acceso a objetos en la cuenta mediante Firmas de acceso compartido y Directivas de acceso almacenadas

Una Firma de acceso compartido es una cadena que contiene un token de seguridad que puede asociarse a un URI que le permite delegar el acceso a objetos de almacenamiento y especificar restricciones, como permisos o un intervalo de fecha y hora para el acceso.

Puede conceder acceso a blobs, contenedores, mensajes de colas, archivos y tablas. En el caso de las tablas, realmente puede conceder permiso para tener acceso a un intervalo de entidades en la tabla especificando los intervalos de clave de fila y partición a los que desea que el usuario tenga acceso. Por ejemplo, si tiene datos almacenados con una clave de partición por estados geográficos, podría dar a una persona acceso a los datos de California.

En otro ejemplo, podría proporcionar un token de Firma de acceso compartido a una aplicación web que le permitiera escribir entradas en una cola, y proporcionar otro token de Firma de acceso compartido a una aplicación de rol de trabajo para que obtenga mensajes de la cola y los procese. O bien, podría dar a un cliente un token de Firma de acceso compartido que pueda utilizar para cargar imágenes a un contenedor en Almacenamiento de blobs y conceder permiso a una aplicación web para que lea esas imágenes. En ambos casos, se trata de procesos independientes: es posible dar a cada aplicación solo el acceso que requiere para realizar sus tareas. Esto es posible mediante el uso de Firmas de acceso compartido.

####Razones para usar las Firmas de acceso compartido

¿Por qué desearía utilizar una Firma de acceso compartido en lugar de simplemente proporcionar la clave de la cuenta de almacenamiento, que es mucho más fácil? Si comparte la clave de su cuenta de almacenamiento es como si prestara a alguien las llaves de su reino de almacenamiento. Con ella, es posible acceder a todo el contenido. Cualquiera podría usar sus claves y cargar toda su biblioteca de música en la cuenta de almacenamiento. También podría reemplazar sus archivos por versiones infectadas o robar sus datos. Conceder acceso ilimitado a su cuenta de almacenamiento es algo que no debe hacerse a la ligera.

Con las Firmas de acceso compartido, es posible conceder a un cliente únicamente los permisos que necesite durante un período de tiempo limitado. Por ejemplo, si alguien está cargando un blob a su cuenta, puede concederle acceso de escritura durante un tiempo suficiente que le permita cargar el blob (dependerá del tamaño del blob, por supuesto). Y si cambia de opinión, puede revocar el acceso.

Además, puede especificar que las solicitudes realizadas mediante una Firma de acceso compartido se limiten a una cierta dirección IP o un intervalo de direcciones IP externas a Azure. También puede requerir que las solicitudes se realicen mediante un protocolo específico (HTTPS o HTTP/HTTPS). Esto significa que si solo desea permitir el tráfico HTTPS, puede establecer que el protocolo requerido sea solo HTTPS y se bloqueará el tráfico HTTP.

####Definición de una Firma de acceso compartido

Una Firma de acceso compartido es un conjunto de parámetros de consulta anexo a la dirección URL que señala al recurso

que proporciona información sobre el acceso permitido y el intervalo de tiempo durante el que se permite el acceso. Aquí presentamos un ejemplo: este URI proporciona acceso de lectura a un blob durante cinco minutos. Tenga en cuenta que los parámetros de consulta de la Firma de acceso compartido deben presentar codificación URL; por ejemplo %3A para los dos puntos (:) o %20 para un espacio.

	http://mystorage.blob.core.windows.net/mycontainer/myblob.txt (URL to the blob)
	?sv=2015-04-05 (storage service version)
	&st=2015-12-10T22%3A18%3A26Z (start time, in UTC time and URL encoded)
	&se=2015-12-10T22%3A23%3A26Z (end time, in UTC time and URL encoded)
	&sr=b (resource is a blob)
	&sp=r (read access)
	&sip=168.1.5.60-168.1.5.70 (requests can only come from this range of IP addresses)
	&spr=https (only allow HTTPS requests)
	&sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D (signature used for the authentication of the SAS)

####Autenticación de la Firma de acceso compartido por el servicio Almacenamiento de Azure

Cuando el servicio de almacenamiento recibe la solicitud, toma los parámetros de consulta de entrada y crea una firma utilizando el mismo método que el programa que realiza la llamada. A continuación, compara las dos firmas. Si coinciden, el servicio de almacenamiento puede comprobar la versión del servicio de almacenamiento para asegurarse de que es válida, comprobar que la fecha y la hora actuales están dentro del período especificado, asegurarse de que el acceso solicitado corresponde a la solicitud realizada, etc.

Por ejemplo, con la dirección URL anterior, si la dirección URL apuntase a un archivo en lugar de a un blob, esta solicitud produciría un error porque especifica que la Firma de acceso compartido es para un blob. Si el comando REST al que se llama fuera para actualizar un blob, se produciría un error porque la Firma de acceso compartido especifica que se permite acceso de solo lectura.

####Tipos de Firmas de acceso compartido

-	Es posible utilizar una Firma de acceso compartido a nivel de servicio para tener acceso a recursos específicos de una cuenta de almacenamiento. Por ejemplo, para recuperar una lista de los blobs de un contenedor, descargar un blob, actualizar una entidad en una tabla, agregar mensajes a una cola o cargar un archivo en un recurso compartido de archivos.

-	Una Firma de acceso compartido a nivel de cuenta se puede utilizar para tener acceso a lo mismo para lo que se puede usar una Firma de acceso compartido a nivel de servicio. Además, puede dar opciones a los recursos que no se permiten con una Firma de acceso compartido a nivel de servicio, como la posibilidad de crear contenedores, tablas, colas y recursos compartidos de archivos. También puede especificar el acceso a varios servicios a la vez. Por ejemplo, puede conceder a alguien acceso tanto a blobs como a archivos en su cuenta de almacenamiento.

####Creación de un URI de Firma de acceso compartido

1.  Puede crear un URI ad hoc a petición, definiendo todos los parámetros de consulta cada vez.

    Esta opción es realmente flexible, pero si tiene un conjunto lógico de parámetros que se parecen cada vez, es mejor utilizar una Directiva de acceso almacenada.

2.  Puede crear una Directiva de acceso almacenada para todo un contenedor, el recurso compartido de archivos, la tabla o la cola. A continuación, puede utilizar esto como base para los URI de Firma de acceso compartido que cree. Los permisos basados en Directivas de acceso almacenadas se pueden revocar fácilmente. Puede tener hasta 5 directivas definidas en cada contenedor, cola, tabla o recurso compartido de archivos.

    Por ejemplo, si en su caso muchas personas fueran a leer los blobs de un contenedor determinado, podría crear una Directiva de acceso almacenada para "conceder acceso de lectura" y cualquier otro valor que se repita cada vez. A continuación, puede crear un URI de Firma de acceso compartido mediante la configuración de la Directiva de acceso almacenada y la especificación de la fecha y hora de expiración. Esto presenta la ventaja de que no es necesario que especifique todos los parámetros de consulta cada vez.

####Revocación

Supongamos que se ha puesto en riesgo su Firma de acceso compartido o que desea cambiarla porque así se lo exigen los requisitos de seguridad de la empresa o el cumplimiento normativo. ¿Cómo se podría revocar el acceso a un recurso que use esa Firma de acceso compartido? Depende de cómo haya creado el URI de la Firma de acceso compartido.

Si utiliza un URI ad hoc, tiene tres opciones. Puede emitir tokens de Firmas de acceso compartido con directivas de expiración breves y simplemente esperar a que las firmas expiren. Puede cambiar el nombre del recurso o eliminarlo (suponiendo que el token estuviera limitado a un único objeto). Puede cambiar las claves de la cuenta de almacenamiento. Esta última opción puede conllevar importantes repercusiones, dependiendo de cuántos servicios estén utilizando esa cuenta de almacenamiento, y no suele ser una buena idea hacerlo sin planificarlo con antelación.

Si está utilizando una Firma de acceso compartido derivada de una directiva de acceso almacenada, puede quitar el acceso revocando la Directiva de acceso almacenada (puede cambiar solo lo que ya ha expirado o bien quitarlo todo en su conjunto). Esto surte efecto inmediatamente e invalida todas las Firmas de acceso compartido creadas utilizando esa Directiva de acceso almacenada. El hecho de actualizar o quitar la Directiva de acceso almacenada puede repercutir en los usuarios que accedan específicamente a ese contenedor, recurso compartido de archivos, tabla o cola a través de la Firma de acceso compartido, pero si se han escrito los clientes de manera que soliciten una nueva Firma de acceso compartido cuando la anterior deje de ser válida, esto funcionará correctamente..

Dado que el uso de una Firma de acceso compartido derivada de una Directiva de acceso almacenada ofrece la posibilidad de revocar esa firma de acceso compartido de inmediato, se recomienda usar siempre las Directivas de acceso almacenadas cuando sea posible.

####Recursos

Para más información sobre el uso de Firmas de acceso compartido y Directivas de acceso almacenadas, junto con ejemplos, consulte los artículos siguientes:

-   Estos son los artículos de referencia.

	-	[Ejemplos SAS del servicio.](https://msdn.microsoft.com/library/dn140256.aspx)

		Este artículo proporciona ejemplos de cómo utilizar una SAS de nivel de servicio con blobs, mensajes de cola, intervalos de tabla y archivos.

	-	[Creación de una SAS de servicio](https://msdn.microsoft.com/library/dn140255.aspx)

	-	[Creación de una SAS de cuenta](https://msdn.microsoft.com/library/mt584140.aspx)

-   Se trata de tutoriales para usar la biblioteca de cliente .NET a fin de crear Firmas de acceso compartido y Directivas de acceso almacenadas.

    -	[Firmas de acceso compartido, Parte 1: Descripción del modelo de firmas de acceso compartido](storage-dotnet-shared-access-signature-part-1.md)

        Este artículo incluye una explicación del modelo de SAS, ejemplos de SAS y recomendaciones para el mejor uso práctico de SAS. También trata la revocación de los permisos concedidos.

    -	[Firmas de acceso compartido, Parte 2: Creación y uso de una firma de acceso compartido con el servicio BLOB](storage-dotnet-shared-access-signature-part-2.md)

        Este artículo muestra cómo generar URI de SAS mediante la biblioteca de cliente de almacenamiento de .NET.

-   Limitación del acceso por dirección IP (ACL de IP)

    -	[¿Qué es una lista de control de acceso (ACL) de extremo?](../virtual-network/virtual-networks-acl.md)

    -	[Creación de una SAS de servicio](https://msdn.microsoft.com/library/azure/dn140255.aspx)

		Este es el artículo de referencia para SAS de nivel de servicio; incluye un ejemplo de creación de ACL de IP.

	-	[Creación de una SAS de cuenta](https://msdn.microsoft.com/library/azure/mt584140.aspx)

    	Este es el artículo de referencia para SAS de nivel de cuenta; incluye un ejemplo de creación de ACL de IP.

-   Autenticación

	-    [Autenticación para los servicios de almacenamiento de Azure](https://msdn.microsoft.com/library/azure/dd179428.aspx)

-   Tutorial de introducción a las Firmas de acceso compartido

	-	[Getting Started with Shared Access Signatures (SAS) (Introducción a las Firmas de acceso compartido)](https://github.com/Azure-Samples/storage-dotnet-sas-getting-started)

##Cifrado en tránsito

###Cifrado de nivel de transporte – Uso de HTTPS

Otro paso que debe seguir para garantizar la seguridad de los datos de Almacenamiento de Azure es cifrar los datos entre el cliente y Almacenamiento de Azure. La primera recomendación es utilizar siempre el protocolo [HTTPS](https://en.wikipedia.org/wiki/HTTPS), que garantiza una comunicación segura a través de la red pública de Internet.

Siempre debe usar HTTPS al llamar a las API de REST u obtener acceso a objetos de almacenamiento. Además, las **Firmas de acceso compartido**, que pueden utilizarse para delegar el acceso a objetos de Almacenamiento de Azure, incluyen una opción para especificar que se utilice solo el protocolo HTTPS con las Firmas de acceso compartido, lo que garantiza que cualquiera que envíe vínculos con tokens de SAS utilice el protocolo adecuado.

####Recursos

-   [Habilitación de HTTPS para una aplicación en el servicio de aplicaciones de Azure](../app-service-web/web-sites-configure-ssl-certificate.md)

	Este artículo le muestra cómo habilitar HTTPS en una aplicación web de Azure.

###Uso del cifrado durante el tránsito con recursos compartidos de archivos de Azure

Almacenamiento de archivos de Azure admite HTTPS cuando se usa la API de REST, pero se usa con más frecuencia como un recurso compartido de archivos de SMB adjunto a una máquina virtual. SMB 2.1 no admite el cifrado, por lo que solo se permiten las conexiones dentro de la misma región de Azure. Sin embargo, SMB 3.0 admite el cifrado, y se puede utilizar con Windows Server 2012 R2, Windows 8, Windows 8.1 y Windows 10, lo que permite el acceso entre regiones e incluso el acceso en el escritorio.

Tenga en cuenta que aunque puede usar recursos compartidos de archivos de Azure con Unix, el cliente SMB de Linux aún no admite el cifrado, por lo que solo se permite el acceso dentro de una misma región de Azure. La compatibilidad con el cifrado para Linux está en la hoja de ruta de los desarrolladores de Linux responsables de la funcionalidad SMB. Cuando se agregue el cifrado, podrá tener acceso a un recurso compartido de archivos de Azure en Linux al igual que hace en Windows.

####Recursos

-   [Uso del almacenamiento de archivos de Azure con Linux](storage-how-to-use-files-linux.md)

    Este artículo muestra cómo montar un recurso compartido de archivos de Azure en un sistema Linux y cargar/descargar archivos.

-   [Introducción a Almacenamiento de archivos de Azure en Windows](storage-dotnet-how-to-use-files.md)

	Este artículo ofrece una visión general de los recursos compartidos de archivos de Azure y cómo montarlos y usarlos con PowerShell y. NET.

-   [Almacenamiento de archivos dentro de Azure](https://azure.microsoft.com/blog/inside-azure-file-storage/)

    En este artículo se anuncia la disponibilidad general de Almacenamiento de archivos de Azure y se proporcionan detalles técnicos sobre el cifrado SMB 3.0.

###Uso del cifrado de cliente para proteger los datos que envía al almacenamiento

Otra opción que le ayuda a garantizar que sus datos están protegidos mientras se transfieren entre una aplicación cliente y el servicio Almacenamiento es el Cifrado de cliente. Los datos se cifran antes de ser transferidos a Almacenamiento de Azure. En el proceso de recuperación de los datos de Almacenamiento de Azure, estos se descifran una vez recibidos en el cliente. Aunque los datos se cifran al pasar por el cable, se recomienda que también se utilice HTTPS, ya sus comprobaciones incorporadas de la integridad de los datos ayudan a mitigar los errores de red que pueden incidir en esta.

El Cifrado de cliente es también un método para cifrar los datos en reposo, ya que los datos se almacenan en su forma cifrada. Hablaremos sobre esto con más detalle en la sección sobre [Cifrado en reposo](#encryption-at-rest).

##Cifrado en reposo

Hay tres características de Azure que proporcionan cifrado en reposo. El Cifrado de discos de Azure se utiliza para cifrar los discos de datos y del sistema operativo en máquinas virtuales de IaaS. Los otros dos modos de cifrado, Cifrado en el cliente y Cifrado del servicio de almacenamiento, se utilizan para cifrar los datos en Almacenamiento de Azure. Vamos a analizarlas por separado y luego las vamos a comparar para determinar cuándo se puede utilizar cada una de ellas.

Aunque puede usar el Cifrado de cliente para cifrar los datos en tránsito (que también se almacenan en su forma cifrada en Almacenamiento), es preferible simplemente usar HTTPS durante la transferencia y disponer de algún método para que los datos se cifren automáticamente al almacenarse. Hay dos maneras de hacerlo: Cifrado de discos de Azure y SSE. Uno se utiliza para cifrar directamente los datos en los discos de datos y del sistema operativo usados por las máquinas virtuales, y el otro para cifrar los datos escritos en Almacenamiento de blobs de Azure.

###Cifrado del servicio de almacenamiento (SSE)

SSE es una nueva característica de Almacenamiento de Azure en versión preliminar pública. Esta característica le permite solicitar que el servicio de almacenamiento cifre automáticamente los datos al escribirlos en Almacenamiento de Azure. Cuando se lean los datos desde el Almacenamiento de Azure, el servicio de almacenamiento los descifrará antes de devolverlos. De este modo, protege los datos sin tener que modificar el código o agregar código a las aplicaciones.

Se trata de una configuración que se aplica a toda la cuenta de almacenamiento. Puede habilitar y deshabilitar esta característica cambiando el valor de la configuración. Para ello, puede usar el Portal de Azure, PowerShell, la CLI de Azure, la API de REST del proveedor de recursos de almacenamiento o la biblioteca de cliente de almacenamiento de .NET. De forma predeterminada, SSE está desactivado.

En este momento, Microsoft administra las claves utilizadas para el cifrado. Generamos las claves originalmente y administramos el almacenamiento seguro de las claves, así como la rotación periódica de acuerdo con la política interna de Microsoft. En el futuro, tendrá la posibilidad de administrar sus propias claves de cifrado, y proporcionaremos una ruta de migración desde las claves administradas por Microsoft a las claves administradas por el cliente.

Esta característica está disponible para cuentas de Almacenamiento de tipo Estándar y Premium creadas mediante el modelo de implementación de Resource Manager después del 30/3/2016 a las 12:00 a.m. (hora estándar del Pacífico). SSE se aplica solo a blobs en bloques, blobs en páginas y blobs en anexos. Los otros tipos de datos, como tablas, colas y archivos, no se cifrarán.

Solo se cifran los datos cuando está habilitado SSE y los datos se escriben en Almacenamiento de blobs. El hecho de habilitar o deshabilitar el SSE no afecta a los datos existentes. En otras palabras, al habilitar este cifrado, no se cifrarán los datos que ya existan; y tampoco se descifrarán los datos que ya existan al deshabilitarlo.

Si desea probar esta función con una cuenta de almacenamiento creada con anterioridad a la fecha mencionada previamente, o una cuenta de almacenamiento clásica, puede crear una nueva cuenta de almacenamiento y usar AzCopy para copiar los datos a la nueva cuenta. Esto no debería ser necesario en versiones posteriores a la preliminar.

Al igual que con la mayoría de las versiones preliminares, no es recomendable usarla en producción hasta que la característica esté disponible con carácter general.

###Cifrado de cliente

Hemos mencionado el Cifrado de cliente al hablar sobre el cifrado de los datos en tránsito. Esta característica le permite cifrar mediante programación los datos en una aplicación cliente antes de enviarlos a través del cable para que se escriban en el Almacenamiento de Azure, y descifrar mediante programación los datos después de recuperarlos de Almacenamiento de Azure.

Proporciona cifrado en tránsito, pero además incluye la característica de cifrado en reposo. Tenga en cuenta que aunque los datos se cifran en tránsito, sigue siendo recomendable usar HTTPS para aprovechar las comprobaciones incorporadas de integridad de los datos que ayudan a mitigar los errores de red que pueden incidir en esta.

Por ejemplo, pongamos que tiene una aplicación web que almacena blobs y recupera blobs, y desea que la aplicación y los datos sean lo más seguros posibles. En ese caso, utilizaría el cifrado de cliente. El tráfico entre el cliente y el servicio BLOB de Azure contiene el recurso cifrado, y nadie puede interpretar los datos en tránsito y reconstituirlos en los blobs privados.

El cifrado de cliente se integra en Java y las bibliotecas de cliente de almacenamiento de .NET, que a su vez utilizan las API de Almacén de claves de Azure, por lo que es bastante fácil de implementar. El proceso de cifrado y descifrado de los datos usa la técnica del sobre, y almacena los metadatos usados por el cifrado en cada objeto de almacenamiento. Por ejemplo, en el caso de los blobs, los almacena en los metadatos del blob, mientras que para las colas, los agrega a cada mensaje de la cola.

Para el propio cifrado, puede generar y administrar sus propias claves de cifrado. También puede usar las claves generadas por la biblioteca de cliente de Almacenamiento de Azure, o puede hacer que el Almacén de claves de Azure genere las claves. Puede almacenar las claves de cifrado en el almacenamiento de claves local o en un Almacén de claves de Azure. El Almacén de claves de Azure permite conceder acceso a los secretos del Almacén de claves de Azure a usuarios específicos mediante Azure Active Directory. Esto significa que no cualquier persona puede leer el Almacén de claves de Azure y recuperar las claves que está utilizando para el cifrado de cliente.

####Recursos

-   [Cifrado y descifrado de blobs en Almacenamiento de Microsoft Azure con Almacén de claves de Azure](storage-encrypt-decrypt-blobs-key-vault.md)

    Este artículo muestra cómo utilizar el cifrado de cliente con el Almacén de claves de Azure, incluido cómo crear la KEK y almacenarlo en el almacén con PowerShell.

-   [Cifrado del lado de cliente y Almacén de claves de Azure para el Almacenamiento de Microsoft Azure](storage-client-side-encryption.md)

    Este artículo ofrece una explicación del cifrado de cliente y proporciona ejemplos de uso de la biblioteca de cliente de almacenamiento para cifrar y descifrar los recursos de los cuatro servicios de almacenamiento. También trata el Almacén de claves de Azure.

###Uso del Cifrado de discos de Azure para cifrar discos usados por las máquinas virtuales

El Cifrado de discos de Azure es una nueva característica que está actualmente en su versión preliminar. Esta característica le permite cifrar los discos de datos y del sistema operativo usados por una máquina virtual de IaaS. Para Windows, las unidades se cifran mediante la tecnología de cifrado de BitLocker estándar del sector. Para Linux, los discos se cifran mediante la tecnología DM-Crypt. Se integra con el Almacén de claves de Azure para permitirle controlar y administrar las claves de cifrado del disco.

La solución Cifrado de discos de Azure admite los tres siguientes escenarios de cifrado de cliente:

-   Habilitar el cifrado en nuevas máquinas virtuales de IaaS creadas a partir de archivos VHD cifrados por el cliente y claves de cifrado proporcionadas por el cliente, que se almacenan en el Almacén de claves de Azure.

-   Habilitar el cifrado en nuevas máquinas virtuales de IaaS creadas en Azure Marketplace.

-   Habilitar el cifrado en máquinas virtuales de IaaS existentes que ya se ejecutan en Azure.

>[AZURE.NOTE] Para máquinas virtuales Linux que ya se ejecuten en Azure, o nuevas máquinas virtuales Linux creadas a partir de imágenes en Azure Marketplace, actualmente no se admite el cifrado del disco del sistema operativo. El cifrado del volumen del sistema operativo en máquinas virtuales Linux solo se admite para máquinas virtuales que se cifraron localmente y se cargaron en Azure. Esta restricción se aplica solo al disco del sistema operativo; se admite el cifrado de volúmenes de datos en una máquina virtual Linux.

La solución admite lo siguiente para las máquinas virtuales IaaS para la versión de la vista previa pública cuando se habilita en Microsoft Azure:

-   Integración con el Almacén de claves de Azure

-   [Máquinas virtuales IaaS de las series A, D y G](https://azure.microsoft.com/pricing/details/virtual-machines/) estándares

-   Habilitar el cifrado en máquinas virtuales de IaaS creadas con el modelo del [Administrador de recursos de Azure](../resource-group-overview.md)

-   Todas las [regiones](https://azure.microsoft.com/regions/) públicas de Azure

Esta característica garantiza que todos los datos de los discos de máquinas virtuales se cifran en reposo en el Almacenamiento de Azure.

####Recursos

-   [Azure Disk Encryption for Windows and Linux Azure Virtual Machines (Cifrado de discos de Azure para máquinas virtuales IaaS de Linux y Windows)](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

    En este artículo se describe la versión preliminar del Cifrado de discos de Azure y se proporciona un vínculo para descargar las notas del producto.

###Comparación entre Cifrado de discos de Azure, SSE y Cifrado en el cliente

####Máquinas virtuales de IaaS y sus archivos VHD

Para los discos usados por las máquinas virtuales de IaaS, se recomienda utilizar Cifrado de discos de Azure. Puede activar SSE para cifrar los archivos VHD que se usan para respaldar esos discos en Almacenamiento de Azure, pero solos se cifran los datos recién escritos. Esto significa que si crea una máquina virtual y luego habilita SSE en la cuenta de almacenamiento que contiene el archivo VHD, se cifrarán solo los cambios, no el archivo VHD original.

Si crea una máquina virtual mediante una imagen de Azure Marketplace, Azure realiza una [copia superficial](https://en.wikipedia.org/wiki/Object_copying) de ella en la cuenta de almacenamiento en Almacenamiento de Azure y no se cifra aunque tenga habilitado SSE. Cuando se cree la máquina virtual y comience a actualizarse la imagen, SSE iniciará el cifrado de los datos. Por este motivo, es mejor utilizar el Cifrado de discos de Azure en máquinas virtuales creadas a partir de imágenes en Azure Marketplace si quiere que estén totalmente cifradas.

Si trae una máquina virtual previamente cifrada a Azure desde el entorno local, podrá cargar las claves de cifrado en el Almacén de claves de Azure y mantener el cifrado para esa máquina virtual que se utilizaba de forma local. La configuración del Cifrado de discos de Azure le permite controlar este escenario.

Si dispone de un VHD no cifrado desde el entorno local, puede cargarlo en la galería como una imagen personalizada y aprovisionar una máquina virtual a partir del mismo. Si lo hace con las plantillas de Resource Manager, puede solicitar la activación del Cifrado de discos de Azure al arrancar la máquina virtual.

Al agregar un disco de datos y montarlo en la máquina virtual, puede activar el Cifrado de discos de Azure en ese disco de datos. Cifrará primero ese disco de datos localmente y, a continuación, la capa de administración del servicio hará una escritura diferida en el almacenamiento para que se cifre el contenido del mismo.

####Cifrado de cliente####

El Cifrado de cliente es el método más seguro para cifrar los datos, porque estos se cifran antes del tránsito y en reposo. Sin embargo, requiere que agregue código a sus aplicaciones mediante el almacenamiento, y quizá no desee hacerlo. En esos casos, puede utilizar HTTPS para los datos en tránsito y SSE para cifrar los datos en reposo.

Con el cifrado de cliente, puede cifrar las entidades de tabla, los mensajes de colas y los blobs. Con SSE, solo se pueden cifrar los blobs. Si necesita cifrar los datos de tabla y cola, debe utilizar el Cifrado de cliente.

El Cifrado de cliente es administrado completamente por la aplicación. Es el método más seguro, pero es necesario realizar cambios mediante programación en la aplicación e implementar procesos de administración de claves. Es recomendable usarlo si desea una seguridad adicional durante el tránsito y desea que los datos almacenados se cifren.

El Cifrado de cliente supone más carga en el cliente; así pues, debe tener en cuenta este factor en los planes de escalabilidad, especialmente si está cifrando y transfiriendo una gran cantidad de datos.

####Cifrado del servicio de almacenamiento (SSE)

SSE es administrado mediante Almacenamiento de Azure y es fácil de administrar. El uso de SSE no proporciona seguridad para los datos en tránsito, pero cifra los datos a medida que se escriben en Almacenamiento de Azure. El uso de esta característica no repercute sobre el rendimiento en modo alguno.

Solo puede cifrar blobs en bloques, blobs en anexos y blobs en páginas mediante SSE. Si necesita cifrar datos en tabla o en cola, considere el uso del Cifrado de cliente.

Si tiene un archivo o una biblioteca de archivos VHD que utiliza como base para la creación de nuevas máquinas virtuales, puede crear una nueva cuenta de almacenamiento, habilitar SSE y luego cargar los archivos VHD en esa cuenta. Esos archivos VHD se cifrarán con el Almacenamiento de Azure.

Si tiene habilitados el Cifrado de discos de Azure para los discos de una máquina virtual y SSE en la cuenta de almacenamiento que contiene los archivos VHD, el proceso funcionará correctamente y dará como resultado que todos los datos recién escritos se cifren dos veces.

##Análisis de almacenamiento

###Uso de Análisis de almacenamiento para supervisar el tipo de autorización

Para cada cuenta de almacenamiento, puede habilitar el Análisis de almacenamiento de Azure para realizar el registro y almacenar datos de métricas. Se trata de una excelente herramienta que puede usar para comprobar las métricas de rendimiento de una cuenta de almacenamiento o cuando necesite solucionar los problemas de rendimiento de una cuenta de almacenamiento.

Los registros de análisis de almacenamiento contienen también el método de autenticación utilizado por un usuario al acceder al almacenamiento. Por ejemplo, con el Almacenamiento de blobs, puede ver si los usuarios utilizan una Firma de acceso compartido o las claves de la cuenta de almacenamiento, o si el blob es de acceso público.

Esto puede resultar muy útil si se está esforzando por proteger el acceso al almacenamiento. Por ejemplo, en el Almacenamiento de blobs puede configurar todos los contenedores para que sean privados e implementar el uso de un servicio de Firma de acceso compartido en sus aplicaciones. Luego puede comprobar los registros periódicamente para ver si se ha accedido a los blobs mediante las claves de la cuenta de almacenamiento, lo que podría indicar una infracción de seguridad, o si los blobs son públicos pero no deberían serlo.

####¿Qué aspecto tienen los registros?

Después de habilitar las métricas de la cuenta de almacenamiento y el registro a través del Portal de Azure, los datos del análisis empezarán a acumularse rápidamente. El registro y las métricas de cada servicio son independientes; el registro solo se escribe cuando hay actividad en esa cuenta de almacenamiento, mientras que las métricas se registrarán cada minuto, cada hora o cada día, dependiendo de la configuración.

Los registros se almacenan en blobs en bloques en un contenedor denominado $logs en la cuenta de almacenamiento. Este contenedor se crea automáticamente cuando se habilita el Análisis de almacenamiento. Una vez creado este contenedor, no se puede eliminar, aunque sí puede eliminar su contenido.

En el contenedor $logs, hay una carpeta para cada servicio y luego hay subcarpetas para año/mes/día/hora. En la hora, simplemente se numeran los registros. Este es el aspecto que tendrá la estructura de directorios:

![Vista de archivos de registro](./media/storage-security-guide/image1.png)

Se registra cada solicitud para el Almacenamiento de Azure. Esta es una captura de pantalla de un archivo de registro donde se muestran algunos de los primeros campos.

![Instantánea de un archivo de registro](./media/storage-security-guide/image2.png)

Puede ver que puede usar los registros para realizar el seguimiento de cualquier tipo de llamadas a una cuenta de almacenamiento.

####¿Para qué sirven todos estos campos?

Este es un artículo que aparece en los siguientes recursos y que proporciona la lista de los numerosos campos de los registros (explicando además para qué se utilizan). Esta es la lista de campos en orden:

![Instantánea de los campos de un archivo de registro](./media/storage-security-guide/image3.png)

Nos interesan las entradas de GetBlob y el modo de autenticación, así que nos fijaremos en las entradas con el tipo de operación "Get-Blob" y comprobaremos el estado de la solicitud (4<sup>ª</sup> columna) y el tipo de autorización (8<sup>ª</sup> columna).

Por ejemplo, en las primeras filas del listado anterior, el estado de la solicitud es "Succes" y el tipo de autorización es "authenticated". Esto significa que la solicitud se validó con la clave de la cuenta de almacenamiento.

####¿Cómo se autentican mis blobs?

Son tres los casos que nos interesan.

1.  El blob es público y se tiene acceso a él mediante una dirección URL sin Firma de acceso compartido. En este caso, el estado de la solicitud es "AnonymousSuccess" y el tipo de autorización es "anonymous".

    1\.0;2015-11-17T02:01:29.0488963Z;GetBlob;**AnonymousSuccess**;200;124;37;**anonymous**;;mystorage…

2.  El blob es privado y se utilizó con una Firma de acceso compartido. En este caso, el estado de la solicitud es "SASSuccess" y el tipo de autorización es "sas".

    1\.0;2015-11-16T18:30:05.6556115Z;GetBlob;**SASSuccess**;200;416;64;**sas**;;mystorage…

3.  El blob es privado y la clave de almacenamiento se utilizó para tener acceso a él. En este caso, el estado de la solicitud es "**Success**" y el tipo de autorización es "**authenticated**".

    1\.0;2015-11-16T18:32:24.3174537Z;GetBlob;**Success**;206;59;22;**authenticated**;mystorage…

Puede utilizar el Analizador de mensajes de Microsoft para ver y analizar estos registros. Incluye funcionalidades de búsqueda y filtrado. Por ejemplo, puede que desee buscar instancias de GetBlob para ver si efectivamente se les está dando el uso que espera; es decir, para asegurarse de que un usuario no tenga acceso la cuenta de almacenamiento incorrectamente.

####Recursos

-   [Análisis de almacenamiento](storage-analytics.md)

	Este artículo ofrece una visión general del análisis de almacenamiento y explica cómo habilitarlo.

-   [Formato del registro del análisis de almacenamiento](https://msdn.microsoft.com/library/azure/hh343259.aspx)

	Este artículo ilustra el formato del registro del análisis de almacenamiento y detalla los campos que incluye, como el tipo de autenticación, que indica el tipo de autenticación utilizado para la solicitud.

-   [Supervisión de una cuenta de almacenamiento en el Portal de Azure](storage-monitor-storage-account.md)

	Este artículo muestra cómo configurar la supervisión de las métricas y el registro para una cuenta de almacenamiento.

-   [Solución integral de problemas con los registros y métricas de Almacenamiento de Azure, AzCopy y el analizador de mensajes](storage-e2e-troubleshooting.md)

	Este artículo trata la solución de problemas mediante el Análisis de almacenamiento y muestra cómo utilizar el Analizador de mensajes de Microsoft.

-   [Guía de funcionamiento del analizador de mensajes de Microsoft](https://technet.microsoft.com/library/jj649776.aspx)

	Este artículo es la referencia para el Analizador de mensajes de Microsoft e incluye vínculos a un tutorial, el inicio rápido y el resumen de características.

##Uso compartido de recursos entre orígenes

###Acceso entre dominios de recursos

Cuando un explorador web que se ejecuta en un dominio realiza una solicitud HTTP para un recurso desde un dominio diferente, el proceso se denomina solicitud HTTP entre orígenes. Por ejemplo, una página HTML atendida desde contoso.com realiza una solicitud para un jpeg hospedado en fabrikam.blob.core.windows.net. Por motivos de seguridad, los exploradores restringen las solicitudes HTTP entre orígenes iniciadas desde scripts, como JavaScript. Esto significa que cuando el código JavaScript de una página web en contoso.com solicite jpeg en fabrikam.blob.core.windows.net, el explorador no permitirá la solicitud.

¿Qué relación guarda esto con el Almacenamiento de Azure? Si almacena recursos estáticos como archivos de datos JSON o XML en el Almacenamiento de blobs con una cuenta de almacenamiento llamada Fabrikam, el dominio de los recursos será fabrikam.blob.core.windows.net y la aplicación web de contoso.com no podrá tener acceso a ellos mediante JavaScript, ya que los dominios son diferentes. Esto se cumple también si trata de llamar a uno de los servicios de almacenamiento de Azure — como Almacenamiento de tablas— que devuelven datos JSON para que se procesen por parte del cliente JavaScript.

####Posibles soluciones

Una manera de resolver esto es asignar un dominio personalizado, como "storage.contoso.com", a fabrikam.blob.core.windows.net. El problema es que solo se puede asignar ese dominio personalizado a una cuenta de almacenamiento. ¿Qué ocurre si los recursos se almacenan en varias cuentas de almacenamiento?

Otra manera de resolver este problema es hacer que la aplicación web actúe como proxy para las llamadas de almacenamiento. Esto significa que si está cargando un archivo al Almacenamiento de blobs, la aplicación web podría escribirlo localmente y, a continuación, copiarlo en el Almacenamiento de blobs; o bien leerlo entero en la memoria y, a continuación, escribirlo en el Almacenamiento de blobs. Como alternativa, podría escribir una aplicación web dedicada (como una API web) que cargue los archivos localmente y los escriba en el Almacenamiento de blobs. En cualquier caso, debe tener en cuenta esa función al determinar las necesidades de escalabilidad.

####¿En qué sentido puede resultar útil el uso compartido de recursos entre orígenes?

Almacenamiento de Azure permite habilitar el uso compartido de recursos entre orígenes. Puede especificar los dominios que pueden tener acceso a los recursos de cada una de las cuentas de almacenamiento. Por ejemplo, en nuestro caso descrito anteriormente, podemos habilitar el uso compartido de recursos entre orígenes en la cuenta de almacenamiento fabrikam.blob.core.windows.net y configurarlo para permitir el acceso a contoso.com. A continuación, la aplicación web contoso.com puede acceder directamente a los recursos en fabrikam.blob.core.windows.net.

Hay que destacar que el uso compartido de recursos entre orígenes permite el acceso, pero no proporciona autenticación, que es necesaria para todos los accesos no públicos de los recursos de almacenamiento. Esto significa que solo puede tener acceso a blobs si son públicos o se incluye una Firma de acceso compartido que le proporcione el permiso adecuado. Las tablas, las colas y los archivos no tienen acceso público y requieren una Firma de acceso compartido.

De forma predeterminada, el uso compartido de recursos entre orígenes está deshabilitado en todos los servicios. Puede habilitarlo mediante la API de REST o la biblioteca del cliente de almacenamiento para llamar a uno de los métodos a fin de establecer las directivas del servicio. Cuando lo haga, debe incluir una regla de uso compartido de recursos entre orígenes, que está en formato XML. Aquí presentamos un ejemplo de una regla de uso compartido de recursos entre orígenes que se ha establecido mediante la operación Set Service Properties para el servicio BLOB para una cuenta de almacenamiento. Puede realizar la operación mediante la biblioteca de cliente de almacenamiento o las API de REST para Almacenamiento de Azure.

	<Cors>    
	    <CorsRule>
	        <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
	        <AllowedMethods>PUT,GET</AllowedMethods>
	        <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
	        <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
	        <MaxAgeInSeconds>200</MaxAgeInSeconds>
	    </CorsRule>
	<Cors>

Esto es lo que significa cada fila:

-   **AllowedOrigins**: señala qué dominios no coincidentes pueden solicitar y recibir datos del servicio de almacenamiento. En este caso, indica que contoso.com y fabrikam.com pueden solicitar datos desde Almacenamiento de blobs para una cuenta de almacenamiento específica. También puede configurar este valor en un carácter comodín (*) para permitir que todos los dominios accedan a las solicitudes.

-   **AllowedMethods**: se trata de la lista de métodos (verbos de solicitud HTTP) que se puede utilizar al realizar la solicitud. En este ejemplo, se permiten solo GET y PUT. Puede configurar este valor en un carácter comodín (*) para permitir que se usen todos los métodos.

-   **AllowedHeaders**: se trata de los encabezados de solicitud que puede especificar el dominio de origen al realizar la solicitud. En el ejemplo anterior, se permiten todos los encabezados de metadatos que comienzan por x-ms-meta-data, x-ms-meta-target y x-ms-meta-abc. Tenga en cuenta que el carácter comodín (*) indica que se permite cualquier encabezado que empiece con el prefijo especificado.

-   **ExposedHeaders**: indica que el explorador debe exponer los encabezados de respuesta al emisor de la solicitud. En este ejemplo, se expondrán los encabezados que empiecen por "x-ms-meta-".

-   **MaxAgeInSeconds**: el tiempo máximo que un explorador almacenará en la caché la solicitud preparatoria OPTIONS. (para más información acerca de la solicitud preparatoria, consulte el primer artículo de los que se presentan a continuación).

####Recursos

Para más información acerca del uso compartido de recursos entre orígenes y cómo habilitarlo, consulte estos recursos.

-   [Compatibilidad con Uso compartido de recursos entre orígenes (CORS) para los Servicios de almacenamiento de Azure en Azure.com](storage-cors-support.md)

	Este artículo proporciona una visión general del uso compartido de recursos entre orígenes y explica cómo establecer las reglas para los diferentes servicios de almacenamiento.

-   [Compatibilidad con Uso compartido de recursos entre orígenes (CORS) para los Servicios de almacenamiento de Azure en MSDN](https://msdn.microsoft.com/library/azure/dn535601.aspx)

	Se trata de la documentación de referencia para la compatibilidad con el uso compartido de recursos entre orígenes para los Servicios de almacenamiento de Azure. Contiene vínculos a artículos que se aplican a cada servicio de almacenamiento, se muestra un ejemplo y se explica cada elemento en el archivo del uso compartido de recursos entre orígenes.

-   [Microsoft Azure Storage: Introducing CORS (Almacenamiento de Microsoft Azure: Introducción a uso compartido de recursos entre orígenes)](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/02/03/windows-azure-storage-introducing-cors.aspx)

	Se trata de un vínculo al artículo inicial del blog en el que se anuncia el uso compartido de recursos entre orígenes y se muestra cómo utilizarlo.

##Preguntas más frecuentes acerca de la seguridad de Almacenamiento de Azure

1.  **¿Cómo se puede comprobar la integridad de los blobs que estoy transfiriendo hacia o desde Almacenamiento de Azure si no puedo usar el protocolo HTTPS?**

	Si por algún motivo debe utilizar HTTP en lugar de HTTPS y está trabajando con blobs en bloques, puede usar la comprobación de MD5 para ayudar a comprobar la integridad de los blobs que se transfieren. Esto le ayudará con la protección frente a errores de red o de la capa de transporte, pero no necesariamente con ataques de intermediarios.

	Si puede usar HTTPS, que proporciona seguridad de nivel de transporte, el uso de la comprobación de MD5 es redundante e innecesario.
	
	Para más información, consulte [Microsoft Azure Blob MD5 Overview](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/02/18/windows-azure-blob-md5-overview.aspx) (Información general de MD5 del servicio BLOB de Azure).

2.  **¿Y qué ocurre con el cumplimiento de la norma FIPS para el Gobierno de EE. UU.?**

	La norma Federal Information Processing Standard (FIPS) de EE. UU. define los algoritmos criptográficos aprobados para su uso por los sistemas informáticos del Gobierno Federal de EE. UU para la protección de los datos confidenciales. La habilitación del modo FIPS en un servidor o escritorio de Windows indica al sistema operativo que solo se deben usar los algoritmos criptográficos validados por FIPS. Si una aplicación utiliza algoritmos no compatibles, se invalidarán las aplicaciones. Con .NET Framework en su versión 4.5.2 u otra posterior, la aplicación cambia automáticamente los algoritmos criptográficos para utilizar los algoritmos conformes a FIPS cuando el equipo está en modo FIPS.

	Microsoft permite que sea cada cliente quien decida si desea habilitar el modo FIPS. Creemos que los clientes que no están sujetos a reglamentaciones gubernamentales no tienen ninguna razón para habilitar el modo FIPS de forma predeterminada.

	**Recursos**

-	[Why We’re Not Recommending “FIPS Mode” Anymore (Por qué ya no recomendamos el "modo FIPS")](http://blogs.technet.com/b/secguide/archive/2014/04/07/why-we-re-not-recommending-fips-mode-anymore.aspx)

	En este artículo de blog se proporciona una visión global de FIPS y se explica por qué no se habilita el modo FIPS de forma predeterminada.

-   [FIPS 140 Validation (Validación FIPS 140)](https://technet.microsoft.com/library/cc750357.aspx)

	Este artículo proporciona información sobre el cumplimiento de los productos y los módulos criptográficos de Microsoft con la norma FIPS para el Gobierno Federal de EE. UU.

-   [Efectos de la configuración de seguridad "Criptografía de sistema: usar algoritmos que cumplan FIPS para cifrado, firma y operaciones hash" en Windows XP y en versiones posteriores de Windows](https://support.microsoft.com/kb/811833)

	Este artículo aborda el uso del modo FIPS en equipos más antiguos de Windows.

<!---HONumber=AcomDC_0622_2016-->