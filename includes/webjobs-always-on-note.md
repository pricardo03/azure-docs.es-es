---
title: archivo de inclusión
description: archivo de inclusión
services: app-service
author: ggailey777
ms.service: app-service
ms.topic: include
ms.date: 02/19/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 14f2e663e3db81684a73c4ea093ed0403cbb09ed
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186473"
---
> [!NOTE]
> Una aplicación web puede agotar el tiempo de espera después de 20 minutos de inactividad. Solo las solicitudes a la aplicación web real restablecen el temporizador. Al ver la configuración de la aplicación en Azure Portal o realizar solicitudes en el sitio de herramientas avanzadas (`https://<app_name>.scm.azurewebsites.net`), no se restablece el temporizador. Si la aplicación ejecuta WebJobs continuos o programados, habilite **AlwaysOn** para asegurarse de que los WebJobs se ejecuten de manera confiable. Esta característica solo está disponible en los [planes de tarifa](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) Básico, Estándar y Premium.
