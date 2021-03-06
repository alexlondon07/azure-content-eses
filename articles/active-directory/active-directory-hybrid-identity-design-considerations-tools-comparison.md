<properties
	pageTitle="Identidades híbridas: comparación de las herramientas para la integración de directorios de identidades híbridas | Microsoft Azure"
	description="Esta página proporcionará tablas completas con la comparación de las distintas herramientas que se pueden usar para la integración de directorios."
	services="active-directory"
	documentationCenter=""
	authors="billmath"
	manager="stevenpo"
	editor="curtand"/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="05/12/2016"
	ms.author="billmath"/>

# Identidades híbridas: comparación de las herramientas para la integración de directorios de identidades híbridas

A lo largo de los años, las herramientas para la integración de directorios han crecido y evolucionado. En este documento, se proporciona una vista consolidada de estas herramientas, además de una comparación de las características disponibles en cada una de ellas.

<!-- The hardcoded link is a workaround for campaign ids not working in acom links-->

>[AZURE.NOTE] Azure AD Connect incorpora los componentes y la funcionalidad publicados previamente como Dirsync y Sincronización de AAD. Estas herramientas ya no se publican de manera individual, y todas las futuras mejoras se incluirán en actualizaciones de Azure AD Connect; así, siempre sabrá dónde obtener la funcionalidad más actualizada.
>
>DirSync y Sincronización de Azure AD están en desuso. Se puede encontrar más información en [Upgrade Microsoft Azure Active Directory Sync (“DirSync”) and Azure Active Directory Sync (“Azure AD Sync”)](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-dirsync-deprecated/?WT.mc_id=DirSyncDepACOM) (Actualización de la sincronización de Microsoft Azure Active Directory (“DirSync”) y sincronización de Azure Active Directory (“Azure AD Sync”)).


Utilice la clave siguiente para cada una de las tablas.

● = Disponible ahora VF = Versión futura VP = Versión preliminar pública

## Sincronización de local a la nube

| Característica | Azure Active Directory Connect | Servicios de sincronización de Azure Active Directory (Sincronización de AAD) | Herramienta de sincronización de Azure Active Directory (DirSync)| Forefront Identity Manager 2010 R2 (FIM) |Microsoft Identity Manager 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Conexión a un bosque de AD local | ● | ● | ● | ● |● |
| Conexión a varios bosques de AD locales |● | ● | | ● |● |
| Conexión a varias organizaciones de Exchange locales | ● | | | | |
| Conexión a un directorio LDAP local | VF | | | ● |● |
| Conexión a varios directorios LDAP locales |VF | | | ● |● |
| Conexión a directorios AD y LDAP locales |VF | | | ● |● |
| Conexión a sistemas personalizados (es decir, SQL, Oracle, MySQL, etc.) | VF | | | ● |● |
| Sincronización de atributos definidos por el cliente (extensiones de directorio) | ● | | | | |
|Conexión a sistemas de recursos humanos locales (es decir, SAP, Oracle eBusiness, PeopleSoft)| VF | | | ● |● |
|Admite reglas de sincronización y conectores de FIM para el aprovisionamiento de sistemas locales.| | | | ● |● |

## Sincronización de la nube a local

| Característica | Azure Active Directory Connect | Servicios de sincronización de Azure Active Directory | Herramienta de sincronización de Azure Active Directory (DirSync) | Forefront Identity Manager 2010 R2 (FIM) |Microsoft Identity Manager 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Escritura diferida de dispositivos | ● | | ● | ||
| Escritura diferida de atributos (para la implementación híbrida de Exchange) | ● | ● | ● | ● |● |
| Escritura diferida de objetos de grupos y usuarios | ●| | | ||
| Escritura diferida de contraseñas (desde autoservicio de restablecimiento de contraseña [SSPR] y cambio de contraseña) | ● | ● | | ||



## Compatibilidad con características de autenticación

| Característica | Azure Active Directory Connect | Servicios de sincronización de Azure Active Directory | Herramienta de sincronización de Azure Active Directory (DirSync) | Forefront Identity Manager 2010 R2 (FIM) |Microsoft Identity Manager 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Sincronización de contraseñas para bosque de AD local | ● | ● | ● | ||
| Sincronización de contraseñas para varios bosques de AD locales | ●| ● | | ||
| Inicio de sesión único con federación | ● | ● | ● | ● |● |
| Escritura diferida de contraseñas (desde SSPR y cambio de contraseña) |● | ● | | ||



## Configuración e instalación

| Característica | Azure Active Directory Connect | Servicios de sincronización de Azure Active Directory | Herramienta de sincronización de Azure Active Directory (DirSync) | Microsoft Identity Manager 2016 (MIM) |
| :-------- |:--------:|:--------:|:--------:|:--------:
| Admite la instalación de un controlador de dominio | ● | ● | ● | |
| Admite la instalación con SQL Express | ● | ● | ● | |
| Actualización sencilla desde DirSync |● | | | |
| Localización de la experiencia de usuario de administración a los idiomas de Windows Server | ● | ● | ● | |
|Localización de la experiencia de usuario de usuario final a los idiomas de Windows Server| | | |● |
| Compatibilidad con Windows Server 2008 y Windows Server 2008 R2 | ● para sincronización, no para federación| ● | ● | ● |
| Compatibilidad con Windows Server 2012 y Windows Server 2012 R2 | ● | ● | ● | ● |

## Filtrado y configuración

Característica | Azure Active Directory Connect | Servicios de sincronización de Azure Active Directory | Herramienta de sincronización de Azure Active Directory (DirSync) | Forefront Identity Manager 2010 R2 (FIM)| Microsoft Identity Manager 2016 (MIM)
:-------- |:--------:|:--------:|:--------:|:--------:|:--------:|
Filtrar según dominios y unidades organizativas | ● | ● | ● | ● | ●
Filtrar según valores de atributo de los objetos | ● | ● | ● | ●| ●
Permitir sincronización de un conjunto mínimo de atributos (MinSync) | ● | ● | ||
Permitir aplicar distintas plantillas de servicio para flujos de atributo |● | ● | ||
Permitir quitar atributos del flujo de AD a Azure AD | ● | ● | | |
Permitir personalización avanzada para flujos de atributo | ● | ● | | ● | ●

## Pasos siguientes
Obtenga más información sobre la [Integración de las identidades locales con Azure Active Directory](active-directory-aadconnect.md).

<!---HONumber=AcomDC_0518_2016-->