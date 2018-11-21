---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: wmgries
ms.service: storage
ms.topic: include
ms.date: 07/18/2018
ms.author: wgries
ms.custom: include file
ms.openlocfilehash: 03fe587ede297ac7dea90b7a5fb2d5323f60659e
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51628208"
---
| Recurso | Destino | Límite máximo |
|----------|--------------|------------|
| Servicios de sincronización de almacenamiento por suscripción | 15 servicios de sincronización de almacenamiento por región | Sin  |
| Grupos de sincronización por servicio de sincronización de almacenamiento | 100 grupos de sincronización | SÍ |
| Servidores registrados por servicio de sincronización de almacenamiento | 99 servidores | SÍ |
| Puntos de conexión en la nube por grupo de sincronización | 1 punto de conexión en la nube | SÍ |
| Puntos de conexión del servidor por grupo de sincronización | 50 puntos de conexión del servidor | Sin  |
| Puntos de conexión del servidor por servidor | 33-99 puntos de conexión del servidor | Sí, pero varía en función de la configuración (CPU, memoria, volúmenes, renovación de archivo, número de archivos, etc.) |
| Tamaño de punto de conexión | 4 TiB | Sin  |
| Objetos del sistema de archivos (archivos y directorios) por grupo de sincronización | 25 millones de objetos | Sin  |
| Número máximo de objetos del sistema de archivos (archivos y directorios) en un directorio | 200 000 de objetos | SÍ |
| Longitud máxima del nombre de objeto (archivos y directorios) | 255 caracteres | SÍ |
| Tamaño máximo del descriptor de seguridad de (archivos y directorios) del objeto | 4 KiB | SÍ |
| Tamaño de archivo | 100 GiB | Sin  |
| Tamaño mínimo de un archivo que se va a organizar en niveles | 64 KiB | SÍ |
| Sesiones sincronizadas simultáneas | Agente V4: el límite varía en función de los recursos del sistema disponibles. <BR> Agente V3: 2 sesiones sincronizadas activas por procesador o un máximo de 8 sesiones sincronizadas activas por servidor | SÍ
