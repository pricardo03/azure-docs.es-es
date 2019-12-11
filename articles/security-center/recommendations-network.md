---
title: Recomendaciones de Azure Security Center para las redes
description: En este artículo se enumeran las recomendaciones de seguridad de Azure Security Center que ayudan a proteger los recursos de red.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 47fa1f76-683d-4230-b4ed-d123fef9a3e8
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/29/2019
ms.author: memildin
ms.openlocfilehash: 1b8d84286d14949c007d1ece3d089a7606464aaf
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74781844"
---
# <a name="network-recommendations---reference-guide"></a>Guía de referencia sobre las recomendaciones de red

En este artículo se proporciona una lista completa de las recomendaciones que puede ver en Azure Security Center con respecto a la topología de la red y los puntos de conexión accesibles desde Internet.

Para una explicación de cómo buscarlos y resolverlos, consulte [esto](security-center-network-recommendations.md).

## <a name="network-recommendations"></a>Recomendaciones de red

|Nombre de la recomendación|DESCRIPCIÓN|severity|Puntuación segura|Tipo de recurso|
|----|----|----|----|----|----|
|Se deben habilitar los grupos de seguridad de red en el nivel de subred|Habilite los grupos de seguridad de red para controlar el acceso a la red de los recursos implementados en las subredes.|Alta, media|30|Subnet|
|Las máquinas virtuales deben estar asociadas a un grupo de seguridad de red|Habilite grupos de seguridad de red para controlar el acceso de red de las máquinas virtuales.|Alta, media|30|Máquina virtual|
|Se debe restringir el acceso a los grupos de seguridad de red permisivos con máquinas virtuales accesibles desde Internet|Proteja los grupos de seguridad de red de las máquinas virtuales accesibles desde Internet mediante la restricción del acceso de las reglas de permiso existentes.|Alto|20|Máquina virtual|
|Se deben proteger las reglas de las aplicaciones web en los NSG de IaaS|Proteja el grupo de seguridad de red (NSG) de las máquinas virtuales que ejecutan aplicaciones web con reglas de NSG que son demasiado permisivas con respecto a los puertos de la aplicación web.|Alto|20|Máquina virtual|
|Se debe restringir el acceso a App Services|Cambie la configuración de red para restringir el acceso a App Services y denegar el tráfico entrante desde intervalos demasiado amplios.|Alto|10|App Service|
|Se deben cerrar los puertos de administración en las máquinas virtuales|Proteja el grupo de seguridad de red de las máquinas virtuales para restringir el acceso a los puertos de administración.|Alto|10|Máquina virtual|
Se debe habilitar DDoS Protection estándar|Habilite el estándar del servicio de protección contra DDoS para proteger las redes virtuales que contienen aplicaciones con IP públicas. La protección contra DDoS permite mitigar los ataques volumétricos de red y protocolo.|Alto|10|Virtual network|
|El reenvío de IP en la máquina virtual debe estar deshabilitado|Deshabilite el reenvío de IP. Cuando el reenvío de IP está habilitado en el adaptador de red de una máquina virtual, la máquina puede recibir tráfico dirigido a otros destinos. El reenvío de IP rara vez es necesario (por ejemplo, cuando se usa la máquina virtual como una aplicación virtual de red) y, por lo tanto, el equipo de seguridad de red debe revisarlo.|Mediano|10|Máquina virtual|
|Acceso a la aplicación web solo a través de HTTPS|Habilite el acceso "Solo HTTPS" para aplicaciones web. El uso de HTTPS garantiza la autenticación del servicio y el servidor, y protege los datos en tránsito frente a ataques de intercepción de nivel de red.|Mediano|20|Aplicación web|
|El control de acceso de red Just-In-Time se debe aplicar en las máquinas virtuales|Aplique el control de acceso de máquina virtual (VM) Just-In-Time (JIT) para bloquear de forma permanente el acceso a los puertos seleccionados y permitir que los usuarios autorizados los abran mediante JIT y solo durante una periodo de tiempo limitado.|Alto|20|Máquina virtual|
|Function App solo debería ser accesible a través de HTTPS|Habilite el acceso "Solo HTTPS" para las aplicaciones de funciones. El uso de HTTPS garantiza la autenticación del servicio y el servidor, y protege los datos en tránsito frente a ataques de intercepción de nivel de red.|Mediano|20|Aplicación de función|
|Se debe habilitar la transferencia segura a las cuentas de almacenamiento|Habilite la transferencia segura para las cuentas de almacenamiento. La transferencia segura es una opción que obliga a la cuenta de almacenamiento a aceptar solamente solicitudes de conexiones seguras (HTTPS). El uso de HTTPS garantiza la autenticación entre el servidor y el servicio, y protege los datos en tránsito de ataques de nivel de red, como "man in-the-middle", interceptación y secuestro de sesión.|Alto|20|Cuenta de almacenamiento|


## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre las recomendaciones que se aplican a otros tipos de recursos de Azure, consulte los siguientes artículos:

* [Supervisión de la identidad y el acceso](security-center-identity-access.md)
* [Protección de las aplicaciones y las máquinas](security-center-virtual-machine-protection.md)
* [Protección del servicio Azure SQL](security-center-sql-service-recommendations.md)

