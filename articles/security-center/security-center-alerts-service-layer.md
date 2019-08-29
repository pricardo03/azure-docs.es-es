---
title: Detección de amenazas para la capa de servicios de Azure en Azure Security Center | Microsoft Docs
description: En este tema se presentan las alertas de la capa de servicios de Azure disponibles en Azure Security Center.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: 33c45447-3181-4b75-aa8e-c517e76cd50d
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 8/18/2019
ms.author: v-mohabe
ms.openlocfilehash: 70b43c65703316e5dee8e9cf2cf86fe982a49592
ms.sourcegitcommit: 55e0c33b84f2579b7aad48a420a21141854bc9e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/19/2019
ms.locfileid: "69624780"
---
# <a name="threat-detection-for-azure-service-layer-in-azure-security-center"></a>Detección de amenazas para la capa de servicios de Azure en Azure Security Center

En este tema se presentan las alertas de Security Center disponibles al supervisar las siguientes capas de servicios de Azure.

* [Capa de red de Azure](#network-layer)
* [Capa de administración de Azure (Azure Resource Manager) (versión preliminar)](#management-layer)

>[!NOTE]
>Con la telemetría que Security Center obtiene al acceder a las fuentes internas de Azure, el análisis proporcionado a continuación es aplicable a todos los tipos de recursos.

## Capa de red de Azure<a name="network-layer"></a>

El análisis de la capa de red de Security Center se basa en [datos IPFIX](https://en.wikipedia.org/wiki/IP_Flow_Information_Export) de ejemplo, que son encabezados de paquete recopilados por los enrutadores principales de Azure. Sobre la base de esta fuente de datos, los modelos de Machine Learning de Security Center identifican y marcan actividades de tráfico malintencionadas. Para enriquecer direcciones IP, Security Center aprovecha la base de datos de información sobre amenazas de Microsoft.

> [!div class="mx-tableFixed"]

|Alerta|DESCRIPCIÓN|
|---|---|
|**Actividad de red RDP saliente sospechosa**|El análisis de las muestras de tráfico ha detectado una comunicación del Protocolo de escritorio remoto (RDP) saliente anómala con origen en un recurso de su implementación. Esta actividad se considera anómala para este entorno y puede indicar que el recurso está en peligro y ahora se usa para el punto de conexión RDP externo atacado por fuerza bruta. Tenga en cuenta que este tipo de actividad puede dar lugar a que entidades externas marquen su dirección IP como malintencionada.|
|**Actividad de red RDP saliente sospechosa hacia varios destinos**|El análisis de las muestras de tráfico ha detectado una comunicación del Protocolo de escritorio remoto (RDP) saliente anómala con origen en un recurso de su implementación con varios destinos. Esta actividad se considera anómala para este entorno y puede indicar que el recurso está en peligro y ahora se usa para los puntos de conexión RDP externos atacados por fuerza bruta. Tenga en cuenta que este tipo de actividad puede dar lugar a que entidades externas marquen su dirección IP como malintencionada.|
|**Actividad de red de SSH saliente sospechosa**|El análisis de las muestras de tráfico ha detectado una comunicación de Secure Shell (SSH) saliente anómala con origen en un recurso de su implementación. Esta actividad se considera anómala para este entorno y puede indicar que el recurso está en peligro y ahora se usa para el punto de conexión SSH externo atacado por fuerza bruta. Tenga en cuenta que este tipo de actividad puede dar lugar a que entidades externas marquen su dirección IP como malintencionada.|
|**Actividad de red de SSH saliente sospechosa hacia varios destinos**|El análisis de las muestras de tráfico ha detectado una comunicación de Secure Shell (SSH) saliente anómala con origen en un recurso de su implementación con varios destinos. Esta actividad se considera anómala para este entorno y puede indicar que el recurso está en peligro y ahora se usa para los puntos de conexión SSH externos atacados por fuerza bruta. Tenga en cuenta que este tipo de actividad puede dar lugar a que entidades externas marquen su dirección IP como malintencionada.|
|**Actividad de red SSH entrante sospechosa procedente de varios orígenes**|El análisis de las muestras de tráfico ha detectado comunicaciones de SSH entrante anómala desde varios orígenes con destino en un recurso de su implementación. Varias direcciones IP únicas conectadas al recurso se consideran anómalas para este entorno. Esta actividad puede indicar un intento de ataque por fuerza bruta a la interfaz de SSH desde varios hosts (Botnet).|
|**Actividad de red SSH entrante sospechosa**|El análisis de las muestras de tráfico ha detectado una comunicación de SSH entrante anómala con destino en un recurso de su implementación. Un número relativamente elevado de conexiones entrantes al recurso se consideran anómalas para este entorno. Esta actividad puede indicar un intento de ataque por fuerza bruta a su interfaz de SSH.
|**Actividad de red RDP entrante sospechosa procedente de varios orígenes**|El análisis de las muestras de tráfico ha detectado comunicaciones de RDP entrante anómala desde varios orígenes con destino en un recurso de su implementación. Varias direcciones IP únicas conectadas al recurso se consideran anómalas para este entorno. Esta actividad puede indicar un intento de ataque por fuerza bruta a la interfaz de RDP desde varios hosts (Botnet).|
|**Actividad de red RDP entrante sospechosa**|El análisis de las muestras de tráfico ha detectado una comunicación de RDP entrante anómala con destino en un recurso de su implementación. Un número relativamente elevado de conexiones entrantes al recurso se consideran anómalas para este entorno. Esta actividad puede indicar un intento de ataque por fuerza bruta a su interfaz de SSH.|
|**Detectada comunicación de red con una dirección malintencionada**|El análisis de las muestras de tráfico de red ha detectado una comunicación desde un recurso de su implementación con un posible servidor de comando y control (C&C). Tenga en cuenta que este tipo de actividad puede dar lugar a que entidades externas marquen su dirección IP como malintencionada.|

Para comprender cómo Security Center puede usar señales relacionadas con la red para aplicar protección contra amenazas, consulte [Detecciones de DNS heurísticas en Azure Security Center](https://azure.microsoft.com/blog/heuristic-dns-detections-in-azure-security-center/).
## Capa de administración de Azure (Azure Resource Manager) (versión preliminar)<a name ="management-layer"></a>

>[!NOTE]
>La capa de protección de Security Center basada en Azure Resource Manager está actualmente en versión preliminar.

Security Center ofrece una capa adicional de protección al aprovechar eventos de Azure Resource Manager, lo que se considera el plano de control de Azure. Al analizar los registros de Azure Resource Manager, Security Center detecta operaciones inusuales o potencialmente peligrosas en el entorno de suscripción de Azure.

> [!div class="mx-tableFixed"]

|Alerta|DESCRIPCIÓN|
|---|---|
|**Ejecución del kit de herramientas MicroBurst**|Se ha detectado la ejecución de un conocido kit de herramientas de reconocimiento de entornos de nube en su entorno. Un atacante (o evaluador de penetración) puede usar la herramienta "MicroBurst" (consulte https://github.com/NetSPI/MicroBurst) ) para asignar sus recursos de suscripciones, identificar configuraciones poco seguras y revelar información confidencial.|
|**Ejecución del kit de herramientas Azurite**|Se ha detectado la ejecución de un conocido kit de herramientas de reconocimiento de entornos de nube en su entorno. Un atacante (o evaluador de penetración) puede usar la herramienta "Azurite" (consulte https://github.com/mwrlabs/Azurite) ) para asignar sus recursos de suscripciones e identificar configuraciones poco seguras.|
|**Sesión de administración sospechosa con una cuenta inactiva**|El análisis de registros de actividad de suscripción ha detectado un comportamiento sospechoso. Una entidad de seguridad que no se ha usado durante un largo período de tiempo ahora está realizando acciones que pueden garantizar la persistencia de un atacante.|
|**Sesión de administración sospechosa con PowerShell**|El análisis de registros de actividad de suscripción ha detectado un comportamiento sospechoso. Una entidad de seguridad que no usa con frecuencia PowerShell para administrar el entorno de suscripción ahora usa PowerShell y realiza acciones que pueden garantizar la persistencia de un atacante.|
|**Uso de técnicas de persistencia avanzadas de Azure**|El análisis de registros de actividad de suscripción ha detectado un comportamiento sospechoso. Se han concedido entidades de identidad legitimizadas a roles personalizados. Esto puede provocar que el atacante consiga persistencia en un entorno de cliente de Azure.|
|**Actividad desde un país poco frecuente**|Se ha producido actividad desde una ubicación que ningún usuario de la organización ha visitado recientemente o nunca.<br/>Esta detección tiene en cuenta las ubicaciones de actividad anteriores para determinar las ubicaciones nuevas e infrecuentes. El motor de detección de anomalías almacena información sobre las ubicaciones anteriores utilizadas por los usuarios de la organización. 
|**Actividad desde direcciones IP anónimas**|Se ha detectado actividad de usuarios desde una dirección IP que se ha identificado como una dirección IP de proxy anónima. <br/>A menudo, estos servidores proxy los usan los usuarios que desean ocultar la dirección IP del dispositivo y es posible que se usen con fines malintencionados. Esta detección aprovecha un algoritmo de aprendizaje automático que reduce "falsos positivos", como las direcciones IP mal etiquetadas que otros usuarios de la organización utilizan ampliamente.|
|**Viaje imposible detectado**|Se han producido dos actividades de usuario (en una o varias sesiones) que se originan desde ubicaciones geográficamente distantes dentro de un período de tiempo menor que el tiempo que habría tardado el usuario en viajar de la primera ubicación a la segunda. Esto indica que otro usuario está usando las mismas credenciales. <br/>Esta detección aprovecha un algoritmo de aprendizaje automático que omite "falsos positivos" obvios que contribuyen a las condiciones de viaje imposible, como las VPN y las ubicaciones que otros usuarios de la organización usan con regularidad. La detección tiene un período de aprendizaje inicial de siete días, durante el cual aprende el patrón de actividad del nuevo usuario.|

>[!NOTE]
> Varios de los análisis anteriores se realizan mediante Microsoft Cloud App Security (MCAS). Para poder usar estos análisis, se necesita una licencia de MCAS activada. Si tiene una licencia de MCAS, estas alertas están habilitadas de forma predeterminada. Para deshabilitarlas:
>
> 1. En la hoja Security Center, seleccione **Directiva de seguridad**. Para la suscripción que desea cambiar, haga clic en **Editar la configuración**.
> 2. Haga clic en **Detección de amenazas**.
> 3. En **Enable integrations** (Habilitar integraciones), desactive la opción **Allow Microsoft Cloud App Security to access my data** (Permitir a Microsoft Cloud App Security acceder a mis datos) y haga clic en **Guardar**.

>[!NOTE]
>Azure Security Center almacena datos de clientes relacionados con la seguridad en la misma zona geográfica que su recurso. Si Microsoft aún no ha implementado Azure Security Center en la zona geográfica del recurso, almacenará los datos en Estados Unidos. Cuando Microsoft Cloud App Security (MCAS) esté habilitado, esta información se almacenará con arreglo a las reglas de ubicación geográfica de MCAS. Para más información, consulte [Almacenamiento de datos para servicios no regionales](https://azuredatacentermap.azurewebsites.net/).
