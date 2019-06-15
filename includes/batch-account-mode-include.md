---
title: archivo de inclusión
description: archivo de inclusión
services: batch
author: dlepow
ms.service: batch
ms.topic: include
ms.date: 05/04/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: b5bb5da2b68bae36353b81a04993814b395ce67b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66127563"
---
> [!NOTE]
> Al crear una cuenta de Batch, puede elegir entre dos modos de *asignación de grupo*: **suscripción de usuario** y **servicio Batch**. En la mayoría de los casos, debe usar el modo predeterminado servicio Batch, en el que los grupos se asignan en segundo plano en las suscripciones administradas por Azure. En el modo de suscripción de usuario alternativo, tanto las máquinas virtuales de Batch como otros recursos se crean directamente en su suscripción cuando se crea un grupo. El modo de suscripción de usuario es necesario si quiere crear grupos de [Azure Reserved Virtual Machine Instances](https://azure.microsoft.com/pricing/reserved-vm-instances/). Para crear una cuenta de Batch en modo de suscripción de usuario, también debe registrar su suscripción a Azure Batch y asociar la cuenta a una instancia de Azure Key Vault.