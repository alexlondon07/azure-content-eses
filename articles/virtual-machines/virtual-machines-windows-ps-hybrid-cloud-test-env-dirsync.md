<properties 
	pageTitle="Entorno de prueba de Office 365 DirSync | Microsoft Azure" 
	description="Aprenda a configurar un servidor de sincronización de directorios (DirSync) de Office 365 en una nube híbrida de profesionales de TI o de pruebas de desarrollo." 
	services="virtual-machines-windows" 
	documentationCenter="" 
	authors="JoeDavies-MSFT" 
	manager="timlt" 
	editor=""
	tags="azure-resource-manager"/>

<tags 
	ms.service="virtual-machines-windows" 
	ms.workload="infrastructure-services" 
	ms.tgt_pltfrm="vm-windows" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="07/19/2016" 
	ms.author="josephd"/>

# Configuración de la sincronización de directorios (DirSync) de Office 365 en una nube híbrida para pruebas
 
En este tema se le guiará en el proceso de creación de un entorno de nube híbrida para probar la sincronización de directorios (DirSync) de Office 365 con la sincronización de contraseña hospedada en Microsoft Azure. Aquí está la configuración resultante.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-dirsync/virtual-machines-windows-ps-hybrid-cloud-test-env-dirsync-ph3.png)
 
Esta configuración simula un servidor DirSync en el entorno de producción de Azure desde su ubicación en Internet. Consta de:

- Una red local simplificada (la subred de red corporativa).
- Una red virtual entre locales hospedada en Azure (TestVNET).
- Una conexión VPN de sitio a sitio.
- Una suscripción de prueba de Office 365 FastTrack.
- Un servidor DirSync que ejecute la herramienta Azure AD Connect y un controlador de dominio secundario en la red virtual TestVNET.

Esta configuración proporciona una base y un punto de partida común desde el que puede:

- Desarrollar y probar aplicaciones para Office 365 basadas en la sincronización con un dominio de Active Directory local mediante la sincronización de contraseñas.
- Realizar pruebas de esta carga de trabajo de TI basada en la nube.

Hay tres fases principales para configurar este entorno de prueba de nube híbrida:

1.	Configuración del entorno de nube híbrida para pruebas.
2.	Configuración de la versión de prueba de Office 365 FastTrack.
3.	Configuración del servidor DirSync (DS1).

Si todavía no dispone de una suscripción de Azure, puede registrarse para obtener una cuenta gratuita en la página para [probar Azure](https://azure.microsoft.com/pricing/free-trial/). Si tiene una suscripción de MSDN o de Visual Studio, consulte [Crédito mensual de Azure para suscriptores de Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

## Fase 1: configuración del entorno de nube híbrida

Utilice las instrucciones del tema [Configuración de un entorno de nube híbrida para hacer pruebas](virtual-machines-windows-ps-hybrid-cloud-test-env-base.md). Dado que este entorno de prueba no requiere la presencia del servidor APP1 en la subred de la red corporativa, no dude en cerrarlo por ahora.

Esta es su configuración actual.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-dirsync/virtual-machines-windows-ps-hybrid-cloud-test-env-dirsync-ph1.png)

> [AZURE.NOTE] Asimismo, puede configurar el [entorno de prueba de nube híbrida simulada para la fase 1](virtual-machines-windows-ps-hybrid-cloud-test-env-sim.md).

## Fase 2: configuración de la versión de prueba de Office 365 FastTrack

Para iniciar la versión de prueba de Office 365 FastTrack, necesita un nombre de compañía ficticio y una cuenta de Microsoft. Le recomendamos que use una variante del nombre de la compañía Contoso como nombre de su compañía; esta es una compañía ficticia que se usa en el contenido de ejemplo de Microsoft, pero no es obligatorio.

A continuación, regístrese para obtener una nueva cuenta de Microsoft. Vaya a **http://outlook.com** y cree una cuenta con una dirección de correo electrónico como user123@outlook.com. Regístrese para una prueba de Office 365 FastTrack con esa cuenta.

A continuación, regístrese para obtener una nueva prueba de Office 365 Enterprise E3.

1.	Inicie sesión en CLIENT1 con las credenciales de cuenta CORP\\User1.
2.	Abra Internet Explorer y vaya a **https://go.microsoft.com/fwlink/p/?LinkID=403802**.
3.	Realice paso a paso el proceso de registro de la versión de prueba de Office 365 Enterprise E3.

