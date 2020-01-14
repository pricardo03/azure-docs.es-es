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
ms.date: 01/05/2020
ms.author: memildin
ms.openlocfilehash: f2fda893ad84aaf9d11d26d761f5395c7f5650d2
ms.sourcegitcommit: ff9688050000593146b509a5da18fbf64e24fbeb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2020
ms.locfileid: "75666379"
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

Para obtener una lista de las alertas de Azure App Service, consulte la [Tabla de referencia de alertas](alerts-reference.md#alerts-azureappserv).

## Contenedores de Azure<a name="azure-containers"></a>

Security Center proporciona detección de amenazas en tiempo real para los entornos en contenedores y genera alertas de actividades sospechosas. Puede usar esta información para corregir problemas de seguridad y mejorar la seguridad de los contenedores rápidamente.

Detectamos amenazas en diferentes niveles: 

* **Nivel de host**: el agente de Security Center (disponible en el nivel estándar; consulte los [precios](security-center-pricing.md) para obtener detalles) supervisa Linux en busca de actividades sospechosas. El agente desencadena alertas de actividades sospechosas que se originan en el nodo o en un contenedor que se ejecuta en el mismo. Algunos ejemplos de estas actividades son la detección shell de web y la conexión con direcciones IP sospechosas conocidas.

    Para obtener una visión más detallada de la seguridad del entorno en el contenedor, el agente supervisa el análisis específico de ese contenedor. Esto desencadenará alertas de eventos como la creación de contenedores con privilegios, las actividades sospechosas de acceso a los servidores de API y los servidores de Secure Shell (SSH) que se ejecutan dentro de un contenedor de Docker.

    >[!NOTE]
    > Si decide no instalar los agentes en los hosts, solo recibirá un subconjunto de las ventajas y alertas de detección de amenazas. Aún así, seguirá recibiendo alertas relacionadas con el análisis de redes y las comunicaciones con servidores malintencionados.

    Para obtener una lista de las alertas de nivel de host, consulte la [Tabla de referencia de alertas](alerts-reference.md#alerts-containerhost).


* En cuanto al **nivel de clúster de AKS**, hay una supervisión de la detección de amenazas basada en el análisis de los registros de auditoría de Kubernetes. Para habilitar esta supervisión **sin agente**, agregue la opción de Kubernetes a la suscripción desde la página **Precios y configuración** (consulte los [precios](security-center-pricing.md)). Para generar alertas en este nivel, Security Center supervisa los servicios que administra AKS con los registros que recupera el mismo AKS. Entre los ejemplos de eventos en este nivel se incluyen los paneles de Kubernetes expuestos y la creación de roles con privilegios elevados y de montajes confidenciales.

    >[!NOTE]
    > Igualmente, Security Center genera alertas de detección para las acciones y las implementaciones de Azure Kubernetes Service que se producen después de que la opción de Kubernetes esté habilitada en la configuración de la suscripción. 

    Para obtener una lista de las alertas de nivel de clúster de AKS, consulte la [Tabla de referencia de alertas](alerts-reference.md#alerts-akscluster).

Además, nuestro equipo global de investigadores de seguridad supervisa constantemente el panorama de las amenazas. Agregan alertas específicas del contenedor y vulnerabilidades a medida que se detectan.