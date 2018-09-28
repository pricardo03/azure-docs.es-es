---
title: Error al crear un clúster en el Explorador de datos de Azure
description: En este artículo se describen los pasos de solución de problemas para crear un clúster en el Explorador de datos de Azure.
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 6009953ece78eefd52fca9f12e3db80a6d2cc3eb
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46953215"
---
# <a name="troubleshoot-failure-to-create-a-cluster-in-azure-data-explorer"></a>Solución de problemas: Error al crear un clúster en el Explorador de datos de Azure

En el improbable caso de que la creación de un clúster en el Explorador de datos de Azure produzca un error, siga estos pasos.

1. Asegúrese de que tiene los permisos adecuados. Para crear un clúster, debe ser miembro del rol *colaborador* o *propietario* en la suscripción de Azure. Si es necesario, hable con su administrador de suscripciones para que pueda agregarle el rol adecuado.

1. Asegúrese de que no haya ningún error de validación relacionado con el nombre del clúster que ha escrito en **Crear clúster** en Azure Portal.

1. Compruebe el [panel de mantenimiento de servicios de Azure](https://azure.microsoft.com/status/>). Busque el estado del Explorador de datos de Azure en la región donde intenta crear el clúster.

    Si el estado no es **Bueno** (marca de verificación verde), pruebe a crear el clúster una vez que mejore el estado.

1. Si aún necesita ayuda para solucionar el problema, abra una solicitud de soporte técnico en [Azure Portal](https://portal.azure.com).