Cuando se le pida la **dirección de correo electrónico de empresa**, escriba su nueva cuenta de Microsoft.

Cuando deba crear una identificación, escriba el nombre de una cuenta de Office 365 inicial, el nombre de la compañía ficticia y una contraseña. Anote la dirección de correo electrónico resultante (por ejemplo, user123@contoso123.onmicrosoft.com) y la contraseña en una ubicación segura. Necesitará esta información para completar la configuración de Azure AD Connect en la fase 3.

Cuando complete estos pasos, deberá ver la página principal del portal de Office 365. En la cinta de opciones superior, haga clic en **Administrar** y, a continuación, haga clic en **Office 365**. Aparecerá la página del centro de administración de Office 365. Mantenga esta página abierta en CLIENT1.

Esta es su configuración actual.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-dirsync/virtual-machines-windows-ps-hybrid-cloud-test-env-dirsync-ph2.png)

## Fase 3: configuración del servidor DirSync (DS1)

Desde el Portal de Azure, inicie el equipo de DC2 si es necesario.

Después, cree una máquina virtual de Azure de DS1 con estos comandos en el símbolo del sistema de Azure PowerShell en el equipo local. Antes de ejecutar estos comandos, introduzca los valores de las variables y quite los caracteres < y >.

	$rgName="<your resource group name>"
	$locName="<your Azure location, such as West US>"
	$saName="<your storage account name>"
	
	$vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
	$subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
	$pip=New-AzureRMPublicIpAddress -Name DS1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
	$nic=New-AzureRMNetworkInterface -Name DS1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
	$vm=New-AzureRMVMConfig -VMName DS1 -VMSize Standard_A1
	$storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
	$cred=Get-Credential -Message "Type the name and password of the local administrator account for DS1."
	$vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName DS1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
	$vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
	$vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
	$osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DS1-TestLab-OSDisk.vhd"
	$vm=Set-AzureRMVMOSDisk -VM $vm -Name DS1-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
	New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Luego, use el Portal de Azure para conectarse a la nueva máquina virtual de DS1 con las credenciales de la cuenta de administrador local.

A continuación, configure una regla del Firewall de Windows para permitir el tráfico para probar la conectividad básica. Desde un símbolo del sistema de Windows PowerShell con nivel de administrador en DS1, ejecute estos comandos.

	Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
	ping dc2.corp.contoso.com

El comando ping debería devolver cuatro respuestas correctas desde la dirección IP 192.168.0.4.

Después, una DS1 al dominio de Active Directory CORP con estos comandos en el símbolo del sistema de Windows PowerShell.

	Add-Computer -DomainName corp.contoso.com
	Restart-Computer

Utilice la cuenta CORP\\User1 cuando se le pida que proporcione las credenciales de cuenta de dominio para el comando **Add-Computer**.

Después de reiniciar, utilice el Portal de Azure para conectarse a DS1 con la cuenta y la contraseña de CORP\\User1.

Después, instale .NET 3.5 en DS1 con este comando en el símbolo del sistema de Windows PowerShell con nivel de administrador.

	Add-WindowsFeature NET-Framework-Core

A continuación, habilite la sincronización de directorios de la versión de prueba de Office 365 FastTrack.

1.	En CLIENT1, en la página **Centro de administración de Office 365**, en el panel izquierdo, haga clic en **Usuarios** y, a continuación, en **Usuarios activos**.
2.	Para la **Sincronización de Active Directory**, haga clic en **Configurar**.
3.	En la página de configuración y administración de la sincronización de Active Directory, en el paso 3, haga clic en **Activar**.
4.	Cuando le pregunten **¿Desea activar la sincronización de Active Directory?**, haga clic en **Activar**. Después de hacer esto, verá el mensaje **Se ha activado la sincronización de Active Directory** en el paso 3.
5.	Deje la página **Configuración y administración de la sincronización de Active Directory** abierta en CLIENT1.

