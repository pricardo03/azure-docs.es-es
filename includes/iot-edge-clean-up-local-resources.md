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
ms.openlocfilehash: cdb60ffb3ba3c31c336c8b74c46621792c707f74
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2018
ms.locfileid: "40046451"
---
### <a name="delete-local-resources"></a>Eliminación de recursos locales

Si desea eliminar el entorno de ejecución de Azure IoT Edge y los recursos relacionados del dispositivo, use los comandos adecuados para el sistema operativo del dispositivo. 

#### <a name="windows"></a>Windows

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