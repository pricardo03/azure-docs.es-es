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
ms.openlocfilehash: 63c9a8b9e266dacbb0fb6faba50fb44ac9a4b46e
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027891"
---
# <a name="supported-azure-blockchain-service-ledger-versions"></a>Versiones admitidas de libro de contabilidad de servicio de Azure Blockchain

Azure Blockchain Service usa basado en Ethereum [quórum](https://github.com/jpmorganchase/quorum/wiki) contabilidad diseñada para el procesamiento de transacciones privadas dentro de un grupo de participantes conocidos, identificado como un consorcio de Azure Blockchain Service.

Actualmente, Azure Blockchain Service admite [quórum versión 2.2.1](https://github.com/jpmorganchase/quorum/releases/tag/v2.2.1) y [Administrador de transacciones Tessera](https://github.com/jpmorganchase/tessera).

## <a name="managing-updates-and-upgrades"></a>Administración de actualizaciones

Control de versiones en el quórum se realiza a través de una versión principal, versión principal y una versión secundaria. Por ejemplo, si el quórum versión 2.0.1, 2 es una versión principal, 0 es una versión principal y 1 es la versión secundaria. El servicio aplica revisiones automáticamente a las versiones de poca importancia del libro de contabilidad. Actualmente, no se admiten las versiones principales y principales el punto de actualización.

## <a name="availability-of-new-ledger-versions"></a>Disponibilidad de nuevas versiones de libro de contabilidad

Azure Blockchain Service proporciona las últimas versiones de libro de contabilidad 60 días después de que se va a obtener del fabricante del libro de contabilidad. Un máximo de cuatro versiones principales se proporcionan para que consorcios elegir al aprovisionar un nuevo miembro y un consorcio.

## <a name="next-steps"></a>Pasos siguientes

[Límites de servicio de Azure Blockchain](limits.md)
