---
title: Introducción a la validación como servicio de Azure Stack | Microsoft Docs
description: Una introducción a los problemas conocidos de la validación como servicio de Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 56251245a23df031f3bc8fe3d36de43e194fbcc7
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44159680"
---
# <a name="what-is-validation-as-a-service-for-azure-stack"></a>¿Qué es la validación como servicio de Azure Stack?

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

La validación como servicio (VaaS) es un servicio nativo de Azure diseñado para los asociados de soluciones que trabajan conjuntamente con Microsoft en la construcción de ofertas de Azure Stack. Los asociados de soluciones pueden usar el servicio para comprobar que sus soluciones cumplen los requisitos de Microsoft y que funcionan con Azure Stack como se esperaba.

El uso principal de VaaS es:

- Validar nuevas soluciones de Azure Stack
- Validar los cambios en el software de Azure Stack
- Obtener los paquetes de asociados de soluciones con firma digital usados durante la implementación
- Obtener una vista previa de la documentación y material adjunto de validación de Azure Stack

## <a name="validate-new-azure-stack-solution"></a>Validación de una nueva solución de Azure Stack

Los asociados usan el flujo de trabajo de validación de soluciones para comprobar las nuevas soluciones de Azure Stack. La solución debe superar las comprobaciones de componentes de las pruebas de Azure Stack de Hardware Lab Kit (HKL). Solo es necesario ejecutar el flujo de trabajo dos veces con cada nueva solución: una vez para la configuración mínima y otra para la configuración máxima.

Para más información, consulte [Validar nuevas soluciones de Azure Stack](azure-stack-vaas-validate-solution-new.md).

## <a name="validate-changes-to-the-azure-stack-software"></a>Validar los cambios en el software de Azure Stack

Los asociados usan el flujo de trabajo de validación de paquetes para comprobar que su solución funciona con la reciente actualización de software de Azure Stack. Como mínimo, el flujo de trabajo de validación de paquetes se debe ejecutar en el entorno de hardware que Microsoft recomienda, y en una solución donde se hayan usado la revisión y la actualización para aplicar la actualización. La validación de paquetes se debe ejecutar en la compilación de base de referencia.

Para más información, consulte [Validación de las actualizaciones de software de Microsoft](azure-stack-vaas-validate-microsoft-updates.md).

## <a name="get-digitally-signed-solution-partner-packages"></a>Obtención de paquetes de asociados de soluciones con firma digital

Además de validar las actualizaciones de Azure Stack, puede usar el flujo de trabajo de validación de paquetes para comprobar las actualizaciones de paquetes de personalización OEM, que incluyen controladores específicos del asociado de Azure Stack, así como firmware y otro software que se usa durante la implementación del software de Azure Stack. Implemente el paquete que está comprobando para ver si tiene la versión actual del software de Azure Stack con al menos la solución de tamaño mínimo que se admitirá. El paquete actualizado debe cargarse en el servicio antes de comenzar la prueba. Si las pruebas se realizan correctamente, notifíquelo a vaashelp@microsoft.com. Indique al asociado de Azure Stack que el paquete ha completado las pruebas y que se deber firmar digitalmente con la firma digital de Azure Stack. Microsoft firma el paquete y notifica al asociado de Azure Stack que está disponible para descarga en el portal.

Para más información, consulte [Validación de paquetes de OEM](azure-stack-vaas-validate-oem-package.md).

## <a name="preview-azure-stack-validation-collateral"></a>Obtener una vista previa de la documentación y material adjunto de validación de Azure Stack

Microsoft presenta periódicamente nuevas características en Azure Stack. Como parte del proceso de desarrollo para comercializar estas características, se presenta nueva documentación y material adjunto de pruebas en el flujo de trabajo de pruebas superadas. El flujo de trabajo de pruebas superadas incluye documentación y material adjunto de pruebas de otros flujos de trabajo para permitir la ejecución de pruebas no oficiales. No use el flujo de trabajo de pruebas superadas para enviar resultados para su aprobación. Use el flujo de trabajo de validación de soluciones y de validaciones de paquetes para obtener la aprobación oficial de su solución.

## <a name="next-steps"></a>Pasos siguientes

- Empiece a trabajar y [configure la cuenta de validación como servicio](azure-stack-vaas-validate-solution-new.md).
