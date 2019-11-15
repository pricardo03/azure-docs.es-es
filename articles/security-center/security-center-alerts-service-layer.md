---
title: 'Detección de amenazas en la capa de servicios de Azure: Azure Security Center'
description: En este tema se presentan las alertas de la capa de servicios de Azure disponibles en Azure Security Center.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 33c45447-3181-4b75-aa8e-c517e76cd50d
ms.service: security-center
ms.topic: conceptual
ms.date: 08/25/2019
ms.author: memildin
ms.openlocfilehash: b82eab9d20966ddd0678c9213bf25a14b5313f58
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73686457"
---
# <a name="threat-detection-for-the-azure-service-layer-in-azure-security-center"></a>Detección de amenazas para la capa de servicios de Azure en Azure Security Center

En este tema se presentan las alertas de Azure Security Center disponibles al supervisar las siguientes capas de servicios de Azure:

* [Capa de red de Azure](#network-layer)
* [Capa de administración de Azure (Azure Resource Manager) (versión preliminar)](#management-layer)

>[!NOTE]
>Los análisis siguientes se aplican a todos los tipos de recursos. Usan los datos de telemetría que Security Center proporciona al pulsar las fuentes internas de Azure.

## Capa de red de Azure<a name="network-layer"></a>

El análisis de la capa de red de Security Center se basa en [datos IPFIX](https://en.wikipedia.org/wiki/IP_Flow_Information_Export) de ejemplo, que son encabezados de paquete recopilados por los enrutadores principales de Azure. Sobre la base de esta fuente de datos, los modelos de Machine Learning de Security Center identifican y marcan actividades de tráfico malintencionadas. Para enriquecer direcciones IP, Security Center usa la base de datos de información sobre amenazas de Microsoft.

> [!div class="mx-tableFixed"]

|Alerta|DESCRIPCIÓN|
|---|---|
|**Actividad de red RDP saliente sospechosa**|El análisis de las muestras de tráfico ha detectado una comunicación saliente anómala del Protocolo de escritorio remoto (RDP) con origen en un recurso de su implementación. Esta actividad se considera anómala para este entorno. Podría indicar que el recurso está en peligro y ahora se usa para realizar un ataque por fuerza bruta en un punto de conexión RDP externo. Tenga en cuenta que este tipo de actividad puede dar lugar a que entidades externas marquen su dirección IP como malintencionada.|
|**Actividad de red RDP saliente sospechosa hacia varios destinos**|El análisis de las muestras de tráfico ha detectado una comunicación de RDP saliente anómala con origen en un recurso de su implementación con varios destinos. Esta actividad se considera anómala para este entorno. Podría indicar que el recurso está en peligro y ahora se usa para realizar un ataques por fuerza bruta en puntos de conexión RDP externos. Tenga en cuenta que este tipo de actividad puede dar lugar a que entidades externas marquen su dirección IP como malintencionada.|
|**Actividad de red de SSH saliente sospechosa**|El análisis de las muestras de tráfico ha detectado una comunicación saliente anómala de Secure Shell (SSH) con origen en un recurso de su implementación. Esta actividad se considera anómala para este entorno. Podría indicar que el recurso está en peligro y ahora se usa para realizar un ataque por fuerza bruta en un punto de conexión SSH externo. Tenga en cuenta que este tipo de actividad puede dar lugar a que entidades externas marquen su dirección IP como malintencionada.|
|**Actividad de red de SSH saliente sospechosa hacia varios destinos**|El análisis de las muestras de tráfico ha detectado una comunicación de SSH saliente anómala con origen en un recurso de su implementación con varios destinos. Esta actividad se considera anómala para este entorno. Podría indicar que el recurso está en peligro y ahora se usa para realizar ataques por fuerza bruta en puntos de conexión SSH externos. Tenga en cuenta que este tipo de actividad puede dar lugar a que entidades externas marquen su dirección IP como malintencionada.|
|**Actividad de red SSH entrante sospechosa procedente de varios orígenes**|El análisis de las muestras de tráfico ha detectado comunicaciones de SSH entrante anómala desde varios orígenes con destino en un recurso de su implementación. Varias direcciones IP únicas conectadas al recurso se consideran anómalas para este entorno. Esta actividad podría indicar un intento de ataque por fuerza bruta a la interfaz de SSH desde varios hosts (Botnet).|
|**Actividad de red SSH entrante sospechosa**|El análisis de las muestras de tráfico ha detectado una comunicación de SSH entrante anómala con destino en un recurso de su implementación. Para este entorno, se considera anómalo un número relativamente elevado de conexiones entrantes al recurso. Esta actividad podría indicar un intento de ataque por fuerza bruta a su interfaz de SSH.
|**Actividad de red RDP entrante sospechosa procedente de varios orígenes**|El análisis de las muestras de tráfico ha detectado comunicaciones de RDP entrante anómala desde varios orígenes con destino en un recurso de su implementación. Varias direcciones IP únicas conectadas al recurso se consideran anómalas para este entorno. Esta actividad podría indicar un intento de ataque por fuerza bruta a la interfaz de RDP desde varios hosts (Botnet).|
|**Actividad de red RDP entrante sospechosa**|El análisis de las muestras de tráfico ha detectado una comunicación de RDP entrante anómala con destino en un recurso de su implementación. Para este entorno, se considera anómalo un número relativamente elevado de conexiones entrantes al recurso. Esta actividad podría indicar un intento de ataque por fuerza bruta a su interfaz de SSH.|
|**Detectada comunicación de red con una dirección malintencionada**|El análisis de las muestras de tráfico de red ha detectado una comunicación desde un recurso de su implementación con un posible servidor de comando y control (C&C). Tenga en cuenta que este tipo de actividad puede dar lugar a que entidades externas marquen su dirección IP como malintencionada.|

Para comprender cómo Security Center puede usar señales relacionadas con la red para aplicar protección contra amenazas, consulte [Detecciones de DNS heurísticas en Azure Security Center](https://azure.microsoft.com/blog/heuristic-dns-detections-in-azure-security-center/).

>[!NOTE]
>En Azure Security Center, las alertas de detección de amenazas de la capa de red de Azure solo se generan en máquinas virtuales a las que se les ha asignado la misma dirección IP durante la hora en la que se ha producido una comunicación sospechosa. Esto se aplica a las máquinas virtuales, así como a las máquinas virtuales que se crean en la suscripción del cliente como parte de un servicio administrado (por ejemplo, AKS o Databricks).

## Capa de administración de Azure (Azure Resource Manager) (versión preliminar)<a name ="management-layer"></a>

>[!NOTE]
>La capa de protección de Security Center basada en Azure Resource Manager está actualmente en versión preliminar.

Security Center ofrece una capa adicional de protección al usar eventos de Azure Resource Manager, lo que se considera el plano de control de Azure. Al analizar los registros de Azure Resource Manager, Security Center detecta operaciones inusuales o potencialmente peligrosas en el entorno de suscripción de Azure.

> [!div class="mx-tableFixed"]

|Alerta|DESCRIPCIÓN|
|---|---|
|**Ejecución del kit de herramientas MicroBurst**|Se ha detectado la ejecución de un conocido kit de herramientas de reconocimiento de entornos de nube en su entorno. Un atacante (o evaluador de penetración) puede usar la herramienta [MicroBurst](https://github.com/NetSPI/MicroBurst) para asignar recursos de sus suscripciones, identificar configuraciones poco seguras y revelar información confidencial.|
|**Ejecución del kit de herramientas Azurite**|Se ha detectado la ejecución de un conocido kit de herramientas de reconocimiento de entornos de nube en su entorno. Un atacante (o evaluador de penetración) puede usar la herramienta [Azurite](https://github.com/mwrlabs/Azurite) para asignar recursos de sus suscripciones e identificar configuraciones poco seguras.|
|**Sesión de administración sospechosa con una cuenta inactiva**|El análisis de registros de actividad de suscripción ha detectado un comportamiento sospechoso. Una entidad de seguridad que no se ha usado durante un largo período de tiempo ahora está realizando acciones que pueden garantizar la persistencia de un atacante.|
|**Sesión de administración sospechosa con PowerShell**|El análisis de registros de actividad de suscripción ha detectado un comportamiento sospechoso. Una entidad de seguridad que no usa con frecuencia PowerShell para administrar el entorno de suscripción ahora usa PowerShell y realiza acciones que pueden garantizar la persistencia de un atacante.|
|**Uso de técnicas de persistencia avanzadas de Azure**|El análisis de registros de actividad de suscripción ha detectado un comportamiento sospechoso. Se han concedido entidades de identidad legitimizadas a roles personalizados. Esto puede provocar que el atacante consiga persistencia en un entorno de cliente de Azure.|
|**Actividad desde un país poco frecuente**|Se ha producido actividad desde una ubicación que ningún usuario de la organización ha visitado recientemente o nunca.<br/>Esta detección tiene en cuenta las ubicaciones de actividad anteriores para determinar las ubicaciones nuevas e infrecuentes. El motor de detección de anomalías almacena información sobre las ubicaciones anteriores utilizadas por los usuarios de la organización. 
|**Actividad desde direcciones IP anónimas**|Se ha detectado actividad de usuarios desde una dirección IP que se ha identificado como una dirección IP de proxy anónima. <br/>Estos servidores proxy los usan los usuarios que quieren ocultar la dirección IP del dispositivo y es posible que se usen con fines malintencionados. Esta detección usa un algoritmo de aprendizaje automático que reduce los falsos positivos, como las direcciones IP mal etiquetadas que otros usuarios de la organización usan ampliamente.|
|**Viaje imposible detectado**|Se han producido dos actividades de usuario (en una o varias sesiones) con origen en ubicaciones geográficamente distantes. Han tenido lugar en un período de tiempo más corto que el que tardaría el usuario en viajar de la primera ubicación a la segunda. Esto indica que otro usuario está usando las mismas credenciales. <br/>Esta detección usa un algoritmo de aprendizaje automático que omite los falsos positivos obvios que contribuyen a las condiciones de viaje imposible, como las VPN y las ubicaciones que otros usuarios de la organización usan con regularidad. La detección tiene un período de aprendizaje inicial de siete días, durante el cual aprende el patrón de actividad del nuevo usuario.|

>[!NOTE]
> Varios de los análisis anteriores se realizan mediante Microsoft Cloud App Security. Para beneficiarse de estos análisis, debe activar una licencia de Cloud App Security. Si tiene una licencia de Cloud App Security, estas alertas están habilitadas de forma predeterminada. Para deshabilitarlas:
>
> 1. En la hoja **Security Center**, seleccione **Directiva de seguridad**. Para la suscripción que quiere cambiar, elija **Editar la configuración**.
> 2. Seleccione **Detección de amenazas**.
> 3. En **Habilitar integraciones**, desactive la opción **Allow Microsoft Cloud App Security to access my data** (Permitir que Microsoft Cloud App Security acceda a mis datos) y seleccione **Guardar**.

>[!NOTE]
>Security Center almacena datos de clientes relacionados con la seguridad en la misma zona geográfica que su recurso. Si Microsoft aún no ha implementado Security Center en la zona geográfica del recurso, almacenará los datos en Estados Unidos. Cuando Cloud App Security esté habilitado, esta información se almacenará con arreglo a las reglas de ubicación geográfica de Cloud App Security. Para obtener más información, consulte [Almacenamiento de datos para servicios no regionales](https://azuredatacentermap.azurewebsites.net/).
