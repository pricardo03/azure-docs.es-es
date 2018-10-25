---
title: archivo de inclusión
description: archivo de inclusión
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 06/08/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 91a4a9ae1d3d84f1396adad07d1cda73ee3747c9
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2018
ms.locfileid: "49312454"
---
## <a name="what-happens-to-my-app-during-deployment"></a>¿Qué ocurre con mi aplicación durante la implementación?

Todos los métodos de implementación admitidos oficialmente tienen algo en común: realizan cambios en los archivos de la carpeta `/site/home/wwwroot` de la aplicación. Estos son los mismos archivos que se ejecutan en producción. Por tanto, se puede producir un error durante la implementación debido a archivos bloqueados, o puede que la aplicación de producción tenga un comportamiento impredecible durante la implementación debido a que no todos los archivos se actualizan simultáneamente. Hay varias formas de evitar estos problemas:

- Detenga la aplicación o habilite el modo sin conexión para la aplicación durante la implementación. Para más información, consulte [Dealing with locked files during deployment](https://github.com/projectkudu/kudu/wiki/Dealing-with-locked-files-during-deployment) (Tratamiento de archivos bloqueados durante la implementación).
- Realice la implementación en un [espacio de ensayo](../articles/app-service/web-sites-staged-publishing.md) con [intercambio automático](../articles/app-service/web-sites-staged-publishing.md#configure-auto-swap) habilitado. 
- Use [Run From Package](https://github.com/Azure/app-service-announcements/issues/84) en su lugar.
