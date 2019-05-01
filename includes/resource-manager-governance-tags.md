---
title: archivo de inclusión
description: archivo de inclusión
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 11/20/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: c426df2293cfb2d8ba4dc02e8fc5519c3d822168
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2019
ms.locfileid: "64868760"
---
Aplicará etiquetas a los recursos de Azure que proporcionan metadatos para organizarlos de forma lógica en una taxonomía. Cada etiqueta consta de un nombre y un par de valores. Por ejemplo, puede aplicar el nombre "Environment" y el valor "Production" a todos los recursos en producción.

Después de aplicar las etiquetas, puede recuperar todos los recursos de la suscripción que tengan ese nombre y valor de etiqueta. Las etiquetas le permiten recuperar los recursos relacionados que se encuentran en distintos grupos de recursos. Este enfoque puede resultar útil si necesita organizar recursos para facturación o administración.

La taxonomía debe considerar una estrategia de autoservicio de etiquetado de metadatos además de una estrategia de autoetiquetado para reducir la carga sobre los usuarios y aumentar la precisión.

Se aplican las siguientes limitaciones a las etiquetas:

* No todos los tipos de recursos admiten etiquetas. Para determinar si se puede aplicar una etiqueta a un tipo de recurso determinado, consulte [Tag support for Azure resources](../articles/azure-resource-manager/tag-support.md) (Compatibilidad con etiquetas para los recursos de Azure).
* Cada recurso o grupo de recursos puede tener un máximo de 15 pares de nombre/valor de etiqueta. Esta limitación solo se aplica a las etiquetas que se aplican directamente al recurso o grupo de recursos. Un grupo de recursos puede contener muchos recursos con 15 pares de clave/valor de etiqueta cada uno. Si necesita asociar más de 15 valores a un recurso, utilice una cadena JSON como valor de la etiqueta. La cadena JSON puede contener muchos valores que se aplican a un sol nombre de etiqueta. En este artículo se muestra un ejemplo de asignación de una cadena JSON a la etiqueta.
* El nombre de etiqueta está limitado a 512 caracteres y el valor de la etiqueta, a 256. En las cuentas de almacenamiento, el nombre de etiqueta se limita a 128 caracteres y el valor de la etiqueta, a 256.
* Las máquinas virtuales y conjuntos de escalado de máquinas virtuales están limitados a un total de 2.048 caracteres para todos los valores y nombres de etiqueta.
* Los recursos de un grupo de recursos no heredan las etiquetas aplicadas a este.
* No se pueden aplicar etiquetas a recursos clásicos como Cloud Services.
* Los nombres de etiqueta no pueden contener estos caracteres: `<`, `>`, `%`, `&`, `\`, `?`, `/`
