---
title: archivo de inclusión
description: archivo de inclusión
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 9f865897ee478f25a44fe876d44aec253e84eb62
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/22/2018
ms.locfileid: "36319716"
---
En la _ventana del terminal local_, agregue una instancia remota de Azure al repositorio de Git local. Reemplace _&lt;deploymentLocalGitUrl-from-create-step>_ por la dirección URL del repositorio de Git remoto que guardó en [Creación de una aplicación web](#create).

```bash
git remote add azure <deploymentLocalGitUrl-from-create-step>
```

Inserte en la instancia remota de Azure para implementar la aplicación con el comando siguiente. Cuando el administrador de credenciales de Git le solicite las credenciales, asegúrese de que especifica las credenciales que creó en [Configuración de un usuario de implementación](#configure-a-deployment-user), no las que usó para iniciar sesión en Azure Portal.

```bash
git push azure master
```

Este comando puede tardar varios minutos en ejecutarse. Durante la ejecución, muestra información similar a la del ejemplo siguiente:
