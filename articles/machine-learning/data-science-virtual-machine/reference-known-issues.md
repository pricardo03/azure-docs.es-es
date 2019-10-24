---
title: Problemas conocidos y soluciones
titleSuffix: Azure Data Science Virtual  Machine
description: Obtenga una lista de los problemas conocidos y soluciones para Azure Data Science Virtual Machine
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: gvashishtha
ms.author: gopalv
ms.topic: reference
ms.date: 10/10/2019
ms.openlocfilehash: a0b22326a429edfa2f2b8741453215b42910891c
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2019
ms.locfileid: "72302201"
---
# <a name="known-issues-and-troubleshooting-the-azure-data-science-virtual-machine"></a>Problemas conocidos y solución de problemas de Azure Data Science Virtual Machine

En este artículo obtendrá ayuda para buscar y corregir los errores que se producen al usar Azure Data Science Virtual Machine.

## <a name="python-package-installation-issues"></a>Problemas de instalación del paquete de Python

### <a name="installing-packages-with-pip-breaks-dependencies-on-linux"></a>La instalación de paquetes con PIP rompe las dependencias en Linux

Use `sudo pip install` en lugar de `pip install` al instalar paquetes.

## <a name="disk-encryption-issues"></a>Problemas de cifrado de disco

### <a name="disk-encryption-fails-on-the-ubuntu-dsvm"></a>Error de cifrado de disco en Ubuntu DSVM

Azure Disk Encryption (ADE) no se admite actualmente en Ubuntu DSVM. Como alternativa, considere la posibilidad de configurar el [cifrado de Azure Storage con claves administradas por el cliente](../../storage/common/storage-encryption-keys-portal.md).

## <a name="tool-appears-disabled"></a>La herramienta aparece deshabilitada

### <a name="hyper-v-does-not-work-on-the-windows-dsvm"></a>Hyper-V no funciona en Windows DSVM

Este es el comportamiento esperado, ya que para mejorar el rendimiento de arranque hemos deshabilitado algunos servicios. Para volver a habilitarlos, abra la barra de búsqueda en Windows DSVM, escriba "servicios" y, a continuación, establezca todos los servicios de Hyper-V en "Manual" y establezca "Administración de máquinas virtuales de Hyper-V" en "Automático".

La pantalla final debería tener este aspecto:

   ![Habilitar Hyper-V](./media/workaround/hyperv-enable-dsvm.png)

