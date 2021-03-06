<properties
	pageTitle="Acceso condicional de Azure en versión de vista previa para aplicaciones SaaS | Microsoft Azure"
	description="El acceso condicional en Azure AD permite la configuración de reglas de acceso de autenticación multifactor por aplicación y la capacidad de bloquear el acceso de los usuarios en una red que no es de confianza. "
	services="active-directory"
	documentationCenter=""
	authors="femila"
	manager="swadhwa"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/14/2016"
	ms.author="femila"/>

# Introducción al acceso condicional a Azure AD  

El acceso condicional a Azure Active Directory está disponible como versión preliminar pública en las aplicaciones SaaS y las conectadas de Azure AD. Gracias a esto, podrá configurar el acceso condicional en función del grupo, la ubicación y la confidencialidad de la aplicación.

Puede aplicar reglas de autenticación multifactor a todos los usuarios que están asignados a la aplicación, o bien solo a los que pertenecen a grupos de seguridad especificados. Con el acceso condicional en función de la confidencialidad de la aplicación, puede establecer las reglas de acceso de autenticación multifactor (MFA) por aplicación, de modo que puede bloquear el acceso a los usuarios que no se encuentren en una red de confianza. Los usuarios pueden excluirse del requisito de autenticación multifactor si van a tener acceso a la aplicación desde una dirección IP que se encuentre dentro de la red de la organización.

Estas funcionalidades estarán disponibles para los clientes que hayan adquirido una licencia de Azure Active Directory Premium.

## Requisitos previos de escenario
* Licencia de Azure Active Directory Premium

* Inquilino de Azure Active Directory federado o administrado

* Para los inquilinos federados, es necesario que la autenticación multifactor esté habilitada.

## Problemas conocidos de esta versión de vista previa
Esta versión preliminar se aplica a las aplicaciones SaaS federadas previamente integradas, las aplicaciones que utilizan inicio de sesión único con contraseña, las aplicaciones desarrolladas registradas y de línea de negocio y al proxy de la aplicación de Azure AD. Siguen estando habilitadas algunas aplicaciones adicionales.

## Configuración de las reglas de acceso por aplicación

En esta sección se describe cómo configurar las reglas de acceso por aplicación.

1. Inicie sesión en el Portal de Azure clásico con una cuenta que sea administrador global en Azure AD.
2. En el panel izquierdo, seleccione **Active Directory**.
3. En la pestaña Directorio, seleccione su directorio.
4. Seleccione la pestaña **Aplicaciones**.
5. Seleccione la aplicación para la que se establecerá la regla.
6. Seleccione la pestaña **Configurar**.
7. Desplácese hasta la sección de reglas de acceso. Seleccione la regla de acceso deseada.
8. Especifique los usuarios a los que se aplicará la regla.
9. Habilite la directiva; para ello, seleccione **Activado en Habilitado**.

##Descripción de las reglas de acceso

En esta sección se ofrece una descripción detallada de las reglas de acceso que se admiten en la vista previa de acceso condicional de las aplicaciones de Azure.

### Especificación de los usuarios a los que se aplican las reglas de acceso

De forma predeterminada, la directiva se aplicará a todos los usuarios que tengan acceso a la aplicación. Sin embargo, la directiva también se puede restringir a los usuarios que son miembros de los grupos de seguridad especificados. El botón **Agregar grupo** se utiliza para seleccionar uno o varios grupos en el cuadro de diálogo de selección de grupos a los que se aplicará la regla de acceso. Este cuadro de diálogo también sirve para quitar grupos seleccionados. Cuando se seleccionan las reglas que se aplicarán a los grupos, solo se aplicarán las reglas de acceso para los usuarios que pertenezcan a uno de los grupos de seguridad especificados.

Los grupos de seguridad también se pueden excluir explícitamente de la directiva si activa la opción Excepto y especifica uno o más grupos. Los usuarios que sean miembros de un grupo en la lista Excepto no estarán sujetos al requisito de la autenticación multifactor, aunque sean miembros de un grupo al que se aplica la regla de acceso. La regla de acceso que se muestra a continuación requerirá que todos los usuarios del grupo Administradores usen la autenticación multifactor para tener acceso a la aplicación.

![Configuración de reglas de acceso condicional con MFA](./media/active-directory-conditional-access/conditionalaccess-saas-apps.png)

## Reglas de acceso condicional con MFA
Si un usuario se ha configurado con la característica de autenticación multifactor por usuario, esta opción en el usuario se combinará con las reglas de autenticación multifactor de aplicación. Esto significa que un usuario que se ha configurado para la autenticación multifactor por usuario deberá realizarla aunque se haya excluido de las reglas de autenticación multifactor de aplicación. Obtenga más información sobre Multi-Factor Authentication y la configuración por usuario.

### Opciones de reglas de acceso
En la vista previa actual, se admiten las opciones siguientes:

* **Requerir autenticación multifactor**: con esta opción, los usuarios a los que se aplican las reglas de acceso deberán llevar a cabo la autenticación multifactor antes de acceder a la aplicación a la que se aplica la directiva.

* **Requerir autenticación multifactor fuera del trabajo**: con esta opción, a los usuarios que proceden de una dirección IP de confianza no se les pedirá que realicen la autenticación multifactor. Los intervalos de direcciones IP de confianza pueden configurarse en la página de configuración de Multi-Factor Authentication.

* **Bloquear acceso fuera del trabajo**: con esta opción, se bloqueará a los usuarios que no procedan de una dirección IP de confianza. Los intervalos de direcciones IP de confianza pueden configurarse en la página de configuración de Multi-Factor Authentication.

### Configuración del estado de la regla
El estado de la regla de acceso permite la activación o desactivación de las reglas. Cuando las reglas de acceso están desactivadas, el requisito de autenticación multifactor no se aplica.

### Evaluación de la regla de acceso

Cuando un usuario tiene acceso a una aplicación federada que usa OAuth 2.0, OpenID Connect, SAML o WS-Federation, se evalúan las reglas de acceso. Además, las reglas de acceso se evalúan con OAuth 2.0 y OpenID Connect cuando se usa un token de actualización para adquirir un token de acceso. Si la evaluación de directivas da error cuando se usa un token de actualización, se muestra el error invalid\_grant. Este error indica que el usuario debe volver a autenticarse en el cliente. Configuración de los servicios de federación para proporcionar autenticación multifactor

En el caso de los inquilinos federados, puede que Azure Active Directory o el servidor local de AD FS ejecute Multi-Factor Authentication (MFA).

De forma predeterminada, MFA se producirá en una página hospedada por Azure Active Directory. Para configurar la MFA local, el valor de la propiedad –SupportsMFA debe establecerse en **True** en Azure Active Directory usando el módulo de Azure AD para Windows PowerShell.

En el ejemplo siguiente se muestra cómo habilitar MFA local mediante el cmdlet [Set-MsolDomainFederationSettings cmdlet](https://msdn.microsoft.com/library/azure/dn194088.aspx) en el inquilino contoso.com:

    Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true

Además de establecer esta marca, la instancia de AD FS de inquilinos federados debe configurarse para llevar a cabo Multi-Factor Authentication. Siga las instrucciones para implementar Azure Multi-Factor Authentication en modo local.

## Artículos relacionados

- [Protección del acceso a Office 365 y otras aplicaciones conectadas a Azure Active Directory](active-directory-conditional-access.md)
- [Índice de artículos sobre la administración de aplicaciones en Azure Active Directory](active-directory-apps-index.md)

<!---HONumber=AcomDC_0713_2016-->