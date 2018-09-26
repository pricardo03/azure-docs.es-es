---
title: FQDN de infraestructura para Azure Firewall
description: Obtenga información sobre los FQDN de infraestructura en Azure Firewall
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 9/24/2018
ms.author: victorh
ms.openlocfilehash: 1369a82829b2c80768d746ba92daf2482c1fd7f8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46994146"
---
# <a name="infrastructure-fqdns"></a>FQDN de infraestructura

Azure Firewall incluye una colección de reglas integradas para FQDN de infraestructura que están permitidos de forma predeterminada. Estos FQDN son específicos para la plataforma y no se pueden usar para otros fines. 

En la colección integrada de reglas se incluyen los siguientes servicios:

- Acceso de proceso al repositorio de imágenes de la plataforma (PIR) de almacenamiento
- Acceso al almacenamiento de estado de los discos administrados
- Azure Diagnostics y registro (MDS)
- Azure Active Directory

## <a name="overriding"></a>Invalidación 

Puede invalidar esta colección integrada de reglas de infraestructura creando una colección de reglas de aplicación Denegar todo que se procese en último lugar. Se procesará siempre antes que la colección de reglas de infraestructura. Cualquier cosa que no esté en la colección de reglas de infraestructura se denegará de forma predeterminada.

## <a name="next-steps"></a>Pasos siguientes

- Consulte el tutorial [Implementación y configuración de Azure Firewall](tutorial-firewall-deploy-portal.md).