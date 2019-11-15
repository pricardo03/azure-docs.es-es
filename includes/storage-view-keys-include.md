---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/06/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: e3b3d944508a4261b78def0b3bee13f7395a8bf0
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2019
ms.locfileid: "73749002"
---
Para ver y copiar la cadena de conexión o las claves de acceso de la cuenta de almacenamiento desde Azure Portal:

1. Acceda a [Azure Portal](https://portal.azure.com).
2. Busque su cuenta de almacenamiento.
3. En la sección **Configuración** de la información general de la cuenta de almacenamiento, seleccione **Claves de acceso**. Aparecen las claves de acceso de la cuenta, así como la cadena de conexión completa de cada clave.
4. Busque el valor de **Clave** en **key1** y haga clic en el botón **Copiar** para copiar la clave de la cuenta.
5. Como alternativa, puede copiar la cadena de conexión completa. Busque el valor de **Cadena de conexión** en **key1**y haga clic en el botón **Copiar** para copiar la cadena de conexión.

    ![Captura de pantalla que muestra cómo ver las claves de acceso en Azure Portal](media/storage-view-keys-include/portal-connection-string.png)

Puede utilizar cualquiera de las dos claves para acceder a Azure Storage, pero en general es recomendable usar la primera clave y reservar el uso de la segunda clave para cuando se roten las claves.
