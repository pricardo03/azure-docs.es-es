---
title: archivo de inclusión
description: archivo de inclusión
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 01/21/2019
ms.author: v-biyu
ms.custom: include file
ms.openlocfilehash: 8709956adee06e4e783ac5a7b317b2c4dec43e73
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60765657"
---
## <a name="what-happens-to-my-app-during-deployment"></a>¿Qué ocurre con mi aplicación durante la implementación?

Todos los métodos de implementación admitidos oficialmente tienen algo en común: realizan cambios en los archivos de la carpeta `/home/site/wwwroot` de la aplicación. Estos son los mismos archivos que se ejecutan en producción. Por tanto, se puede producir un error durante la implementación debido a archivos bloqueados, o puede que la aplicación de producción tenga un comportamiento impredecible durante la implementación debido a que no todos los archivos se actualizan simultáneamente. Hay varias formas de evitar estos problemas:

- Detenga la aplicación o habilite el modo sin conexión para la aplicación durante la implementación. Para más información, consulte [Dealing with locked files during deployment](https://github.com/projectkudu/kudu/wiki/Dealing-with-locked-files-during-deployment) (Tratamiento de archivos bloqueados durante la implementación).
- Realice la implementación en un [espacio de ensayo](../articles/app-service/deploy-staging-slots.md) con [intercambio automático](../articles/app-service/deploy-staging-slots.md#configure-auto-swap) habilitado. 
- Use [Run From Package](https://github.com/Azure/app-service-announcements/issues/84) en su lugar.
