---
title: archivo de inclusión
description: archivo de inclusión
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 08/10/2018
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: 2b61cc8c5c448c28e96b06afa3556688a82567ed
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2019
ms.locfileid: "64866808"
---
### <a name="delete-local-resources"></a>Eliminación de recursos locales

Si desea eliminar el entorno de ejecución de Azure IoT Edge y los recursos relacionados del dispositivo, use los comandos adecuados para el sistema operativo del dispositivo. 

#### <a name="windows"></a> Windows

Desinstale el entorno de ejecución de IoT Edge.

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Uninstall-SecurityDaemon
   ```

Cuando se quita el entorno de ejecución de Azure IoT Edge, los contenedores que se han creado se detienen, pero siguen existiendo en el dispositivo. Vea todos los contenedores.

   ```powershell
   docker ps -a
   ```

Elimine los contenedores del entorno de ejecución que se crearon en el dispositivo.

   ```powershell
   docker rm -f edgeHub
   docker rm -f edgeAgent
   ```

Elimine los contenedores adicionales que se muestran en la salida `docker ps` haciendo referencia a los nombres de los contenedores. 

#### <a name="linux"></a>Linux

Quite el entorno de ejecución de Azure IoT Edge.

   ```bash
   sudo apt-get remove --purge iotedge
   ```

Cuando se quita el entorno de ejecución de Azure IoT Edge, los contenedores que se han creado se detienen, pero siguen existiendo en el dispositivo. Vea todos los contenedores.

   ```bash
   sudo docker ps -a
   ```

Elimine los contenedores del entorno de ejecución que se crearon en el dispositivo.

   ```bash
   docker rm -f edgeHub
   docker rm -f edgeAgent
   ```

Elimine los contenedores adicionales que se muestran en la salida `docker ps` haciendo referencia a los nombres de los contenedores. 

Quite el entorno de ejecución del contenedor.

   ```bash
   sudo apt-get remove --purge moby
   ```