---
title: Validación de los paquetes del fabricante de equipos originales (OEM) en la validación como servicio de Azure Stack | Microsoft Docs
description: Obtenga información sobre cómo comprobar los paquetes del fabricante de equipos originales (OEM) con la validación como servicio.
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
ms.openlocfilehash: cefa32c35df4e87d4d2b983ee8c4a16dc065e774
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44160462"
---
# <a name="validate-oem-packages"></a>Validación de paquetes de OEM

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Puede probar un nuevo paquete de OEM cuando se ha producido un cambio en el firmware o en los controladores para una validación de solución completa. Cuando el paquete ha superado la prueba, Microsoft lo firma. La prueba debe contener el paquete de extensión de OEM actualizado con los controladores y el firmware que han superado las pruebas del logotipo de Windows Server y las pruebas PCS.

Todas las pruebas finalizan en 24 horas con el resultado de **Correcto**. Si alguna de las pruebas tiene un resultado **erróneo**, registre un error en [Microsoft Collaborate](https://aka.ms/collaborate) y envíe un correo electrónico a [vaashelp@microsoft.com](mailto:vaashelp@microsoft.com) para notificarlo a Microsoft.

## <a name="get-your-oem-package-signed"></a>Obtención del paquete de OEM firmado

1. Asegúrese de que se ha aplicado la actualización del mes actual. Para ver cuál es la versión más reciente, consulte [Documentación del operador de Azure Stack > Información general > Notas de la versión](https://docs.microsoft.com/azure/azure-stack/).

    Las actualizaciones de software de Microsoft en Azure Stack se designan utilizando una convención de nomenclatura, por ejemplo, 1803 indica la actualización de marzo de 2018. Para obtener información acerca de la directiva de actualización de Azure Stack, la cadencia y las notas de la versión disponibles, consulte [Directiva de mantenimiento de Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-servicing-policy).

1. Para comprobar el estado de mantenimiento del sistema, ejecute **Test-AzureStack** tal y como se describe en [Ejecución de una prueba de validación para Azure Stack. Corrija los errores y las advertencias antes de iniciar las pruebas.

2. En el [portal de validación](https://azurestackvalidation.com), seleccione una solución existente. Si no ha agregado la solución, consulte [Adición de una nueva solución](azure-stack-vaas-validate-solution-new.md#add-a-new-solution).

3. Seleccione **Start** (Iniciar) en el icono **Package Validations** (Validaciones de paquetes) para iniciar un nuevo flujo de trabajo.

    ![Validaciones de paquetes](media/image3.png)

4.  Proporcione una cadena de conexión de diagnósticos. Para obtener instrucciones, consulte [Configuración de una cuenta de almacenamiento](azure-stack-vaas-set-up-account.md).

    Se debe especificar un paquete de extensión de OEM para cada ejecución de la validación del paquete. Especifique el paquete de OEM que se instaló en la solución en el momento de la implementación de Azure Stack. Para obtener instrucciones, consulte [Creación de un blob de Azure Storage para almacenar los registros](azure-stack-vaas-set-up-account.md#create-an-azure-storage-blob-to-store-logs).

    Se debe usar un archivo JSON con las variables de entorno para finalizar la entrada de los campos obligatorios de la ejecución con el fin de evitar errores en la entrada de datos. Para obtener instrucciones, consulte [Obtención de un archivo de configuración en una implementación de Azure Stack](azure-stack-vaas-parameters.md).

5. Ejecute las pruebas.

6. Cuando todas las pruebas finalicen correctamente, envíe el nombre de la solución y la validación del paquete a [vaashelp@microsoft.com](mailto:vaashelp@microsoft.com) para solicitar la firma del paquete.

## <a name="next-steps"></a>Pasos siguientes

- Para más información, consulte [Validación como servicio de Azure Stack](https://docs.microsoft.com/azure/azure-stack/partner).
