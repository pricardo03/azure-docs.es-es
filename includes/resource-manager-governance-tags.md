---
title: archivo de inclusión
description: archivo de inclusión
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 06/18/2019
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 5358fe387d2a371d96d46d8546ce0f20b47ca54b
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67206489"
---
Aplicará etiquetas a los recursos de Azure que proporcionan metadatos para organizarlos de forma lógica en una taxonomía. Cada etiqueta consta de un nombre y un par de valores. Por ejemplo, puede aplicar el nombre "Environment" y el valor "Production" a todos los recursos en producción.

Después de aplicar las etiquetas, puede recuperar todos los recursos de la suscripción que tengan ese nombre y valor de etiqueta. Las etiquetas le permiten recuperar los recursos relacionados que se encuentran en distintos grupos de recursos. Este enfoque puede resultar útil si necesita organizar recursos para facturación o administración.

La taxonomía debe considerar una estrategia de autoservicio de etiquetado de metadatos además de una estrategia de autoetiquetado para reducir la carga sobre los usuarios y aumentar la precisión.

Se aplican las siguientes limitaciones a las etiquetas:

* No todos los tipos de recursos admiten etiquetas. Para determinar si se puede aplicar una etiqueta a un tipo de recurso determinado, consulte [Tag support for Azure resources](../articles/azure-resource-manager/tag-support.md) (Compatibilidad con etiquetas para los recursos de Azure).
* Cada recurso o grupo de recursos puede tener un máximo de 15 pares de nombre/valor de etiqueta. Esta limitación solo se aplica a las etiquetas que se aplican directamente al recurso o grupo de recursos. Un grupo de recursos puede contener muchos recursos con 15 pares de clave/valor de etiqueta cada uno. Si necesita asociar más de 15 valores a un recurso, utilice una cadena JSON como valor de la etiqueta. La cadena JSON puede contener muchos valores que se aplican a un sol nombre de etiqueta. En este artículo se muestra un ejemplo de asignación de una cadena JSON a la etiqueta.
* El nombre de etiqueta está limitado a 512 caracteres y el valor de la etiqueta, a 256. En las cuentas de almacenamiento, el nombre de etiqueta se limita a 128 caracteres y el valor de la etiqueta, a 256.
* Virtual Machines y Virtual Machine Scale Sets están limitados a un total de 2048 caracteres en todos los valores y nombres de etiqueta. Las VM generalizadas no admiten etiquetas.
* Los recursos de un grupo de recursos no heredan las etiquetas aplicadas a este.
* No se pueden aplicar etiquetas a recursos clásicos como Cloud Services.
* Los nombres de etiqueta no pueden contener estos caracteres: `<`, `>`, `%`, `&`, `\`, `?`, `/`
