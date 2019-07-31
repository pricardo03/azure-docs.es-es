---
title: archivo de inclusión
description: archivo de inclusión
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 06/12/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 93332002cd2ac513d125e0f9eb75dff4a2d8760c
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67836835"
---
## <a name="what-happens-to-my-app-during-deployment"></a>¿Qué ocurre con mi aplicación durante la implementación?

Todos los métodos de implementación admitidos oficialmente realizan cambios en los archivos de la carpeta `/home/site/wwwroot` de la aplicación. Estos archivos son los mismos que se ejecutan en producción. Por lo tanto, se puede producir un error en la implementación debido a archivos bloqueados. Es posible que la aplicación en producción también se comporte de forma impredecible durante la implementación, ya que no todos los archivos se actualizan al mismo tiempo. Hay varias formas de evitar estos problemas:

- Detenga la aplicación o habilite el modo sin conexión para la aplicación durante la implementación. Para más información, consulte [Deal with locked files during deployment](https://github.com/projectkudu/kudu/wiki/Dealing-with-locked-files-during-deployment) (Tratamiento de archivos bloqueados durante la implementación).
- Realice la implementación en un [espacio de ensayo](../articles/app-service/deploy-staging-slots.md) con [intercambio automático](../articles/app-service/deploy-staging-slots.md#configure-auto-swap) habilitado. 
- Use [Ejecutar desde un paquete](https://github.com/Azure/app-service-announcements/issues/84) en lugar de la implementación continua.
