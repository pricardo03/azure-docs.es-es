---
title: Actualizar el dispositivo Collector de Azure Migrate | Microsoft Docs
description: Proporciona información sobre las actualizaciones al dispositivo Azure Migrate Collector.
author: musa-57
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 09/28/2018
ms.author: hamusa
services: azure-migrate
ms.openlocfilehash: 6080096194ec5b11f1d5593fcbb6732d1551667f
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "47431399"
---
# <a name="collector-update-release-history"></a>Historial de versiones de actualizaciones de Collector

En este artículo se resume la información de actualización para la aplicación Collector en [Azure Migrate](migrate-overview.md).

Azure Migrate Collector es un dispositivo ligero que sirve para detectar un entorno de vCenter local a efectos de evaluación antes de realizar la migración a Azure. [Más información](concepts-collector.md).


## <a name="one-time-discovery-upgrade-versions"></a>Detección de un solo uso: actualización de versiones

### <a name="version-10914"></a>Versión 1.0.9.14

Valores de código hash el [paquete 1.0.9.14](https://aka.ms/migrate/col/upgrade_9_14) de actualización

**Algoritmo** | **Valor del código hash**
--- | ---
MD5 | c5bf029e9fac682c6b85078a61c5c79c
SHA1 | af66656951105e42680dfcc3ec3abd3f4da8fdec
SHA256 | 58b685b2707f273aa76f2e1d45f97b0543a8c4d017cd27f0bdb220e6984cc90e

### <a name="version-10913"></a>Versión 1.0.9.13

Valores de código hash para actualizar el [paquete 1.0.9.13](https://aka.ms/migrate/col/upgrade_9_13)

**Algoritmo** | **Valor del código hash**
--- | ---
MD5 | 739f588fe7fb95ce2a9b6b4d0bf9917e
SHA1 | 9b3365acad038eb1c62ca2b2de1467cb8eed37f6
SHA256 | 7a49fb8286595f39a29085534f29a623ec2edb12a3d76f90c9654b2f69eef87e

### <a name="version-10911"></a>Versión 1.0.9.11

Hash de valores para actualizar el [paquete 1.0.9.11](https://aka.ms/migrate/col/upgrade_9_11)

**Algoritmo** | **Valor del código hash**
--- | ---
MD5 | 0e36129ac5383b204720df7a56b95a60
SHA1 | aa422ef6aa6b6f8bc88f27727e80272241de1bdf
SHA256 | 5f76dbbe40c5ccab3502cc1c5f074e4b4bcbf356d3721fd52fb7ff583ff2b68f

### <a name="version-1097"></a>Versión 1.0.9.7

Hash de valores para actualizar el [paquete 1.0.9.7](https://aka.ms/migrate/col/upgrade_9_7)

**Algoritmo** | **Valor del código hash**
--- | ---
MD5 | 01ccd6bc0281f63f2a672952a2a25363
SHA1 | 3e6c57523a30d5610acdaa14b833c070bffddbff
SHA256 | e3ee031fb2d47b7881cc5b13750fc7df541028e0a1cc038c796789139aa8e1e6

## <a name="continuous-discovery-upgrade-versions"></a>Detección continua: actualización de versiones

Aún no está disponible ninguna actualización para el dispositivo de detección continua.

## <a name="run-an-upgrade"></a>Ejecutar una actualización

Puede actualizar el Collector a la versión más reciente sin tener que descargar un archivo OVA de nuevo.

1. Descargue el paquete de actualización más reciente en la siguiente lista.
2. Para asegurarse de que la revisión descargada es segura, abra la ventana de comandos del administrador y ejecute el siguiente comando para generar el valor hash para el archivo ZIP. El código hash generado debe coincidir con el hash que se ha mencionado en la versión específica:

    ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

    Ejemplo: **C:\>CertUtil -HashFile C:\AzureMigrate\CollectorUpdate_release_1.0.9.14.zip SHA256)**
3. Copie el archivo zip en la máquina virtual del dispositivo Collector.
4. Haga clic con el botón derecho en el archivo ZIP > **Extraer todo**.
5. Haga clic con el botón derecho en **Setup.ps1** > **Ejecutar con PowerShell** y siga las instrucciones de instalación.


## <a name="next-steps"></a>Pasos siguientes

- [Obtenga más información](concepts-collector.md) sobre el dispositivo Collector.
- [Ejecutar una evaluación](tutorial-assessment-vmware.md) para máquinas virtuales de VMware.
