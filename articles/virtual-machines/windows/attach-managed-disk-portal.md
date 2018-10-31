---
title: 'Conexión de un disco de datos administrado a una VM con Windows: Azure | Microsoft Docs'
description: Cómo conectar un disco de datos administrado a una máquina virtual Windows en Azure Portal.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/08/2018
ms.author: cynthn
ms.openlocfilehash: 8d83af114ebb5e5ff78372897d3e08ed592d4012
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2018
ms.locfileid: "49093908"
---
# <a name="attach-a-managed-data-disk-to-a-windows-vm-by-using-the-azure-portal"></a>Conexión de un disco de datos administrado a una máquina virtual Windows en Azure Portal

En este artículo se muestra cómo conectar un nuevo disco de datos administrado a una máquina virtual Windows en Azure Portal. El tamaño de la máquina virtual determina el número de discos que se puede conectar. Para más información, consulte [Tamaños de las máquinas virtuales Linux en Azure](sizes.md).


## <a name="add-a-data-disk"></a>Agregar un disco de datos

1. En [Azure Portal](https://portal.azure.com), en el menú de la izquierda, seleccione **Máquinas virtuales**.
2. Seleccione una máquina virtual de la lista.
3. En la página **Máquina virtual**, seleccione **Discos**.
4. En la página **Discos**, seleccione **Agregar disco de datos**.
5. En la lista desplegable para el nuevo disco, seleccione **Crear disco**.
6. En la página **Crear disco administrado**, escriba un nombre para el disco y ajuste las demás opciones de configuración según sea necesario. Seleccione **Crear** cuando haya terminado.
7. En la página **Discos**, seleccione **Guardar** para guardar la configuración del nuevo disco de la máquina virtual.
8. Una vez que Azure crea el disco y lo asocia a la máquina virtual, el nuevo disco aparece en la configuración de disco de la máquina virtual en el apartado **Discos de datos**.


## <a name="initialize-a-new-data-disk"></a>Inicio de un nuevo disco de datos

1. Conéctese a la máquina virtual.
1. Seleccione el menú **Inicio** de Windows en la máquina virtual en ejecución y escriba **diskmgmt.msc** en el cuadro de búsqueda. Se abre la consola **Administración de discos**.
2. Administración de discos reconocerá que hay un nuevo disco sin inicializar y se abrirá la ventana **Inicializar disco**.
3. Asegúrese de que el nuevo disco está seleccionado y seleccione **Aceptar** para inicializarlo.
4. El nuevo disco aparecerá como **sin asignar**. Haga clic con el botón derecho en cualquier lugar del disco y seleccione **Nuevo volumen simple**. Se abre la ventana **Asistente para nuevo volumen simple**.
5. Siga los pasos del asistente, conserve todos los valores predeterminados y, cuando haya terminado, seleccione **Finalizar**.
6. Cierre **Administración de discos**.
7. Se abre una ventana emergente que indica que es necesario dar formato al nuevo disco para poder usarlo. Seleccione **Dar formato al disco**.
8. En la ventana **Dar formato al nuevo disco**, active las opciones y seleccione **Iniciar**.
9. Recibirá una advertencia que indica que al dar formato a los discos se borrarán todos los datos. Seleccione **Aceptar**.
10. Cuando se complete el formato, seleccione **Aceptar**.

## <a name="use-trim-with-standard-storage"></a>Uso de TRIM con el almacenamiento estándar

Si utiliza almacenamiento estándar (HDD), debe habilitar el comando **TRIM**. El comando **TRIM** descarta los bloques sin utilizar del disco, por lo que solo se le facturará el almacenamiento que utiliza realmente. Mediante el uso de **TRIM**, puede ahorrar costos si crea archivos grandes y, a continuación, los elimina más adelante. 

Para comprobar la configuración de **TRIM**, abra un símbolo del sistema en la máquina virtual Windows y escriba el siguiente comando:

```
fsutil behavior query DisableDeleteNotify
```

Si el comando devuelve 0, **TRIM** está habilitado correctamente. En caso contrario, si devuelve 1, ejecute el siguiente comando para habilitar **TRIM**:

```
fsutil behavior set DisableDeleteNotify 0
```

Después de eliminar los datos del disco, puede garantizar que las operaciones de **TRIM** se vacían correctamente mediante la ejecución de la desfragmentación con **TRIM**:

```
defrag.exe <volume:> -l
```

También puede dar formato al volumen para asegurarse de que queda recortado por completo.

## <a name="next-steps"></a>Pasos siguientes

- También puede [conectar un disco de datos mediante PowerShell](attach-disk-ps.md).
- Si la aplicación necesita usar la unidad *D:* para almacenar datos, puede [cambiar la letra de la unidad del disco temporal de Windows](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
