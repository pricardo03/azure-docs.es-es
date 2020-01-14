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
ms.openlocfilehash: d4033989830323856ac14ed06eea7df74806f128
ms.sourcegitcommit: ff9688050000593146b509a5da18fbf64e24fbeb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2020
ms.locfileid: "75665706"
---
# <a name="threat-detection-for-the-azure-service-layer-in-azure-security-center"></a>Detección de amenazas para la capa de servicios de Azure en Azure Security Center

En este tema se presentan las alertas de Azure Security Center disponibles al supervisar las siguientes capas de servicios de Azure:

* [Capa de red de Azure](#network-layer)
* [Capa de administración de Azure (Azure Resource Manager) (versión preliminar)](#management-layer)
* [Azure Key Vault](#azure-keyvault)

## Capa de red de Azure<a name="network-layer"></a>

El análisis de la capa de red de Security Center se basa en [datos IPFIX](https://en.wikipedia.org/wiki/IP_Flow_Information_Export) de ejemplo, que son encabezados de paquete recopilados por los enrutadores principales de Azure. En función de esta fuente de distribución de datos, Security Center utiliza modelos de Machine Learning para identificar y marcar actividades de tráfico malintencionado. Security Center también utiliza la base de datos de Microsoft Threat Intelligence para enriquecer las direcciones IP.

Algunas configuraciones de red pueden restringir la Security Center de la generación de alertas en una actividad de red sospechosa. Para que Security Center genere alertas de red, asegúrese de que:

- La máquina virtual tenga una dirección IP pública (o se encuentre en un equilibrador de carga con una dirección IP pública).

- El tráfico de salida de red de la máquina virtual no esté bloqueado por una solución de IDS externa.

- A la máquina virtual se le haya asignado la misma dirección IP durante toda la hora en la que se produjo la comunicación sospechosa. Esto también se aplica a las máquinas virtuales creadas como parte de un servicio administrado (p. ej., AKS, Databricks).

Para obtener una lista de las alertas de nivel de red de Azure, consulte la [tabla de referencia de alertas](alerts-reference.md#alerts-azurenetlayer).

Para obtener detalles sobre cómo Security Center pueden usar señales relacionadas con la red para aplicar la protección contra amenazas, consulte [Detecciones de DNS heurísticas en Security Center](https://azure.microsoft.com/blog/heuristic-dns-detections-in-azure-security-center/).


## Capa de administración de Azure (Azure Resource Manager) (versión preliminar)<a name ="management-layer"></a>

La capa de protección de Security Center basada en Azure Resource Manager está actualmente en versión preliminar.

Security Center ofrece una capa adicional de protección al usar eventos de Azure Resource Manager, lo que se considera el plano de control de Azure. Al analizar los registros de Azure Resource Manager, Security Center detecta operaciones inusuales o potencialmente peligrosas en el entorno de suscripción de Azure.

Para obtener una lista de las alertas de Azure Resource Manager (versión preliminar), consulte la [Tabla de referencia de alertas](alerts-reference.md#alerts-azureresourceman).



>[!NOTE]
> Varios de los análisis anteriores se realizan mediante Microsoft Cloud App Security. Para beneficiarse de estos análisis, debe activar una licencia de Cloud App Security. Si tiene una licencia de Cloud App Security, estas alertas están habilitadas de forma predeterminada. Para deshabilitarlas:
>
> 1. En la hoja **Security Center**, seleccione **Directiva de seguridad**. Para la suscripción que quiere cambiar, elija **Editar la configuración**.
> 2. Seleccione **Detección de amenazas**.
> 3. En **Habilitar integraciones**, desactive la opción **Allow Microsoft Cloud App Security to access my data** (Permitir que Microsoft Cloud App Security acceda a mis datos) y seleccione **Guardar**.

>[!NOTE]
>Security Center almacena datos de clientes relacionados con la seguridad en la misma zona geográfica que su recurso. Si Microsoft aún no ha implementado Security Center en la zona geográfica del recurso, almacenará los datos en Estados Unidos. Cuando Cloud App Security esté habilitado, esta información se almacenará con arreglo a las reglas de ubicación geográfica de Cloud App Security. Para obtener más información, consulte [Almacenamiento de datos para servicios no regionales](https://azuredatacentermap.azurewebsites.net/).

## Azure Key Vault (versión preliminar)<a name="azure-keyvault"></a>

Azure Key Vault es un servicio en la nube que protege las claves de cifrado y los secretos, como certificados, cadenas de conexión y contraseñas. 

En Azure Security Center se incluye protección frente a amenazas avanzada, nativa de Azure, para Azure Key Vault que proporciona una capa adicional de inteligencia de seguridad. Security Center detecta intentos inusuales y potencialmente perjudiciales de acceder o explotar las cuentas de Key Vault. Esta capa de protección le permite afrontar las amenazas sin ser un experto en seguridad y sin la necesidad de administrar sistemas de supervisión de la seguridad de terceros.  

Cuando se producen actividades anómalas, Security Center muestra alertas y, opcionalmente, las envía por correo electrónico a los administradores de suscripciones. Dichas alertas incluyen detalles acerca de cualquier actividad sospechosa y recomendaciones acerca de cómo investigar amenazas y mitigarlas. 

> [!NOTE]
> Este servicio no está disponible actualmente en Azure Government ni en las regiones de nubes soberanas.

Para obtener una lista de las alertas de Azure Key Vault, consulte la [Tabla de referencia de alertas](alerts-reference.md#alerts-azurekv).
