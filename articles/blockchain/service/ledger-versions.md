---
title: Azure Blockchain Service admite las versiones de libro de contabilidad, revisión y actualización
description: Información general de las versiones compatibles de libros de contabilidad en el servicio de Azure Blockchain, incluidas las directivas con respecto a los sistemas de aplicación de revisiones y administrado por el sistema y las actualizaciones administradas por el usuario.
services: azure-blockchain
keywords: cadena de bloques
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: janders
manager: femila
ms.openlocfilehash: 53f65ec91a1e0f1e5a6322f0125bf83cd3e400b2
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399099"
---
# <a name="supported-azure-blockchain-service-ledger-versions"></a>Versiones admitidas de libro de contabilidad de servicio de Azure Blockchain

Azure Blockchain Service usa basado en Ethereum [quórum](https://www.goquorum.com/developers) contabilidad diseñada para el procesamiento de transacciones privadas dentro de un grupo de participantes conocidos, identificado como un consorcio de Azure Blockchain Service.

Actualmente, Azure Blockchain Service admite [quórum versión 2.2.1](https://github.com/jpmorganchase/quorum/releases/tag/v2.2.1) y [Administrador de transacciones Tessera](https://github.com/jpmorganchase/tessera).

## <a name="managing-updates-and-upgrades"></a>Administración de actualizaciones

Control de versiones en el quórum se realiza a través de un principal, secundaria y las revisiones publicadas. Por ejemplo, si la versión de quórum es 2.0.1, tipo de versión se clasificarían como sigue:

|Major | Minor  | Revisión  |
| :--- | :----- | :----- |
| 2 | 0 | 1 | 

Azure Blockchain Service actualiza automáticamente las versiones de revisiones de quórum a los miembros existentes de ejecución dentro de 30 días de pone a disposición de quórum.

## <a name="availability-of-new-ledger-versions"></a>Disponibilidad de nuevas versiones de libro de contabilidad

Azure Blockchain Service proporciona las últimas versiones principales y secundarias del libro de contabilidad de quórum 60 días después de que se va a obtener del fabricante del quórum. Un máximo de cuatro versiones secundarias se proporcionan para que consorcios elegir al aprovisionar un nuevo miembro y un consorcio. Versión de la actualización a una versión principal o secundaria no se admite actualmente.

## <a name="next-steps"></a>Pasos siguientes

[Límites de servicio de Azure Blockchain](limits.md)
