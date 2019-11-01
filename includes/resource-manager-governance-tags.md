---
title: archivo de inclusión
description: archivo de inclusión
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 10/30/2019
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: c141e67157f3ec17d475062ec76406ec765c4f50
ms.sourcegitcommit: fa5ce8924930f56bcac17f6c2a359c1a5b9660c9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2019
ms.locfileid: "73199145"
---
Aplicará etiquetas a los recursos de Azure que proporcionan metadatos para organizarlos de forma lógica en una taxonomía. Cada etiqueta consta de un nombre y un par de valores. Por ejemplo, puede aplicar el nombre "Environment" y el valor "Production" a todos los recursos en producción.

Después de aplicar las etiquetas, puede recuperar todos los recursos de la suscripción que tengan ese nombre y valor de etiqueta. Las etiquetas le permiten recuperar los recursos relacionados que se encuentran en distintos grupos de recursos. Este enfoque puede resultar útil si necesita organizar recursos para facturación o administración.

La taxonomía debe considerar una estrategia de autoservicio de etiquetado de metadatos además de una estrategia de autoetiquetado para reducir la carga sobre los usuarios y aumentar la precisión.

Se aplican las siguientes limitaciones a las etiquetas:

* No todos los tipos de recursos admiten etiquetas. Para determinar si se puede aplicar una etiqueta a un tipo de recurso determinado, consulte [Tag support for Azure resources](../articles/azure-resource-manager/tag-support.md) (Compatibilidad con etiquetas para los recursos de Azure).
* Cada recurso o grupo de recursos puede tener un máximo de cincuenta pares de nombre/valor de etiqueta. Si necesita aplicar más etiquetas que el número máximo permitido, use una cadena JSON para el valor de etiqueta. La cadena JSON puede contener muchos valores que se aplican a un sol nombre de etiqueta. Un grupo de recursos puede contener muchos recursos con cincuenta pares de clave/valor de etiqueta cada uno.
* El nombre de etiqueta está limitado a 512 caracteres y el valor de la etiqueta, a 256. En las cuentas de almacenamiento, el nombre de etiqueta se limita a 128 caracteres y el valor de la etiqueta, a 256.
* Las VM generalizadas no admiten etiquetas.
* Los recursos de un grupo de recursos no heredan las etiquetas aplicadas a este.
* No se pueden aplicar etiquetas a recursos clásicos como Cloud Services.
* Los nombres de etiqueta no pueden contener estos caracteres: `<`, `>`, `%`, `&`, `\`, `?`, `/`

   > [!NOTE]
   > Actualmente las zonas de Azure DNS y los servicios de Traffic Manager tampoco permiten el uso de espacios en la etiqueta. 
