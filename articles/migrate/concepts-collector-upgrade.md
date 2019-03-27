---
title: Actualizar el dispositivo Collector de Azure Migrate | Microsoft Docs
description: Proporciona información sobre las actualizaciones al dispositivo Azure Migrate Collector.
author: musa-57
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: hamusa
services: azure-migrate
ms.openlocfilehash: f9f2ef25a2054d92cdcc3ab33ea9234883e352d6
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58484515"
---
# <a name="collector-appliance-updates"></a>Actualizaciones de dispositivo de recopilador

## <a name="how-to-upgrade-the-appliance"></a>Cómo actualizar el dispositivo

Puede actualizar el Collector a la versión más reciente sin tener que descargar un archivo OVA de nuevo.

1. Cierre todas las ventanas de explorador y cualquiera abrir archivos o carpetas en el dispositivo.
2. Descargue el paquete de actualización más reciente de la lista de las actualizaciones que se mencionan a continuación, en este artículo.
3. Para asegurarse de que el paquete descargado es seguro, abra la ventana de comandos de administrador y ejecute el siguiente comando para generar el hash para el archivo ZIP. El código hash generado debe coincidir con el hash que se ha mencionado en la versión específica:

    ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

    Ejemplo: **C:\>CertUtil -HashFile C:\AzureMigrate\CollectorUpdate_release_1.0.9.14.zip SHA256)**
4. Copie el archivo zip en la máquina virtual del dispositivo Collector.
5. Haga clic con el botón derecho en el archivo ZIP > **Extraer todo**.
6. Haga clic con el botón derecho en **Setup.ps1** > **Ejecutar con PowerShell** y siga las instrucciones de instalación.

## <a name="collector-update-release-history"></a>Historial de versiones de actualizaciones de Collector

En este artículo se resume la información de actualización para la aplicación Collector en [Azure Migrate](migrate-overview.md).

Azure Migrate Collector es un dispositivo ligero que sirve para detectar un entorno de vCenter local a efectos de evaluación antes de realizar la migración a Azure. [Más información](concepts-collector.md).

### <a name="continuous-discovery-upgrade-versions"></a>Detección continua: versiones de actualización

#### <a name="version-101012-released-on-03132019"></a>Versión 1.0.10.12 (publicado 13/03/2019)

Contiene correcciones para problemas en la selección de Azure en la nube en el dispositivo.

Valores para la actualización hash [1.0.10.12 del paquete](https://aka.ms/migrate/col/upgrade_10_12)

**Algoritmo** | **Valor del código hash**
--- | ---
MD5 | 27704154082344c058238000dff9ae44
SHA1 | 41e9e2fb71a8dac14d64f91f0fd780e0d606785e
SHA256 | c6e7504fcda46908b636bfe25b8c73f067e3465b748f77e50027e66f2727c2a9

### <a name="one-time-discovery-deprecated-now-previous-upgrade-versions"></a>Detección de una sola vez (en desuso): versiones de actualización anteriores

> [!NOTE]
> El dispositivo de detección de una sola vez está en desuso, ya que este método dependía de la configuración de las estadísticas de vCenter Server para la disponibilidad de punto de datos de rendimiento y la media recopilada de los contadores de rendimiento, lo que daba lugar a un cálculo de tamaño insuficiente de las máquinas virtuales para la migración a Azure.

#### <a name="version-10916-released-on-10292018"></a>Versión 1.0.9.16 (publicada el 29/10/2018)

Contiene correcciones para problemas de PowerCLI que surgen al configurar el dispositivo.

Valores de código hash para el [paquete 1.0.9.16](https://aka.ms/migrate/col/upgrade_9_16) de actualización

**Algoritmo** | **Valor del código hash**
--- | ---
MD5 | d2c53f683b0ec7aaf5ba3d532a7382e1
SHA1 | e5f922a725d81026fa113b0c27da185911942a01
SHA256 | a159063ff508e86b4b3b7b9a42d724262ec0f2315bdba8418bce95d973f80cfc

#### <a name="version-10914"></a>Versión 1.0.9.14

Valores de código hash el [paquete 1.0.9.14](https://aka.ms/migrate/col/upgrade_9_14) de actualización

**Algoritmo** | **Valor del código hash**
--- | ---
MD5 | c5bf029e9fac682c6b85078a61c5c79c
SHA1 | af66656951105e42680dfcc3ec3abd3f4da8fdec
SHA256 | 58b685b2707f273aa76f2e1d45f97b0543a8c4d017cd27f0bdb220e6984cc90e

#### <a name="version-10913"></a>Versión 1.0.9.13

Valores de código hash para actualizar el [paquete 1.0.9.13](https://aka.ms/migrate/col/upgrade_9_13)

**Algoritmo** | **Valor del código hash**
--- | ---
MD5 | 739f588fe7fb95ce2a9b6b4d0bf9917e
SHA1 | 9b3365acad038eb1c62ca2b2de1467cb8eed37f6
SHA256 | 7a49fb8286595f39a29085534f29a623ec2edb12a3d76f90c9654b2f69eef87e


## <a name="next-steps"></a>Pasos siguientes

- [Obtenga más información](concepts-collector.md) sobre el dispositivo Collector.
- [Ejecutar una evaluación](tutorial-assessment-vmware.md) para máquinas virtuales de VMware.
