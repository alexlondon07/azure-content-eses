<properties
	pageTitle="Compatibilidad de hardware y de plataformas de sistema operativo | Microsoft Azure"
	description="Resume la compatibilidad del SDK de dispositivo de IoT con plataformas de sistema operativo y hardware de dispositivo."
	services="iot-hub"
	documentationCenter=""
	authors="hegate"
	manager="timlt"
	editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="07/18/2016"
     ms.author="hegate"/>

# Compatibilidad de hardware y de plataformas de sistema operativo con SDK de dispositivos

Este documento describe la compatibilidad del SDK con distintas plataformas de sistema operativo, así como las configuraciones de dispositivos específicos incluidas en el [programa Microsoft Azure Certified para IoT](#microsoft-azure-certified-for-iot). Si ya tiene un dispositivo, consulte la lista de dispositivos incluidos en el programa para encontrar información de compatibilidad específica del dispositivo. Si no está seguro del dispositivo que debe usar, eche un vistazo a las sección de compatibilidad de [bibliotecas y plataformas de sistema operativo](#os-platforms).


## Plataformas de sistema operativo

Las bibliotecas de Azure IoT se han probado en las siguientes plataformas de sistema operativo:


|Plataformas de sistema operativo Linux/Unix | Versión|
|:---------------|:------------:|
|Debian Linux| 7\.5|
|Fedora Linux|20 ||
|Raspbian Linux| 3\.18 |
|Ubuntu Linux| 14\.04 |
|Yocto Linux|2\.1 |

|Plataformas de sistema operativo Windows | Versión|
|:---------------|:------------:|
|Android| > 4.2|

|Plataformas de sistema operativo Windows | Versión|
|:---------------|:------------:|
|Escritorio de Windows| 10 |
|Windows IoT Core| 10 |
|Windows Server| 2012 R2|

|Otras plataformas | Versión|
|:---------------|:------------:|
|Arduino | IDE 1.6.8 |
|mbed | 2\.0 |
|TI-RTOS | 2\.x |



## Bibliotecas C

El [SDK de dispositivos de Microsoft Azure IoT](https://github.com/Azure/azure-iot-sdks/blob/master/c/readme.md) se ha probado en las siguientes configuraciones:

|Plataforma de sistema operativo| Versión|Protocolos|
|:---------|:----------:|:----------:|
|Arduino| IDE 1.6.8 | HTTPS |
|Debian Linux| 7\.5 | HTTPS, AMQP, MQTT, AMQP sobre WebSockets |
|Fedora Linux| 20 | | HTTPS, AMQP, MQTT, AMQP sobre WebSockets |
|mbed OS| 2\.0 | HTTPS, AMQP, MQTT |
|TI-RTOS| 2\.x | HTTPS |
|Ubuntu Linux| 14\.04 | HTTPS, AMQP, MQTT, AMQP sobre WebSockets |
|Escritorio de Windows| 10 | HTTPS, AMQP, MQTT, AMQP sobre WebSockets |
|Yocto Linux|2\.1 | HTTPS, AMQP|



## Bibliotecas de Node.js

El [SDK de dispositivos de Microsoft Azure IoT para Node.js](https://github.com/Azure/azure-iot-sdks/blob/master/node/device/readme.md) se ha probado en las siguientes configuraciones:

|Tiempo de ejecución| Versión|Protocolos|
|:---------|:----------:|:----:|
|Node.js| 4\.1.0 | HTTPS, AMQP, MQTT, AMQP sobre WebSockets |

El SDK de dispositivo IoT de Microsoft Azure para Node.js se ha probado en las siguientes configuraciones:

|Tiempo de ejecución| Versión|
|:---------|:----------:|
|Node.js| 4\.1.0 |


## Bibliotecas Java

El [SDK de dispositivos de Microsoft Azure IoT para Java](https://github.com/Azure/azure-iot-sdks/blob/master/java/device/readme.md) se ha probado en las siguientes configuraciones:

|Tiempo de ejecución| Versión|Protocolos|
|:---------|:----------:|----|
|Java SE (Windows)| 1\.8 | HTTPS, AMQP, MQTT |
|Java SE (Linux)| 1\.8 | HTTPS, AMQP, MQTT|
|Android| API 15 | HTTPS y MQTT |

El SDK de servicios de Microsoft Azure IoT para Java se ha probado en las siguientes configuraciones:

|Tiempo de ejecución| Versión|Protocolos|
|:---------|:----------:|:-----|
|Java SE| 1\.8 | HTTPS, AMQP, MQTT |


## CSharp

El [SDK de dispositivos de Microsoft Azure IoT para .NET](https://github.com/Azure/azure-iot-sdks/blob/master/csharp/device/readme.md) se ha probado en las siguientes configuraciones:

|Plataforma de sistema operativo| Versión|Protocolos|
|:---------|:----------:|:----------:|
|Escritorio de Windows| 10 | HTTPS, AMQP, MQTT, AMQP sobre WebSockets |
|Windows IoT Core|10 | HTTPS |
|PCL (Xamarin, Mono, UWP, WP 8.1 y Win 8.1)| | HTTPS, AMQP, MQTT, AMQP sobre WebSockets |

El SDK de servicio IoT de Microsoft Azure para CSharp se ha probado en las siguientes configuraciones:

|Tiempo de ejecución| Versión|
|:---------|:----------:|
|.Net| 4\.5. |
|UWP| |

El código de agente administrado requiere Microsoft .NET Framework 4.5


## Bibliotecas de Python

El [SDK de dispositivo IoT de Microsoft Azure para Python](https://github.com/Azure/azure-iot-sdks/blob/master/python/device/readme.md) se ha probado en las siguientes configuraciones:

|Tiempo de ejecución| Versión|Protocolos|
|:---------|:----------:|:----------:|
|Python | 2\.7.x, 3.4.x y 3.5.x | HTTPS, AMQP, MQTT |


## Microsoft Azure Certified para IoT

**Microsoft Azure Certified para IoT** es el programa de socios comerciales que conecta el amplio ecosistema de IoT con Microsoft Azure para que los arquitectos y desarrolladores comprendan los distintos escenarios de compatibilidad. Específicamente, proporciona una lista de confianza de combinaciones de sistema operativo/dispositivo que le ayudarán a comenzar a trabajar rápidamente con un proyecto de IoT, tanto si se encuentra en fase piloto o de prueba de concepto. Con las combinaciones certificadas de dispositivo y sistemas operativos, podrá comenzar con su proyecto de IoT, con menos trabajo y con la personalización necesaria para asegurarse de que los dispositivos son compatibles con el [Conjunto de aplicaciones de IoT de Azure][lnk-iot-suite] y el Centro de IoT de Azure.

## Certificación para dispositivos de IoT

La **certificación para dispositivos de IoT** ha probado la compatibilidad con el SDK de IoT de Azure y está lista para usarse en su aplicación de IoT. En concreto, identificamos la compatibilidad según la plataforma de sistema operativo y el lenguaje de código.

¿No encuentra el dispositivo que busca en la lista? No hay problema, nuestro SDK de IoT de Azure tiene el código abierto y diseño portátil. Visite nuestro [repositorio](https://github.com/Azure/azure-iot-sdks) de GitHub de la biblioteca del cliente para comenzar y háganos saber si tiene algún problema para conectar el dispositivo en la sección [Problemas](https://github.com/Azure/azure-iot-sdks/issues).

#### Lista de dispositivos

Se ha certificado el funcionamiento de cada dispositivo con nuestro SDK en el sistema operativo y el lenguaje elegido por el fabricante del dispositivo. Por ejemplo, BeagleBone Black funciona en Debian con nuestro lenguaje C, Javascript y Java. Esto significa que los desarrolladores pueden crear aplicaciones en cualquiera de las combinaciones de lenguajes sistemas operativos en los dispositivos específicos.

|Dispositivo| Sistema operativo probado |Idioma|Instrucciones|
|:---------|:----------|:----------|:----------|
|[AAEON ACP-1104](http://www.aaeon.com/en/p/infotainment-multi-touch-panel-solutions-1104/) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-acp-1104-csharp.md)|
|[AAEON BOXER-6614](http://www.aaeon.com/en/p/fanless-embedded-computers-boxer-6614/) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows-boxer-6614-csharp.md)|
|[AAEON GENE-BT05](http://www.aaeon.com/en/p/3-and-half-inches-subcompact-boards-gene-bt05/) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows-gene-bt05-csharp.md)|
|[AAEON GENE-BT05](http://www.aaeon.com/en/p/3-and-half-inches-subcompact-boards-gene-bt05) |Ubuntu | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/ubuntu-gene-bt05-c.md)|
|[AAEON PICO-BT01](http://www.aaeon.com/en/p/pico-itx-boards-pico-bt01) |Ubuntu | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/ubuntu-PICO-BT01-c.md)|
|[AAEON PICO-BT01](http://www.aaeon.com/en/p/pico-itx-boards-pico-bt01) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows-%20pico-bt01-csharp.md)|
|[AAEON UP](http://www.up-board.org/) |ubilinux | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/ubilinux-up-board.md)|
|[AAEON UP](http://www.up-board.org/) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-aaeon-up-csharp.md)|
|[Acme Systems Arietta G25](http://www.acmesystems.it/arietta) |Debian | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/debian-arietta-g25-c.md)|
|[Adafruit Feather M0 Wifi](https://www.adafruit.com/product/3010) |IDE de Arduino (RTOS) | Arduino, C|[Primeros pasos](https://azure.microsoft.com/documentation/samples/iot-hub-c-m0wifi-getstartedkit/)|
|[Adafruit Feather Huzzah](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/overview) |IDE de Arduino (RTOS) | Arduino, C|[Primeros pasos](https://azure.microsoft.com/documentation/samples/iot-hub-c-huzzah-getstartedkit/)|
|[ADLINK IMB-M43](http://www.adlinktech.com/PD/web/PD_detail.php?cKind=&pid=1600&seq=&id=&sid=&category=Industrial-Motherboards-&-SBC_ ATX&utm\_source=#) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-imb-m43-csharp.md)|
|[ADLINK MXE-200](http://www.adlinktech.com/PD/web/PD_detail.php?cKind=&pid=1505&seq=&id=&sid=&category=Fanless-Embedded-Computer_Integrated-Embedded-Computer&utm_source=) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-mxe-200-csharp.md)|
|[ADLINK MXE-200](http://www.adlinktech.com/PD/web/PD_detail.php?cKind=&pid=1505&seq=&id=&sid=&category=Fanless-Embedded-Computer_Integrated-Embedded-Computer&utm_source=) |Windows10 Iot Core | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-iot-core-mxe-200-csharp.md)|
|[ADLINK MXE-202i](http://www.adlinktech.com/PD/web/PD_detail.php?pid=1589) |Wind River | Javascript|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/linux-mxe-202i-nodejs.md)|
|[ADLINK MXE-5400](http://www.adlinktech.com/PD/web/PD_detail.php?pid=1318) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-mxe-5400-csharp.md)|
|[ADLINK MXC-6300](http://www.adlinktech.com/PD/web/PD_detail.php?cKind=&pid=1242&seq=&id=&sid=&category=Fanless-Embedded-Computer_Expandable-Embedded-Computer&utm_source=) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-mxc-6300-csharp.md)|
|[ADLINK NuPRO-E43](http://www.adlinktech.com/PD/web/PD_detail.php?cKind=&pid=1634&seq=&id=&sid=&category=Industrial-Motherboards-&-SBC_PICMG-1.x-CPU-boards&utm_source=) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-nupro-e43-csharp.md)|
|[Advantech Co., ARK-2121L](http://www.advantech.com/products/ark-2000_series_embedded_box_pcs/ark-2121l/mod_dd092808-0832-44bc-b38a-945eb7e016bd) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows-10-iot-enterprise-ltsb-ark-2121l-csharp.md)|
|[Advantech Co., ARK-1123C](http://www.advantech.com/products/92d96fda-cdd3-409d-aae5-2e516c0f1b01/ark-1123c/mod_0b91165c-aa8c-485d-8d25-fde6f88f4873) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows-10-iot-enterprise-ltsb-ark-1123c-csharp.md)|
|[Advantech Co., UNO-1372G](http://www.advantech.com/products/gf-bvl2/uno-1372g/mod_8e63b3c9-b606-4725-a1af-94fccb98bb1a) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows-10-iot-enterprise-ltsb-uno-1372g-csharp.md)|
|[Advantech Co., UNO-1372G](http://www.advantech.tw/products/gf-bvl2/uno-1372g/mod_8e63b3c9-b606-4725-a1af-94fccb98bb1a) |Ubuntu | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/ubuntu-advantech-uno-1372g-c.md)|
|[Advantech Co., UNO-2272G](http://www.advantech.tw/products/1-2mlj9a/uno-2272g/mod_2f889619-f9ba-4735-a432-7ac7a08669c4) |Ubuntu | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/ubuntu-advantech-uno-2272g-jxae-c.md)|
|[Advantech Co., UTX-3115](http://www.advantech.com/products/bda911fe-28bc-4171-aed3-67f76f6a12c8/utx-3115/mod_fa00d5cd-7d2b-430b-8983-c232bfb9f315) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows-10-iot-enterprise-ltsb-utx-3115-csharp.md)|
|[Advantech Co., TREK-674](http://www.advantech.com/products/1-2jsj5t/trek-674/mod_88a737dd-819b-4c8e-8f2e-2bb75b04619b) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows-10-iot-enterprise-ltsb-trek-674-csharp.md)|
|[Alleantia IoT SCADA SERVER](http://www.alleantia.com/en/iot-scada-server-2) |Ubuntu | Java|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/ubuntu-IoT-scada-server-java.md)|
|[Amplified FATBOX G3](http://www.amplified.com.au/#!4gltegateway/c192n) |OpenWRT Linux | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/linux-openwrt-fatbox-g3-c.md)|
|[Arbor IEC-3300](http://www.arbor-technology.com/tw/Product/Pro/Model/IEC-3300_6) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-Arbor-IEC-3300-csharp.md)|
|[Arduino MKR1000](https://www.arduino.cc/en/Main/ArduinoMKR1000) |IDE de Arduino | Arduino, C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/arduinoide-arduino-wifi101-c.md)|
|[Arduino Zero](https://www.arduino.cc/en/Guide/ArduinoZero) |IDE de Arduino | Arduino, C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/arduinoide-arduino-wifi101-c.md)|
|[Arrow DragonBoard 410c](http://partners.arrow.com/campaigns-na/qualcomm/dragonboard-410c/) |Windows 10 IoT Core | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-iot-core-dragonboard-410c-csharp.md)|
|[Atmark Armadillo-IoT Gateway G2](http://armadillo.atmark-techno.com/armadillo-iot/specs) |Debian | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/debian-armadillo-IoT_GW_G2-c.md)|
|[Atmark Armadillo-IoT Gateway G2](http://armadillo.atmark-techno.com/armadillo-iot-g2/specs) |Debian | Java|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/linux-armadillo-iot-gateway-g2-linux-java.md)|
|[Atmark Armadillo-IoT Gateway G3](http://armadillo.atmark-techno.com/armadillo-iot-g3) |Debian | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/linux-armadillo-iot-gw-g3-c.md)|
|[Atmark Armadillo-IoT Gateway G3](http://armadillo.atmark-techno.com/armadillo-iot-g3) |Debian | Java|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/linux-armadillo-iot-gw-g3-java.md)|
|[Atmark Armadillo-IoT Gateway G3L](http://armadillo.atmark-techno.com/armadillo-iot-g3l) |Debian | Java|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/linux-armadillo-iot-gw-g3l-java.md)|
|[Atmark Armadillo-IoT Gateway G3L](http://armadillo.atmark-techno.com/armadillo-iot-g3l) |Debian | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/linux-armadillo-iot-gateway-g3l-c.md)|
|[Avalue EPC-BTCR](http://www.avalue.com.tw/Product/Embedded-Computing/Fanless-Industrial-System/Embedded-Computing-System/EPC-BTCR_2323) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/Windows10-epc-btcr-csharp.md)|
|[Avalue RIPAC-10P1](http://www.avalue.com.tw/product/Intelligent-Systems/POS-Terminal/POS-hardware-systems/RiPac-10P1_2399) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows-ripac-10p1-csharp.md)|
|[Avalue RITAB-10T1](http://www.avalue.com.tw/product/Intelligent-Systems/Mobile-Solution/Mobile-Solution/RiTab-10T1_2384) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows-ritab-10t1-csharp.md)|
|[Axiomtek ICO300](http://www.axiomtek.com/Default.aspx?MenuId=Products&FunctionId=ProductView&ItemId=1151) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows-axiomtek-ico300-csharp.md)|
|[Base Technology QuiX Sensor Gateway](https://www.quix.jp/iot/iotqsx/) |Raspbian | Python|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/raspbian-quix-sensor-gateway-python.md)|
|[BeagleBone Black](http://beagleboard.org/black) | Debian | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/debian-beaglebone-black-c.md)|
|[BeagleBone Black](http://beagleboard.org/black) | Debian | Java|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/linux-beaglebone-black-java.md)|
|[BeagleBone Black](http://beagleboard.org/black) | Debian | Javascript|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-run-sample.md)|
|[BeagleBone Green](http://beagleboard.org/green) |Debian | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/debian-beaglebone-green-c.md)|
|[BeagleBone Green](http://beagleboard.org/green) |Debian | Java|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/linux-beaglebone-green-java.md)|
|[BeagleBone Green](http://beagleboard.org/green) |Debian | Javascript|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-run-sample.md)|
|[Bitjoule Magic Box](http://www.bjbox.io/) |Raspbian | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/raspbian-bitjoule-magic-box-c.md)|
|[Bluebird BM180](http://www.mypidion.com/product/product_tab.asp?bmenu=1&t_idx=508) |Windows 10 Mobile IoT Enterprise | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-iot-mobile-enterprise-BM180-csharp.md)|
|[Bluebird BP30](http://www.mypidion.com/product/product_tab.asp?bmenu=1&t_idx=509) |Windows 10 Mobile IoT Enterprise | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-iot-mobile-enterprise-BP30-csharp.md)|
|[Bluebird EF400](http://www.mypidion.com/product/product_tab.asp?bmenu=1&t_idx=519) |Windows 10 Mobile IoT Enterprise| C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-iot-mobile-enterprise-EF400-csharp.md)|
|[Bluebird EF500](http://www.mypidion.com/product/product_tab.asp?bmenu=1&t_idx=511) |Windows 10 Mobile IoT Enterprise | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-iot-mobile-enterprise-EF500-csharp.md)|
|[Bluebird EF500R](http://www.mypidion.com/product/product_tab.asp?bmenu=1&t_idx=511) |Windows 10 Mobile IoT Enterprise | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-iot-mobile-enterprise-EF500R-csharp.md)|
|[Clientron IT800](http://www.clientron.com/en/goods.php?act=view&no=38) |Ubuntu | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/ubuntu-it800-c.md)|
|[Clientron PT6500](http://www.clientron.com/en/goods.php?act=view&no=20) |Ubuntu | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/ubuntu-PT6500-c.md)|
|[ComponentSoft RFID Tunnel](http://www.componentsoft.com/) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-rfid-tunnel-csharp.md)|
|[Contec BX-320](http://www3.contec.co.jp) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-BX-320-csharp.md)|
|[Contec BX-830](http://www3.contec.co.jp) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-BX-830-csharp.md)|
|[Contec CPS-MC341-ADSC2](http://www3.contec.co.jp/B2B/ConIWCatProductPage_B2B.process?Merchant_Id=1&Section_Id=0&Catalog_Id=0&Product_Id=2537) |Ubuntu | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/ubuntu-CPS-MC341-ADSC2-c.md)|
|[DataVan Glamor Series](http://www.datavan.com.tw/zh_hant/product_4.html) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-glamor-series-csharp.md)|
|[Dell Edge Gateway 5000 Series](http://www.dell.com/IoTgateway) |Ubuntu | Java|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-dell-edge-gateway-5000-series-java.md)|
|[DFI EC700-BT](http://www.dfi.com.tw/Upload/Product/Documents/BT700.pdf) |Windows 10 | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows7-ec700-bt-c.md)|
|[DUX Inc HFBX-100](http://dux.jp/product/hfbx-100.html) |Windows10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-HFBX-100-csharp.md)|
|[e-con Systems Almach](http://www.e-consystems.com/DM3730-development-board.asp) |Linux Yocto | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/linux-almach-c.md)|
|[e-con Systems Ankaa](http://www.e-consystems.com/iMX6-development-board.asp) |Ubuntu | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/ubuntu-ankaa-c.md)|
|[Ecomott Cloud Logger LTE](http://www.ecomott.co.jp/product/cloudloggerlte.html) |Ubuntu | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/ubuntu-cloud-logger-lte-c.md)|
|[embedded systems LogicMachine Series](http://openrb.com/products/) |Linux personalizado | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/linux-logicmachine-c.md)|
|[Fitivision FDG-1000](http://www.fitivision.com/) |Linux | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/openwrt-honeybee-c.md)|
|[Freescale FRDM K64](http://www.freescale.com/products/arm-processors/kinetis-cortex-m/k-series/k6x-ethernet-mcus/freescale-freedom-development-platform-for-kinetis-k64-k63-and-k24-mcus:FRDM-K64F) |mbed 2.0 | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/mbed-freescale-k64f-c.md)|
|[Tablet FUJITSU STYLISTIC con UBIQUITOUSWARE](http://www.fmworld.net/biz/uware/) |Windows10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-arrows-tab-csharp.md)|
|[Hanasis HW-5000](http://hanasis.com/index_eng_test/hanasis/product/pos/pos_hw-5000.php) |Windows 10 IoT Enterprise 2015 LTSB| C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows-10-iot-enterprise-2015-ltsb-hw-5000-c.md)|
|[Hitachi HF-W/IoT](http://www.hitachi-ics.co.jp/product/iot_ctr/index.html) |Windows 10| C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-HF-WIoT-c.md)|
|[HPE Edgeline EL10](http://www8.hp.com/h20195/v2/GetPDF.aspx/c04884747.pdf) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows-8.1-embedded-industry-enterprise-el10-csharp.md)|
|[HPE Edgeline EL20](http://www8.hp.com/h20195/v2/GetPDF.aspx/c04884769.pdf) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows-8.1-embedded-industry-enterprise-el20-csharp.md)|
|[iBase CSB200-897](http://www.ibase-usa.com/english/ProductDetail/EmbeddedComputing/CSB200-897) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows-csb200-897-csharp.md)|
|[iBase SI-12](http://www.ibase.com.tw/english/ProductDetail/DigitalSignagePlayer/SI-12) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows-si-12-csharp.md)|
|[IEI ECW-281B-BT](http://tw.ieiworld.com/product_groups/industrial/content.aspx?keyword=ECW-281&gid=09049552811981014603&cid=0D182494345754583862&id=0F330533395123928332#.Vufhbvl97Dc) |Windows 10| C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-iei-ecw-281b-bt-csharp.md)|
|[IEI ICECARE-10W](http://www.ieimobile.com/index.php?option=com_content&view=article&id=222&Itemid=21) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-icecare-10w-csharp.md)|
|[IEI DRPC-120](http://www.ieiworld.com/product_groups/industrial/content.aspx?gid=09049552811981014603&cid=0D182494345754583862&id=0E318374091597499543#.VqW3Q_l97Dd) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-drpc-120-csharp.md)|
|[IEI IVS-100-BT](http://tw.ieiworld.com/product_groups/industrial/content.aspx?gid=09049552811981014603&cid=0F202412454715193114&id=0F202496627608256517#.VqH1hvl97Dc) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-ivs-100-bt-csharp.md)|
|[IEI TANK-801-BT](http://www.ieiworld.com/product_groups/industrial/content.aspx?gid=09049552811981014603&cid=0D182496000560957303&id=0E316420627555447071#.VuZ1evl97IV) |Windows 10| C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-iei-tank-801-bt-csharp.md)|
|[IEI uIBX-230-BT](http://www.ieiworld.com/product_groups/industrial/content.aspx?gid=09049552811981014603&cid=0D182494345754583862&id=0E317335590193360443) |Windows10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-iei-uibx-230-bt-csharp.md)|
|[Ilevia Eve Server](http://www.ilevia.com/eve-server/) |Debian | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/linux-eve-raspberry-c.md)|
|[Intel Edison](http://www.intel.com/content/www/us/en/do-it-yourself/edison.html) |Yocto | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/yocto-intel-edison-c.md)|
|[Intel Edison](http://www.intel.com/content/www/us/en/do-it-yourself/edison.html) |Yocto | Javascript|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-run-sample.md)|
|[Inventec Corp Avatar](http://www.ioeworld.net/) |Android | Java|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/android-avatar-dev-board-java.md)|
|[Inventec Corp Avatar](http://www.ioeworld.net/) |Windows 10 IoT Core | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-iot-core-avatar-csharp.md)|
|[iWave iW-RainboW-G15M](http://www.iwavesystems.com/i-mx6-qseven-som.html) |WEC2013 | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/wec2013-iw-rainbow-G15m-c.md)|
|[Keith & Koep Myon](http://keith-koep.com/en/products/products-som/myon-1-features-snapdragon-410/) |Windows 10 IoT Core | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows-10-iot-core-myon-csharp.md)|
|[Keith & Koep TrizepsVII](http://keith-koep.com/en/products/products-trizeps/trizeps-vii-features/) |Debian | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/debian-trizeps-vii-c.md)|
|[Lanner LEC-6030](http://www.lannerinc.com/products/industrial-communication-platforms/industrial-cyber-security-box-pc/lec-6030) |Windows 10 IoT Core | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-iot-core-lanner-lec-6030e-csharp.md)|
|[Libelium Meshlium Xtreme](http://www.libelium.com/products/meshlium/) |Debian | Java|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/Debian-meshlium-java.md)|
|[MDS Technology IDM-IWP-HW-2](http://mdstec.com/en/contents/?no=326) |Windows | Java|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows-idm-iwp-hw-2-java.md)|
|[MechaTracks 3GPI](http://www.mechatrax.com/products/3gpi) |Raspbian | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/raspbian-3gpi-c.md)|
|[Minnowboard Max](http://www.minnowboard.org/meet-minnowboard-max/) |Windows 7,8, 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-minnowboard-max-csharp.md)|
|[MiTAC Computing Technology Pluto E220](http://client.mitac.com/products-Embedded-Box-PC-PlutoE220.html) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-pluto-e220-csharp.md)|
|[Motion Control Henry Board](http://www.runele.com/ca1/38/p-r-s/) |Yocto | Javascript|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/linux-henry-board-javascript.md)|
|[Nexcom NDiS B535](http://www.nexcom.com.tw/Products/multi-media-solutions/digital-signage-player/high-performance-player/digital-signage-player-ndis-b535) |Windows 10 IoT Core | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows-10-iot-core-ndis-b535-csharp.md)|
|[Nexcom NIFE100](http://www.nexcom.com.tw/Products/industrial-computing-solutions/pc-based-factory-automation/industr) |Windows 10 IoT Core | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows-10-iot-core-NIFE100-csharp.md)|
|[Nexcom NIFE200](http://www.nexcom.com.tw/Products/industrial-computing-solutions/pc-based-factory-automation/industr) |Windows 10 IoT Core | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows-10-iot-core-NIFE200-csharp.md)|
|[Nexcom NISE50](http://www.nexcom.com.tw/Products/industrial-computing-solutions/industrial-fanless-computer/atom-co) |Windows 10 IoT Core | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows-10-iot-core-NISE50-csharp.md)|
|[NEXCOM NISE 50C](http://www.nexcom.com/Products/industrial-computing-solutions/industrial-fanless-computer/atom-compact/fanless-computer-nise-50c) |Windows 10 IoT Core | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows-iot-core-nexcom-nise50-csharp.md)|
|[Nexcom VTC1010](http://www.nexcom.com.tw/Products/mobile-computing-solutions/in-vehicle-pc/in-vehicle-pc/in-vehicle-) |Windows 10 IoT Core | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows-10-iot-core-VTC1010-csharp.md)|
|[Nexcom VTC6210](http://www.nexcom.com.tw/Products/mobile-computing-solutions/train-pc/train-pc/transportation-comput) |Windows 10 IoT Core | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows-10-iot-core-VTC6210-csharp.md)|
|[NMR ND13](https://www.icp-nmr.com/news/IPC/msi-tablet.html) |Windows 10 | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-nd13-c.md)|
|[Pacific Control Systems G2021ES](http://www.pacificcontrols.net/products/G2021ES-Gateway.html) |Ubuntu | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/linux-g2021es-c.md)|
|[PANASONIC Toughpad FZ-F1](http://www.panasonic.com/global/home.html) |Windows 10 Mobile IoT Enterprise | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-iot-enterprise-fz-f1-csharp.md)|
|[Partner Tech EM-300](http://www.partner.com.tw/product/default.asp?prober=134) |Windows 10| C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-EM-300-c.md)|
|[PFU AR2100-model-120K](http://www.pfu.fujitsu.com/prodes/product/ar/ar2100_120k.html) |Windows 10 |C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-AR2100-model-120K-c.md)|
|[PFU AR2200-model-120K](http://www.pfu.fujitsu.com/prodes/product/ar/ar2200_120k.html) |Windows 10 |C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-AR2200-model-120K-c.md)|
|[Plat'Home OpenBlocks IoT BX1G](https://www.plathome.com/en/bx1g/bx1/) |Debian | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/debian-openblocks-iot-bx1g-c.md)|
|[Protech SE-8124](http://www.protech-usa.net/new_web/Products/product_en.asp?P_id=546&P_typeNo=4&P_subtypeNo=2#tab-1) | Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/Windows10-epc-btcr-csharp.md)|
|[Raspberry Pi 2](https://www.raspberrypi.org/products/raspberry-pi-2-model-b/) | Raspbian | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/raspbian-raspberrypi2-c.md)|
|[Raspberry Pi 2](https://www.raspberrypi.org/products/raspberry-pi-2-model-b/) | Raspbian | Java|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-run-sample.md)|
|[Raspberry Pi 2](https://www.raspberrypi.org/products/raspberry-pi-2-model-b/) | Raspbian | Javascript|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-run-sample.md)|
|[Raspberry Pi 2](https://www.raspberrypi.org/products/raspberry-pi-2-model-b/) | Windows 10 IoT Core| C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-iotcore-csharp.md)|
|[Raspberry Pi 3](https://www.raspberrypi.org/products/raspberry-pi-3-model-b/) | Raspbian | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/raspbian-raspberrypi2-c.md)|
|[Raspberry Pi 3](https://www.raspberrypi.org/products/raspberry-pi-3-model-b/) | Raspbian | Java|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-run-sample.md)|
|[Raspberry Pi 3](https://www.raspberrypi.org/products/raspberry-pi-3-model-b/) | Raspbian | Javascript|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-run-sample.md)|
|[Raspberry Pi 3](https://www.raspberrypi.org/products/raspberry-pi-3-model-b/) | Windows 10 IoT Core| C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-iotcore-csharp.md)|
|[RICOH IT9](http://industry.ricoh.com/fbx_board/it9/index.html) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-IT9-csharp.md)|
|[Samsung ARTIK](http://developer.samsung.com/artik) |Fedora | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/fedora-samsung-artik-c.md)|
|[SADE IoT Cloud Gateway](http://sade.io/sade-iot-cloud-family) |mbed 2.0 | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/mbed-sade-iot-cloud-gateway-c.md)|
|[SECO SBC-A80-eNUC](http://www.seco.com/prods/eu/sbc-a80-enuc.html) |Windows10 | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows-sbc-a80-enuc-c.md)|
|[SECO SBC-A80-eNUC](http://www.seco.com/prods/eu/sbc-a80-enuc.html) |Ubuntu | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/ubuntu-sbc-a80-enuc-c.md)|
|[SERAKU Midori Box](https://midori-cloud.net/service.html) |Debian | Python|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/debian-midoribox-python.md)|
|[SinTau SrL GROPIUS](http://www.sintau.it/index.php?option=com_virtuemart&view=productdetails&virtuemart_product_id=6&virtuemart_category_id=1&Itemid=360&lang=it) |Debian | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/debian-gropius-c.md)|
|[SinTau SrL GROPIUS](http://www.sintau.it/index.php?option=com_virtuemart&view=productdetails&virtuemart_product_id=6&virtuemart_category_id=1&Itemid=360&lang=it) |Debian | Java|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/debian-gropius-java.md)|
|[Smarthesia PasSy Gateway](http://www.smarthesia.com/#!home-page/gj699) |Yocto| Javascript|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/yocto-PasSy-Gateway-nodejs.md)|
|[SOTEC CloudPlug](http://cloudplug.info/) |YOCTO | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/linux-sotec-cloudplug-c.md)|
|[Sparkfun ThingDev](https://www.sparkfun.com/products/13711) |IDE de Arduino (RTOS)| Arduino, C|[Primeros pasos](https://azure.microsoft.com/documentation/samples/iot-hub-c-thingdev-getstartedkit/)|
|[Techman Robot Inc. TM5](http://tm-robot.com/robot/) |Windows Embedded Standard 7 | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows-embedded-standard-7-tm5-c.md)|
|[TI CC3200](http://www.ti.com/product/cc3200) |TI-RTOS 2.x | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/ti-rtos-ti-cc3200-c.md)|
|[Toradex Apalis iMX6](https://www.toradex.com/computer-on-modules/apalis-arm-family/freescale-imx-6) |Linux Angstrom(Yocto) | Java|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-run-sample-toradex-linux.md)|
|[Toradex Apalis iMX6](https://www.toradex.com/computer-on-modules/apalis-arm-family/freescale-imx-6) |Linux Angstrom(Yocto) | Javascript|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-run-sample-toradex-linux.md)|
|[Toradex Apalis T30](https://www.toradex.com/computer-on-modules/apalis-arm-family/nvidia-tegra-3) |Linux Angstrom(Yocto) | Java|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-run-sample-toradex-linux.md)|
|[Toradex Apalis T30](https://www.toradex.com/computer-on-modules/apalis-arm-family/nvidia-tegra-3) |Linux Angstrom(Yocto) | Javascript|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-run-sample-toradex-linux.md)|
|[Toradex Colibri iMX6](https://www.toradex.com/computer-on-modules/colibri-arm-family/freescale-imx6) |Linux Angstrom(Yocto) | Java|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-run-sample-toradex-linux.md)|
|[Toradex Colibri iMX6](https://www.toradex.com/computer-on-modules/colibri-arm-family/freescale-imx6) |Linux Angstrom(Yocto) | Javascript|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-run-sample-toradex-linux.md)|
|[Toradex Colibri T20](https://www.toradex.com/computer-on-modules/colibri-arm-family/nvidia-tegra-2) |Linux Angstrom(Yocto) | Java|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-run-sample-toradex-linux.md)|
|[Toradex Colibri T30](https://www.toradex.com/computer-on-modules/colibri-arm-family/nvidia-tegra-3) |Windows 10 IoT Core | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-iotcore-csharp.md)|
|[Toradex Colibri VF61](https://www.toradex.com/computer-on-modules/colibri-arm-family/freescale-vybrid-vf6xx) |Linux Angstrom(Yocto) | Java|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-run-sample-toradex-linux.md)|
|[Toradex Colibri VF61](https://www.toradex.com/computer-on-modules/colibri-arm-family/freescale-vybrid-vf6xx) |Linux Angstrom(Yocto) | Javascript|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-run-sample-toradex-linux.md)|
|[Toshiba FAB-s320](http://www.toshiba-tops.co.jp/embedded/fab_p/index_j.html) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-FAB-s320-csharp.md)|
|[Trex NGP](http://www.trex.com.tr/en/donanim_dcasngp8739_73.php) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-trex-ngp-csharp.md)|
|[Trueverit V4](http://www.trueverit.com/) |Linux personalizado | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/linux-trueverit-v4-c.md)|
|[USISH EDA8909](http://www.usish.com/) |Windows 10 | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-eda8909-csharp.md)|
|[VaneLife VHo1-X](http://www.vanelife.com/product/detail/2) |Linux Beaglebone | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/linux-vho1-x-c.md)|
|[Vantron VT-M2M-BTA-DE](http://vantrontech.com/hardwares/VT-M2M-BTA-DE.htm) |Windows 10 IoT Core | C#|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/windows10-iot-core-vt-m2m-bta-de-csharp.md)|
|[YASKAWA MMLink-GW](http://www.ysknet.co.jp/product/type/networkboard/mmlink-gw/index.html) |Linux | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/linux-MMLink-GW-c.md)|
|[Zerotech SMART](http://www.zerotech.com/smart-en.html) |Linaro | C|[Primeros pasos](https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/linaro-SMART-c.md)|

Consulte la [introducción al uso de estos dispositivos](https://azure.microsoft.com/develop/iot/get-started/) o visite nuestro [repositorio](https://github.com/Azure/azure-iot-sdks) de GitHub y busque en los documentos de dispositivo por idioma.

## Pasos siguientes

Aprenda más sobre el desarrollo de soluciones con [dispositivos certificados para IoT](http://azure.com/iotdev).

Para más información acerca de planificación de la implementación del Centro de IoT, consulte:

- [Compatibilidad con protocolos adicionales para Centro de IoT][lnk-protocols]
- [Comparación del Centro de IoT y los Centros de eventos][lnk-compare]
- [Escalado del Centro de IoT][lnk-scaling]

Para explorar aún más las funcionalidades de Centro de IoT, consulte:

- [Guía del desarrollador][lnk-devguide]
- [Exploración de la administración de dispositivos desde Centro de IoT de Azure con la IU de ejemplo][lnk-dmui]
- [SDK de puerta de enlace de IoT (beta): envío de mensajes del dispositivo a la nube con un dispositivo simulado usando Linux][lnk-gateway]
- [Administración de Centros de IoT a través del portal de Azure][lnk-portal]


[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/

[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-dmui]: iot-hub-device-management-ui-sample.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-portal]: iot-hub-manage-through-portal.md

<!---HONumber=AcomDC_0720_2016-->