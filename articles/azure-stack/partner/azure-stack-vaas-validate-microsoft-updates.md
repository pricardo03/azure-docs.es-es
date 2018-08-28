---
title: Validación de las actualizaciones de software de Microsoft en la validación como servicio de Azure Stack | Microsoft Docs
description: Obtenga información sobre cómo validar las actualizaciones de software de Microsoft con la validación como servicio.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: a9e1bb2f134e0e51803266f5e7f94c05ab6966f2
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2018
ms.locfileid: "40234501"
---
# <a name="validate-software-updates-from-microsoft"></a>Validación de las actualizaciones de software de Microsoft

[!INCLUDE[Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Microsoft lanzará periódicamente actualizaciones del software de Azure Stack. Estas actualizaciones se proporcionan a los asociados de ingeniería conjunta de Azure Stack antes de estar disponibles públicamente para que puedan validar las actualizaciones en sus soluciones y proporcionar comentarios a Microsoft.

## <a name="test-an-existing-solution"></a>Prueba de una solución existente

1. Inicie sesión en el [portal de validación](https://azurestackvalidation.com).

2. Seleccione una solución existente en la que se haya implementado la actualización de Microsoft y seleccione **Start** (Iniciar) en el icono **Package Validation** (Validación de paquetes).

    ![Validación de paquetes](media/image3.png)

3. Escriba el nombre de la validación.

4. Escriba la dirección URL del paquete de OEM que se instaló en la solución en el momento de la implementación. Use la dirección del paquete almacenado en el servicio de blobs de Azure. Para más información, consulte [Creación de un blob de Azure Storage para almacenar los registros](azure-stack-vaas-set-up-account.md#create-an-azure-storage-blob-to-store-logs).

5. Seleccione **Upload** (Cargar) para agregar el archivo de configuración de la implementación. Consulte [Validación de una nueva solución de Azure Stack](azure-stack-vaas-validate-solution-new.md) para obtener información acerca de cómo cargar el archivo de configuración de la implementación.

6. Se debe personalizar el archivo de configuración de la implementación con el archivo de parámetros de entorno correcto. Consulte [Parámetros de entorno](azure-stack-vaas-parameters.md#environment-parameters) para más detalles.

    > [!Note]   
    > Para personalizar más el archivo de configuración de la implementación, agregue parámetros de pruebas comunes. Para más información, consulte [Parámetros comunes del flujo de trabajo en la validación como servicio de Azure Stack](azure-stack-vaas-parameters.md).

7. El nombre de usuario y la contraseña del usuario del inquilino, el administrador del servicio y el administrador de la nube deben especificarse manualmente.

8. Proporcione la dirección URL del blob de Azure Storage para almacenar los registros de diagnóstico. Para más información, consulte [Creación de un blob de Azure Storage para almacenar los registros](azure-stack-vaas-set-up-account.md#create-an-azure-storage-blob-to-store-logs).

    > [!Note]  
    > Se pueden especificar etiquetas descriptivas para etiquetar el flujo de trabajo.

10. Seleccione **Submit** (Enviar) para guardar el flujo de trabajo.

El flujo de trabajo de la solución se ejecuta durante aproximadamente 24 horas. Agregue un vínculo o instrucciones sobre la programación de las pruebas. Realice la limpieza de la herramienta.

Para más información sobre cómo supervisar el progreso de una ejecución de validación, consulte [Supervisión de una prueba](azure-stack-vaas-monitor-test.md).

## <a name="next-steps"></a>Pasos siguientes

- Para más información, consulte [Validación como servicio de Azure Stack](https://docs.microsoft.com/azure/azure-stack/partner).