Después, en el símbolo del sistema de Windows PowerShell de DC1, ejecute estos comandos **uno por uno** para crear una nueva unidad organizativa denominada contoso\_users y agregue dos cuentas de usuario nuevas para Marci Kaufman y Lynda Meyer.

	New-ADOrganizationalUnit -Name contoso_users -Path "DC=corp,DC=contoso,DC=com"
	New-ADUser -SamAccountName marcik -AccountPassword (Read-Host "Set user password" -AsSecureString) -name "Marci Kaufman" -enabled $true -PasswordNeverExpires $true -ChangePasswordAtLogon $false -Path "OU=contoso_users,DC=corp,DC=contoso,DC=com"
	New-ADUser -SamAccountName lyndam -AccountPassword (Read-Host "Set user password" -AsSecureString) -name "Lynda Meyer" -enabled $true -PasswordNeverExpires $true -ChangePasswordAtLogon $false -Path "OU=contoso_users,DC=corp,DC=contoso,DC=com"

Cuando ejecute cada comando **New-ADUser** de Windows PowerShell, se le solicitará la nueva contraseña de usuario. Anote estas contraseñas y guárdelas en una ubicación segura. Las necesitará más adelante.

A continuación, instale y configure la herramienta Azure AD Connect en DS1.

1.	Inicie Internet Explorer, escriba **https://www.microsoft.com/download/details.aspx?id=47594** en la **barra de direcciones** y, después, presione ENTRAR.
2.	Ejecute el programa de instalación de Microsoft Azure AD Connect.
3.	En el escritorio, haga doble clic en **Azure AD Connect**.
4.	En la página de **Bienvenida**, seleccione **Acepto los términos de licencia y el aviso de privacidad** y, a continuación, haga clic en **Continuar**.
5.	En la página **Configuración rápida**, haga clic en **Usar configuración rápida**.
6.	En la página **Conectar a Azure AD**, escriba la dirección de correo electrónico y la contraseña de la cuenta inicial que creó al configurar la versión de prueba de Office 365 FastTrack en la fase 2. Haga clic en **Siguiente**.
7.	En la página **Conectar a AD DS**, escriba **CORP\\User1** en **Nombre de usuario** y la contraseña de la cuenta User1 en **Contraseña**. Haga clic en Siguiente.
8.	En la página **Listo para configurar**, revise la configuración y, a continuación, haga clic en **Instalar**.
9.	En la página **Completar la configuración**, haga clic en **Salir**.

A continuación, compruebe que las cuentas de usuario en el dominio CORP están sincronizadas con Office 365. Tenga en cuenta que pueden pasar unos minutos antes de que se produzca la sincronización.

En CLIENT1, en la página **Configuración y administración de la sincronización de Active Directory**, haga clic en el vínculo **usuarios** en el paso 6 de esta página. Si la sincronización de directorios se ha realizado correctamente, debería ver algo parecido a esto.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-dirsync/virtual-machines-windows-ps-hybrid-cloud-test-env-dirsync-example.png)

La columna **Estado** indica que la cuenta se obtuvo mediante la sincronización con un dominio de Active Directory.

A continuación, se muestra la sincronización de contraseñas de Office 365 con la cuenta de Active Directory de Lynda Myer.

1.	En CLIENT1, en la página **Usuarios activos**, seleccione la cuenta de **Lynda Meyer**.
2.	En las propiedades de la cuenta de Lynda Meyer, en **Licencia asignada**, haga clic en **Editar**.
3.	En la pestaña **Asignar licencias**, seleccione una ubicación en **Establecer la ubicación del usuario** (por ejemplo, Estados Unidos).
4.	Seleccione **Microsoft Office 365 Plan E3** y haga clic en **Guardar**.
5.	Cierre Internet Explorer.
6.	Inicie Internet Explorer y vaya a **http://portal.microsoftonline.com**.
7.	Inicie sesión con las credenciales de Office 365 de Lynda Meyer. Su nombre de usuario será lyndam@<*su nombre ficticio*>.onmicrosoft.com. La contraseña es la de la cuenta de usuario de Active Directory de Lynda Meyer.
8.	Tras realizar correctamente el inicio de sesión, verá la página principal del portal de Office 365 con **Marquemos hoy la diferencia**.

Esta es su configuración actual.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-dirsync/virtual-machines-windows-ps-hybrid-cloud-test-env-dirsync-ph3.png)
 
Este entorno ya está preparado para realizar pruebas de aplicaciones de Office 365 que se basen en la funcionalidad de DirSync de Office 365 o para probar la funcionalidad de DirSync y el rendimiento desde DS1.

## Paso siguiente

- Implemente esta carga de trabajo [en producción](http://technet.microsoft.com/library/dn635310.aspx).

<!---HONumber=AcomDC_0720_2016-->