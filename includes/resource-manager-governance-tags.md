---
title: archivo de inclusión
description: archivo de inclusión
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 07/11/2019
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 099bca7483100da1a4ee2f8f10057c416ad145b0
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67841584"
---
Aplicará etiquetas a los recursos de Azure que proporcionan metadatos para organizarlos de forma lógica en una taxonomía. Cada etiqueta consta de un nombre y un par de valores. Por ejemplo, puede aplicar el nombre "Environment" y el valor "Production" a todos los recursos en producción.

Después de aplicar las etiquetas, puede recuperar todos los recursos de la suscripción que tengan ese nombre y valor de etiqueta. Las etiquetas le permiten recuperar los recursos relacionados que se encuentran en distintos grupos de recursos. Este enfoque puede resultar útil si necesita organizar recursos para facturación o administración.

La taxonomía debe considerar una estrategia de autoservicio de etiquetado de metadatos además de una estrategia de autoetiquetado para reducir la carga sobre los usuarios y aumentar la precisión.

Se aplican las siguientes limitaciones a las etiquetas:

* No todos los tipos de recursos admiten etiquetas. Para determinar si se puede aplicar una etiqueta a un tipo de recurso determinado, consulte [Tag support for Azure resources](../articles/azure-resource-manager/tag-support.md) (Compatibilidad con etiquetas para los recursos de Azure).
* Cada recurso o grupo de recursos puede tener un máximo de cincuenta pares de nombre/valor de etiqueta. Actualmente, las cuentas de almacenamiento solo admiten quince etiquetas, pero ese límite se elevará a cincuenta en una versión futura. Si necesita aplicar más etiquetas que el número máximo permitido, use una cadena JSON para el valor de etiqueta. La cadena JSON puede contener muchos valores que se aplican a un sol nombre de etiqueta. Un grupo de recursos puede contener muchos recursos con cincuenta pares de clave/valor de etiqueta cada uno.
* El nombre de etiqueta está limitado a 512 caracteres y el valor de la etiqueta, a 256. En las cuentas de almacenamiento, el nombre de etiqueta se limita a 128 caracteres y el valor de la etiqueta, a 256.
* Las VM generalizadas no admiten etiquetas.
* Los recursos de un grupo de recursos no heredan las etiquetas aplicadas a este.
* No se pueden aplicar etiquetas a recursos clásicos como Cloud Services.
* Los nombres de etiqueta no pueden contener estos caracteres: `<`, `>`, `%`, `&`, `\`, `?`, `/`
