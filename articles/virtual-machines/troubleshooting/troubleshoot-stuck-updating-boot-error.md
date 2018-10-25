---
title: El inicio de la máquina virtual de Azure está atascado en una actualización de Windows | Microsoft Docs
description: Obtenga información sobre cómo solucionar el problema que surge cuando el inicio de una máquina virtual de Azure está atascado en una actualización de Windows.
services: virtual-machines-windows
documentationCenter: ''
authors: genli
manager: cshepard
editor: v-jesits
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/09/2018
ms.author: genli
ms.openlocfilehash: 2d42d2014432b72f35e9b0d9543fe499a6ab721b
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "49355382"
---
# <a name="azure-vm-startup-is-stuck-at-windows-update"></a>El inicio de la máquina virtual de Azure está atascado en una actualización de Windows

Este artículo le ayuda a solucionar el problema que surge cuando la máquina virtual (VM) está atascada en la fase de actualización de Windows Update durante el inicio. 

> [!NOTE] 
> Azure tiene dos modelos de implementación diferentes para crear y trabajar con recursos: [el Administrador de recursos y el clásico](../../azure-resource-manager/resource-manager-deployment-model.md). Este artículo trata sobre el uso del modelo de implementación del Administrador de recursos. Se recomienda usar este modelo para las nuevas implementaciones, en lugar de usar el modelo de implementación clásica.

 ## <a name="symptom"></a>Síntoma

 Una máquina virtual de Windows no se inicia. Al revisar las capturas de pantalla en la ventana [Diagnósticos de arranque](../troubleshooting/boot-diagnostics.md), ve que el inicio está atascado en el proceso de actualización. A continuación, le mostramos algunos ejemplos de mensajes de error que puede recibir:

- Instalando Windows ##%. No apague el equipo. Este proceso tardará unos minutos. El equipo se reiniciará varias veces
- Mantenga su equipo encendido hasta que finalice el proceso. Instalando actualización # de #... 
- No pudimos completar la actualización. Deshaciendo cambios. No apague el equipo.
- Error de configuración de las actualizaciones de Windows. Revirtiendo cambios. No apague el equipo.
- Error < código de error > al aplicar las operaciones de actualización ### de ### (\Regist...)
- Error grave < código de error > al aplicar las operaciones de actualización ### de ### (\Regist...)


## <a name="solution"></a>Solución

Según el número de actualizaciones que estén instalándose o revirtiéndose, el proceso de actualización puede tardar unos minutos. Deje la máquina virtual en este estado durante 8 horas. Si la máquina virtual se encuentra en este estado después de ese período, reinicie la máquina virtual desde Azure Portal y vea si puede iniciarse normalmente. Si este paso no funciona, pruebe la siguiente solución.

### <a name="remove-the-update-that-causes-the-problem"></a>Eliminación de la actualización que causa el problema

1. Tome una instantánea del disco del sistema operativo de la máquina virtual afectada como copia de seguridad. Para obtener más información, consulte [Instantánea de un disco](../windows/snapshot-copy-managed-disk.md). 
2. [Conecte el disco del sistema operativo a una máquina virtual de recuperación](troubleshoot-recovery-disks-portal-windows.md).
3. Una vez que el disco del sistema operativo esté conectado a la máquina virtual de recuperación, abra el **Administrador de discos** y asegúrese de que está **CONECTADO**. Tome nota de la letra de unidad que se asigna al disco del sistema operativo conectado que contiene la carpeta \windows. Si el disco está cifrado, descífrelo antes de continuar con los pasos siguientes en este documento.

3. Obtenga la lista de los paquetes de actualización que se encuentran en el disco del sistema operativo conectado:

        dism /image:<Attached OS disk>:\ /get-packages > c:\temp\Patch_level.txt

    Por ejemplo, si el disco del sistema operativo conectado es la unidad F, ejecute el siguiente comando:

        dism /image:F:\ /get-packages > c:\temp\Patch_level.txt
4. Abra el archivo C:\temp\Patch_level.txt y, a continuación, léalo del final al inicio. Busque la actualización que se encuentre en el estado **Instalación pendiente** o **Desinstalación pendiente**.  A continuación se muestra un ejemplo del estado de actualización:

     ```
    Package Identity : Package_for_RollupFix~31bf3856ad364e35~amd64~~17134.345.1.5
    State : Install Pending
    Release Type : Security Update
    Install Time :
    ```
5. Elimine la actualización que causó el problema:
    
    ```
    dism /Image:<Attached OS disk>:\ /Remove-Package /PackageName:<PACKAGE NAME TO DELETE>
    ```
    Ejemplo: 

    ```
    dism /Image:F:\ /Remove-Package /Package_for_RollupFix~31bf3856ad364e35~amd64~~17134.345.1.5
    ```

    > [!NOTE] 
    > Según el tamaño del paquete, la herramienta DISM tardará un rato en procesar la desinstalación. Normalmente, el proceso se completará en un plazo de 16 minutos.

6. Desasocie el disco del sistema operativo y, a continuación, [úselo para recompilar la máquina virtual](troubleshoot-recovery-disks-portal-windows.md). 