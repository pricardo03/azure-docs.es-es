---
title: Detección de amenazas para Cloud Native Compute en Azure Security Center | Microsoft Docs
description: En este tema se presentan las alertas de Cloud Native Compute disponibles en Azure Security Center. en Azure Security Center.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: 5aa5efcf-9f6f-4aa1-9f72-d651c6a7c9cd
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: monhaber
ms.openlocfilehash: bc3cb66d43e71777e06c6bd63dcff35e2ff19df8
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571318"
---
# <a name="threat-detection-for-cloud-native-compute-in-azure-security-center"></a>Detección de amenazas para Cloud Native Compute en Azure Security Center

Como proveedor de nube, Security Center aprovecha la visibilidad única que tiene para analizar registros internos e identificar metodología de ataques en varios destinos. Este tema presenta las alertas disponibles para los siguientes servicios de Azure:

* [Azure App Service](#app-services)
* [Contenedores](#azure-containers) 

## Azure App Service <a name="app-services"></a>

Security Center aprovecha la escala de la nube para identificar ataques dirigidos a aplicaciones de clientes que se ejecutan mediante Azure App Service. Con la presencia de aplicaciones web en prácticamente cualquier red moderna, los atacantes realizan sondeos para encontrarlas y aprovechar las vulnerabilidades. Antes de enrutarse a entornos específicos, las solicitudes a aplicaciones que se ejecutan en Azure atraviesan varias puertas de enlace donde se inspeccionan y registran. A continuación, estos datos se usan para identificar vulnerabilidades y atacantes, así como para aprender nuevos patrones que se usarán más adelante.

Al aprovechar la visibilidad que Azure tiene como proveedor de nube, Security Center analiza los registros internos de App Service para metodología de ataques en múltiples destinos, como, por ejemplo,los ataques distribuidos y de exploración generalizada. Este tipo de ataque normalmente procede de un pequeño subconjunto de direcciones IP y exhibe patrones de rastreo a puntos de conexión similares en varios hosts, en busca de una página o complemento vulnerables. Este ataque se puede detectar mediante la nube, pero no se puede identificar desde el punto de vista de un solo host.

Security Center también tiene acceso a los espacios aislados y máquinas virtuales subyacentes. Junto con el análisis forense de memoria, la infraestructura puede contar qué está ocurriendo, desde un nuevo ataque que prolifera en la red hasta riesgos concretos para las máquinas de los clientes. Por lo tanto, Security Center puede detectar ataques contra aplicaciones web mucho después de que se aprovechen sus vulnerabilidades.

> [!div class="mx-tableFixed"]

|Alerta|DESCRIPCIÓN|
|---|---|
|**Invocación de tema de WordPress sospechoso detectada**|El registro de actividad de Azure App Service indica una posible actividad de inyección de código en el recurso de App Service.<br/> Esta actividad sospechosa es similar a la actividad que manipula un tema de WordPress para permitir la ejecución de código en el servidor, seguida de una solicitud web directa para invocar el archivo de tema manipulado. Este tipo de actividad se ha observado en el pasado como parte de una campaña de ataques a través de WordPress.|
|**Conexión a página web desde una dirección IP anómala detectada**|El registro de actividad de Azure App Service indica una conexión a una página web confidencial desde una dirección de origen que nunca se había conectado a ella antes. Esto podría indicar que alguien está intentando realizar un ataque por fuerza bruta en las páginas de administración de la aplicación web. También puede deberse a que un usuario legítimo esté utilizando una nueva dirección IP.|
|**Se ha encontrado en Información sobre amenazas una dirección IP que se ha conectado a la interfaz de FTP de Azure App Service**|El análisis de registros de FTP de Azure App Service ha detectado una conexión desde una dirección de origen que se ha encontrado en la fuente Información sobre amenazas. Durante esta conexión, un usuario ha accedido a las páginas que se indican a continuación.|
|**Huella digital web detectada**|El registro de actividad de Azure App Service indica una posible actividad de huella digital web en el recurso de App Service. <br/>Esta actividad sospechosa detectada está asociada con una herramienta llamada Blind Elephant. La herramienta crea una huella digital de servidores web e intenta detectar las aplicaciones instaladas y sus versiones. Los atacantes suelen utilizar esta herramienta para sondear aplicaciones web en busca de vulnerabilidades.|
|**Acceso sospechoso a página web posiblemente vulnerable detectado**|El registro de actividad de Azure App Service indica que se ha accedido a una página web que parece ser confidencial. <br/>Esta actividad sospechosa se ha originado desde una dirección de origen cuyo patrón de acceso es similar al de un escáner web. Este tipo de actividad suele estar asociada a un intento de un atacante de examinar la red para intentar obtener acceso a páginas web confidenciales o vulnerables.|
|**Archivo PHP en la carpeta de carga**|El registro de actividad de Azure App Service indica que se ha accedido a una página PHP sospechosa ubicada en la carpeta de carga. <br/>Este tipo de carpeta no suele contener archivos PHP. La existencia de este tipo de archivo podría indicar un ataque que aprovecha vulnerabilidades de carga de archivos arbitrarias.|
|**Intento de ejecución de comandos de Linux en una instancia de App Service de Windows**|El análisis de procesos de App Service ha detectado un intento de ejecutar un comando de Linux en una instancia de App Service de Windows. La aplicación web estaba ejecutando esta acción. Este comportamiento se ve a menudo en campañas que aprovechan una vulnerabilidad en una aplicación web común.|
|**Ejecución de PHP sospechoso detectada**|Los registros de la máquina indican que un proceso PHP sospechoso se está ejecutando. La acción incluye un intento de ejecutar comandos del sistema operativo o código PHP desde la línea de comandos mediante el proceso PHP. Aunque este comportamiento puede ser legítimo, en aplicaciones web también se observa en actividades malintencionadas, como intentos de infectar sitios web con shells web.|
|**Ejecución de procesos desde la carpeta temporal**|El análisis de procesos de App Service ha detectado una ejecución de un proceso desde la carpeta temporal de la aplicación. Aunque este comportamiento puede ser legítimo, en aplicaciones web también se observa en actividades malintencionadas.|
|**Intento de ejecución de comando con privilegios elevados detectado**|El análisis de procesos de App Service ha detectado un intento de ejecutar un comando que requiere privilegios elevados. El comando se ejecutó en el contexto de la aplicación web. Aunque este comportamiento puede ser legítimo, en aplicaciones web también se observa en actividades malintencionadas.|

> [!NOTE]
> La detección de amenazas de Security Center para App Service no está disponible actualmente en Azure Government ni en regiones de nube soberana.

Para más información acerca de las alertas de detección de amenazas de App Service, visite Protección de App Service con Azure Security Center y consulte cómo se habilita la supervisión y la protección de las cargas de trabajo de App Service.

## Contenedores <a name="azure-containers"></a>

Security Center ofrece detección de amenazas en tiempo real para los contenedores en máquinas Linux basados en el marco auditd. Las alertas identifican varias actividades sospechosas de Docker, como la creación de un contenedor con privilegios en el host, una indicación de que un servidor Secure Shell (SSH) se ejecuta dentro de un contenedor de Docker o el uso de mineros de criptografía. Puede usar esta información para corregir problemas de seguridad y mejorar la seguridad de los contenedores rápidamente. Además de las detecciones de Linux, Security Center también ofrece análisis que son más específicos para las implementaciones de contenedores.
