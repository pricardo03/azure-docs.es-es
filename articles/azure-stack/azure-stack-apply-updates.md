---
title: Aplicación de actualizaciones en Azure Stack | Microsoft Docs
description: Aprenda a importar e instalar paquetes de actualización de Microsoft para un sistema integrado de Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/18/2019
ms.author: mabrigg
ms.reviewer: wfayed
ms.openlocfilehash: 190d81fc7811e4afdb32555407716f60f5b9a2d1
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54476145"
---
# <a name="apply-updates-in-azure-stack"></a>Aplicación de actualizaciones en Azure Stack

*Se aplica a: Sistemas integrados de Azure Stack*

Puede utilizar el icono **Actualizar** del portal de administración para aplicar paquetes de actualización de OEM o Microsoft a Azure Stack. Debe descargar el paquete de actualización, importar los archivos del paquete en Azure Stack e instalar el paquete de actualización.

## <a name="download-the-update-package"></a>Descarga del paquete de actualización

Cuando haya disponible un paquete de actualización de OEM o Microsoft para Azure Stack, descárguelo a una ubicación a la que se pueda acceder desde Azure Stack y revise el contenido del paquete. Normalmente, un paquete de actualización consta de los siguientes archivos:

- Un archivo `<PackageName>.exe` autoextraíble. Este archivo contiene la carga útil de la actualización, por ejemplo la actualización acumulativa más reciente de Windows Server.

- Los archivos `<PackageName>.bin` que correspondan. Estos archivos proporcionan compresión para la carga que está asociada el archivo *PackageName*.exe.

- Un archivo `Metadata.xml`. Este archivo contiene información esencial acerca de la actualización, como el editor, nombre, requisito previo, tamaño y dirección URL de ruta de acceso de soporte.

## <a name="import-and-install-updates"></a>Importación e instalación de actualizaciones

El siguiente procedimiento muestra cómo importar e instalar actualizaciones en el portal de administración.

> [!IMPORTANT]  
> Se recomienda firmemente notificar a los usuarios cualquier operación de mantenimiento, así como programar ventanas de mantenimiento normal durante el horario no laborable tanto como sea posible. Las operaciones de mantenimiento pueden afectar tanto a las cargas de trabajo del usuario como a las operaciones del portal.

1. En el portal de administración, haga clic en **Todos los servicios**. Luego, en la categoría **DATOS Y ALMACENAMIENTO**, haga clic en **Cuentas de almacenamiento**. (o bien, en el cuadro de filtro, empiece a escribir **storage accounts** y selecciónelo).

    ![Muestra dónde se encuentran las cuentas de almacenamiento en el portal](media/azure-stack-apply-updates/ApplyUpdates1.png)

2. En el cuadro de filtro, escriba **update**y seleccione la cuenta de almacenamiento **updateadminaccount**.

3. En los detalles de la cuenta de almacenamiento, en **Servicios**, seleccione **Blobs**.
 
    ![Muestra como acceder a Blobs para la cuenta de almacenamiento](media/azure-stack-apply-updates/ApplyUpdates3.png) 

4. En **Blob service**, seleccione **+ Contenedor** para crear un contenedor. Escriba un nombre (por ejemplo *Update-1811*) y, después, seleccione **Aceptar**.
 
     ![Muestra cómo agregar un contenedor en la cuenta de almacenamiento](media/azure-stack-apply-updates/ApplyUpdates4.png)

5. Tras crear el contenedor, haga clic en su nombre y, después, haga clic en **Cargar** para cargar los archivos de paquete en el contenedor.
 
    ![Muestra cómo cargar los archivos de paquete](media/azure-stack-apply-updates/ApplyUpdates5.png)

6. En **Cargar blob**, haga clic en el icono de la carpeta, vaya al archivo .exe del paquete de actualización y haga clic en **Abrir** en la ventana del explorador de archivos.
  
7. En **Cargar blob**, haga clic en **Cargar**.
  
    ![Muestra dónde se carga cada archivo de paquete](media/azure-stack-apply-updates/ApplyUpdates6.png)

8. Repita los pasos 6 y 7 para los archivos *PackageName*.bin y Metadata.xml. No importe el archivo Supplemental Notice.txt si está incluido.
9. Cuando haya finalizado, puede revisar las notificaciones (el icono de campana de la esquina superior derecha del portal). Las notificaciones deben indicar que la carga se ha completado.
10. Vuelva al icono de actualización del panel. El icono debe indicar que hay una actualización disponible. Haga clic en el icono para revisar el paquete de actualización recién agregado.
11. Para instalar la actualización, seleccione el paquete que está marcado como **Listo** y haga clic con el botón derecho en él y seleccione **Actualizar ahora**, o bien haga clic en la acción **Actualizar ahora** cerca de la parte superior.
12. Al hacer clic en el paquete de actualización de instalación, puede ver el estado en el área **Update run details** (Detalles de ejecución de actualización). Aquí también puede hacer clic en **Download full logs** (Descargar registros completos) para descargar los archivos de registro.
13. Cuando la actualización finaliza, el icono de actualización muestra la versión actualizada de Azure Stack.

Puede eliminar manualmente las actualizaciones de la cuenta de almacenamiento después de haberlas instalado en Azure Stack. Azure Stack busca periódicamente los paquetes de actualización más antiguos y los elimina del almacenamiento. Azure Stack puede tardar dos semanas en quitar los paquetes antiguos.

## <a name="next-steps"></a>Pasos siguientes

- [Introducción a la administración de actualizaciones en Azure Stack](azure-stack-updates.md)
- [Directiva de servicio de Azure Stack](azure-stack-servicing-policy.md)
