---
title: Detección de amenazas en Cloud Native Computing en Azure Security Center | Microsoft Docs
description: En este artículo se presentan las alertas de Cloud Native Compute disponibles en Azure Security Center.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 5aa5efcf-9f6f-4aa1-9f72-d651c6a7c9cd
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/05/2019
ms.author: memildin
ms.openlocfilehash: 6b6acb0ae1452795fe02906779b920e4b41f9a55
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2019
ms.locfileid: "73748393"
---
# <a name="threat-detection-for-cloud-native-computing-in-azure-security-center"></a>Detección de amenazas para Cloud Native Computing en Azure Security Center

Como solución nativa, Azure Security Center tiene una visibilidad exclusiva de los registros internos para la identificación de la metodología de los ataques en varios destinos. En este artículo se presentan las alertas disponibles para los siguientes servicios de Azure:

* [Azure App Service](#app-services)
* [Azure Containers](#azure-containers) 

> [!NOTE]
> Estos servicios no están disponibles actualmente en Azure Government ni en la regiones de nubes soberanas.

## Azure App Service <a name="app-services"></a>

Security Center usa la escala de la nube para identificar ataques dirigidos a aplicaciones que se ejecutan mediante App Service. Los atacantes sondean las aplicaciones web para buscar y aprovechar los puntos débiles. Antes de enrutarse a entornos específicos, las solicitudes para las aplicaciones que se ejecutan en Azure atraviesan varias puertas de enlace donde se las inspecciona y registra. A continuación, estos datos se usan para identificar vulnerabilidades y atacantes, así como para aprender nuevos patrones que se usarán más adelante.

Al usar la visibilidad que Azure tiene como proveedor de nube, Security Center analiza los registros internos de App Service para metodología de ataques en múltiples destinos, como, por ejemplo, la metodología incluye el análisis generalizado y los ataques distribuidos. Este tipo de ataque normalmente procede de un pequeño subconjunto de direcciones IP y muestra patrones de rastreo a puntos de conexión similares en varios hosts. Los ataques buscan una página o complemento vulnerables y no se pueden identificar desde el punto de vista de un solo host.

Security Center también tiene acceso a los espacios aislados y máquinas virtuales subyacentes. Junto con el análisis forense de memoria, la infraestructura puede contar qué está ocurriendo, desde un nuevo ataque que prolifera en la red hasta riesgos concretos para las máquinas de los clientes. Por lo tanto, incluso si se implementa Security Center una vez que se han aprovechado las vulnerabilidades de una aplicación web, es posible que pueda detectar los ataques en curso.

> [!div class="mx-tableFixed"]

|Alerta|DESCRIPCIÓN|
|---|---|
|**Invocación de tema de WordPress sospechoso detectada**|El registro de actividad de App Service indica una posible actividad de inyección de código en el recurso de App Service.<br/> Esta actividad sospechosa es similar a la actividad que manipula un tema de WordPress para permitir la ejecución de código en el servidor, seguida de una solicitud web directa para invocar el archivo de tema manipulado. Este tipo de actividad puede formar parte de una campaña de ataque a través de WordPress.|
|**Conexión a página web desde una dirección IP anómala detectada**|El registro de actividad de App Service indica una conexión a una página web confidencial desde una dirección de origen que nunca se había conectado a ella antes. Esta conexión puede indicar que alguien intenta realizar un ataque por fuerza bruta en las páginas de administración de su aplicación web. También puede ser el resultado de un usuario legítimo que usa una nueva dirección IP.|
|**Se ha encontrado en Información sobre amenazas una dirección IP que se ha conectado a la interfaz de FTP de Azure App Service**|El análisis de registros de FTP de App Service ha detectado una conexión desde una dirección de origen que se ha encontrado en la fuente de información sobre amenazas. Durante esta conexión, un usuario ha accedido a las páginas que aquí se indican.|
|**Huella digital web detectada**|El registro de actividad de App Service indica una posible actividad de huella digital web en el recurso de App Service. <br/>Esta actividad sospechosa está asociada con una herramienta llamada Blind Elephant. La herramienta crea una huella digital de servidores web e intenta detectar las aplicaciones instaladas y sus versiones. Los atacantes suelen utilizar esta herramienta para sondear aplicaciones web en busca de vulnerabilidades.|
|**Acceso sospechoso a página web posiblemente vulnerable detectado**|El registro de actividad de App Service indica que se ha accedido a una página web que parece ser confidencial. <br/>Esta actividad sospechosa se ha originado desde una dirección de origen cuyo patrón de acceso es similar al de un escáner web. Este tipo de actividad suele estar asociada a un intento de un atacante de examinar la red para intentar obtener acceso a páginas web confidenciales o vulnerables.|
|**Archivo PHP en la carpeta de carga**|El registro de actividad de App Service indica que se ha accedido a una página PHP sospechosa ubicada en la carpeta de carga. <br/>Este tipo de carpeta no suele contener archivos PHP. La existencia de este tipo de archivo podría indicar un ataque que aprovecha vulnerabilidades de carga de archivos arbitrarias.|
|**Intento de ejecución de comandos de Linux en una instancia de App Service de Windows**|El análisis de procesos de App Service ha detectado un intento de ejecutar un comando de Linux en una instancia de App Service de Windows. La aplicación web estaba ejecutando esta acción. Este comportamiento se ve a menudo en campañas que aprovechan una vulnerabilidad en una aplicación web común.|
|**Ejecución de PHP sospechoso detectada**|Los registros de la máquina indican que se está ejecutando un proceso PHP sospechoso. La acción incluye un intento de ejecutar comandos del sistema operativo o código PHP desde la línea de comandos mediante el proceso PHP. Aunque este comportamiento puede ser legítimo, en aplicaciones web podría indicar actividades malintencionadas, como intentos de infectar sitios web con shells web.|
|**Ejecución de procesos desde la carpeta temporal**|El análisis de procesos de App Service ha detectado una ejecución de un proceso desde la carpeta temporal de la aplicación. Aunque este comportamiento puede ser legítimo, en aplicaciones web podría indicar actividades malintencionadas.|
|**Intento de ejecución de comando con privilegios elevados detectado**|El análisis de procesos de App Service ha detectado un intento de ejecutar un comando que requiere privilegios elevados. El comando se ejecutó en el contexto de la aplicación web. Aunque este comportamiento puede ser legítimo, en aplicaciones web podría indicar actividades malintencionadas.|

## Contenedores de Azure<a name="azure-containers"></a>

Security Center proporciona detección de amenazas en tiempo real para los entornos en contenedores y genera alertas de actividades sospechosas. Puede usar esta información para corregir problemas de seguridad y mejorar la seguridad de los contenedores rápidamente.

Detectamos amenazas en diferentes niveles: 

* **Nivel de host**: el agente de Security Center (disponible en el nivel estándar; consulte los [precios](security-center-pricing.md) para obtener detalles) supervisa Linux en busca de actividades sospechosas. El agente desencadena alertas de actividades sospechosas que se originan en el nodo o en un contenedor que se ejecuta en el mismo. Algunos ejemplos de estas actividades son la detección shell de web y la conexión con direcciones IP sospechosas conocidas.

    Para obtener una visión más detallada de la seguridad del entorno en el contenedor, el agente supervisa el análisis específico de ese contenedor. Esto desencadenará alertas de eventos como la creación de contenedores con privilegios, las actividades sospechosas de acceso a los servidores de API y los servidores de Secure Shell (SSH) que se ejecutan dentro de un contenedor de Docker.

    >[!NOTE]
    > Si decide no instalar los agentes en los hosts, solo recibirá un subconjunto de las ventajas y alertas de detección de amenazas. Aún así, seguirá recibiendo alertas relacionadas con el análisis de redes y las comunicaciones con servidores malintencionados.

* En cuanto al **nivel de clúster de AKS**, hay una supervisión de la detección de amenazas basada en el análisis de los registros de auditoría de Kubernetes. Para habilitar esta supervisión **sin agente**, agregue la opción de Kubernetes a la suscripción desde la página **Precios y configuración** (consulte los [precios](security-center-pricing.md)). Para generar alertas en este nivel, Security Center supervisa los servicios que administra AKS con los registros que recupera el mismo AKS. Entre los ejemplos de eventos en este nivel se incluyen los paneles de Kubernetes expuestos y la creación de roles con privilegios elevados y de montajes confidenciales.

    >[!NOTE]
    > Igualmente, Security Center genera alertas de detección para las acciones y las implementaciones de Azure Kubernetes Service que se producen después de que la opción de Kubernetes esté habilitada en la configuración de la suscripción. 

Además, nuestro equipo global de investigadores de seguridad supervisa constantemente el panorama de las amenazas. Agregan alertas específicas del contenedor y vulnerabilidades a medida que se detectan.


### <a name="aks-cluster-level-alerts"></a>Alertas del nivel de clúster de AKS

> [!div class="mx-tableFixed"]

|Alerta|DESCRIPCIÓN|
|---|---|
|**VERSIÓN PRELIMINAR: se ha detectado un enlace de rol al rol de administrador de clústeres**|El análisis del registro de auditoría de Kubernetes detectó un nuevo enlace al rol de administrador de clústeres, con lo cual se han concedido privilegios de administrador. Conceder de forma innecesaria privilegios de administrador podría provocar problemas de elevación de privilegios en el clúster.|
|**VERSIÓN PRELIMINAR: se ha detectado un panel de Kubernetes expuesto**|El análisis del registro de auditoría de Kubernetes detectó la exposición del panel de Kubernetes por un servicio LoadBalancer. Los paneles expuestos permiten el acceso sin autenticación a la administración del clúster y suponen una amenaza para la seguridad.|
|**VERSIÓN PRELIMINAR: se ha detectado un nuevo rol con privilegios elevados**|El análisis del registro de auditoría de Kubernetes detectó un nuevo rol con privilegios elevados. Un enlace a un rol con privilegios elevados concede al usuario o grupo privilegios elevados en el clúster. Conceder de forma innecesaria privilegios elevados podría provocar problemas de elevación de privilegios en el clúster.|
|**VERSIÓN PRELIMINAR: se ha detectado un nuevo contenedor en el espacio de nombres kube-system**|El análisis del registro de auditoría de Kubernetes detectó un contenedor nuevo en el espacio de nombres kube-system que no está entre los contenedores que se ejecutan normalmente en este espacio de nombres. Los espacios de nombres kube-system no deben contener recursos de usuario. Los atacantes pueden usar este espacio de nombres para ocultar componentes malintencionados.|
|**VERSIÓN PRELIMINAR: contenedor de minería de datos de moneda digital detectado**|El análisis del registro de auditoría de Kubernetes detectó un contenedor que tiene una imagen asociada a una herramienta de minería de monedas digitales.|
|**VERSIÓN PRELIMINAR: se ha detectado un contenedor con privilegios**|El análisis del registro de auditoría de Kubernetes detectó un contenedor nuevo con privilegios. Un contenedor con privilegios tiene acceso a los recursos del nodo y rompe el aislamiento entre contenedores. Si se pone en peligro, un atacante puede usar el contenedor con privilegios para acceder al nodo.|
|**VERSIÓN PRELIMINAR: se ha detectado un contenedor con un volumen confidencial montado**|El análisis del registro de auditoría de Kubernetes detectó un nuevo contenedor con un volumen confidencial montado. El volumen detectado es del tipo hostPath y monta una carpeta o un archivo confidenciales del nodo en el contenedor. Si el contenedor corre peligro, el atacante puede usar este montaje para obtener acceso al nodo.|



### <a name="host-level-alerts"></a>Alertas de nivel de host

> [!div class="mx-tableFixed"]

|Alerta|DESCRIPCIÓN|
|---|---|
|**Se ha detectado un contenedor con privilegios**|Los registros de la máquina indican que se está ejecutando un contenedor de Docker con privilegios. Un contenedor con privilegios tiene acceso total a los recursos del host. Si se pone en peligro, un atacante puede usar el contenedor con privilegios para acceder a la máquina host.|
|**Se ha ejecutado un comando con privilegios en el contenedor**|Los registros de la máquina indican que se ha ejecutado un comando con privilegios en un contenedor de Docker con privilegios. Un comando con privilegios ha extendido sus privilegios a la máquina host.|
|**Se ha detectado un demonio de Docker expuesto**|Los registros de la máquina indican que el demonio de Docker (dockerd) expone un socket TCP. De manera predeterminada, la configuración de Docker no usa cifrado ni autenticación cuando un socket TCP está habilitado. Cualquiera con acceso al puerto pertinente puede obtener acceso total al demonio de Docker.|
|**Se ejecuta un servidor SSH dentro de un contenedor**|Los registros de la máquina indican que se está ejecutando un servidor SSH en un contenedor de Docker. Aunque este comportamiento puede ser intencionado, con frecuencia indica que un contenedor está mal configurado o que es vulnerable.|
|**Se ha detectado un contenedor con la imagen de un extractor**|Los registros de la máquina indican que se ha ejecutado un contenedor de Docker que ejecuta una imagen asociada a la minería de monedas digitales. Este comportamiento puede indicar posiblemente que se está realizando un uso fraudulento de los recursos.|
|**Solicitud sospechosa a la API de Kubernetes**|Los registros de la máquina indican que se ha realizado una solicitud sospechosa a la API de Kubernetes. La solicitud se envió desde un nodo de Kubernetes, posiblemente desde uno de los contenedores que se ejecutan en el nodo. Aunque este comportamiento puede ser intencionado, podría indicar que el nodo ejecuta un contenedor en peligro.|
|**Solicitud sospechosa al panel de Kubernetes**|Los registros de la máquina indican que se ha realizado una solicitud sospechosa al panel de Kubernetes. La solicitud se envió desde un nodo de Kubernetes, posiblemente desde uno de los contenedores que se ejecutan en el nodo. Aunque este comportamiento puede ser intencionado, podría indicar que el nodo ejecuta un contenedor en peligro.|