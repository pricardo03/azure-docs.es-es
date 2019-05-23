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
ms.openlocfilehash: aa6f18d4f667862687083c5db3679ce9d8e188cd
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66137419"
---
En la _ventana del terminal local_, agregue una instancia remota de Azure al repositorio de Git local. Reemplace _&lt;deploymentLocalGitUrl-from-create-step>_ por la dirección URL del repositorio de Git remoto que guardó en [Creación de una aplicación web](#create-a-web-app).

```bash
git remote add azure <deploymentLocalGitUrl-from-create-step>
```

Inserte en la instancia remota de Azure para implementar la aplicación con el comando siguiente. Cuando el Administrador de credenciales de Git le solicite las credenciales, asegúrese de que especifica las credenciales que creó en Configuración de un usuario de implementación, no las que usó para iniciar sesión en Azure Portal.

```bash
git push azure master
```

Este comando puede tardar varios minutos en ejecutarse. Durante la ejecución, muestra información similar a la del ejemplo siguiente:
