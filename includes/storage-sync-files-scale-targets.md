---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: wmgries
ms.service: storage
ms.topic: include
ms.date: 05/05/2019
ms.author: wgries
ms.custom: include file
ms.openlocfilehash: e7aa2b4389fe60eed80b15aff04d6f7fcbc7b013
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "68968935"
---
| Recurso | Destino | Límite máximo |
|----------|--------------|------------|
| Servicios de sincronización de almacenamiento por región | 20 servicios de sincronización de almacenamiento | Sí |
| Grupos de sincronización por servicio de sincronización de almacenamiento | 100 grupos de sincronización | Sí |
| Servidores registrados por servicio de sincronización de almacenamiento | 99 servidores | Sí |
| Puntos de conexión en la nube por grupo de sincronización | 1 punto de conexión en la nube | Sí |
| Puntos de conexión del servidor por grupo de sincronización | 50 puntos de conexión del servidor | Sin |
| Puntos de conexión del servidor por servidor | 30 puntos de conexión de servidor | Sí |
| Objetos del sistema de archivos (archivos y directorios) por grupo de sincronización | 50 millones de objetos | Sin |
| Número máximo de objetos del sistema de archivos (archivos y directorios) en un directorio | 5 millones de objetos | Sí |
| Tamaño máximo del descriptor de seguridad de (archivos y directorios) del objeto | 64 KiB | Sí |
| Tamaño de archivo | 100 GiB | Sin |
| Tamaño mínimo de un archivo que se va a organizar en niveles | 64 KiB | Sí |
| Sesiones sincronizadas simultáneas | Agente de V4 y versiones posteriores: el límite varía en función de los recursos del sistema disponibles. <BR> Agente V3: dos sesiones sincronizadas activas por procesador o un máximo de ocho sesiones sincronizadas activas por servidor. | Sí

> [!Note]  
> Un punto de conexión de Azure File Sync puede escalar verticalmente hasta el tamaño de un recurso compartido de archivos de Azure. Si se alcanza el límite de tamaño de recurso compartido de archivos de Azure, la sincronización no podrá funcionar.
