---
title: Introducción a la validación como servicio de Azure Stack | Microsoft Docs
description: Una introducción a la validación como servicio de Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: fa2e85bb2b201fa8e83a03d24883a66a72910f11
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49650186"
---
# <a name="what-is-validation-as-a-service-for-azure-stack"></a>¿Qué es la validación como servicio de Azure Stack?

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

La validación como servicio (VaaS) es un servicio nativo de Azure diseñado para los asociados de soluciones que trabajan conjuntamente con Microsoft en la construcción de ofertas de Azure Stack. Los asociados de soluciones pueden usar el servicio para comprobar que sus soluciones cumplen los requisitos de Microsoft y que funcionan con Azure Stack como se esperaba.

Los usos principales de VaaS son:

- Validar nuevas soluciones de Azure Stack
- Validar cambios en el software de Azure Stack
- Firmar digitalmente los paquetes de asociados de soluciones usados durante la implementación
- Obtener una vista previa de la documentación y el material adjunto de pruebas de VaaS

## <a name="validate-a-new-azure-stack-solution"></a>Validación de una nueva solución de Azure Stack

Los asociados usan el flujo de trabajo de **validación de soluciones** para comprobar las nuevas soluciones de Azure Stack. La solución debe superar las pruebas de componentes de Azure Stack de Hardware Lab Kit (HKL). Para certificar una gama de configuraciones de hardware, el flujo de trabajo se debe ejecutar dos veces para cada nueva solución: una vez cada una para las configuraciones mínima y máxima.

Para más información, consulte [Validar nuevas soluciones de Azure Stack](azure-stack-vaas-validate-solution-new.md).

## <a name="validate-changes-to-the-azure-stack-software"></a>Validar los cambios en el software de Azure Stack

Los asociados usan el flujo de trabajo de **validación de paquetes** para comprobar que su solución funciona con la actualización la más reciente de software de Azure Stack. El flujo de trabajo de validación de paquetes se debe ejecutar en un entorno de hardware recomendado por Microsoft, donde se hayan usado la revisión y la actualización para aplicar la actualización. Se recomienda también ejecutar el flujo de trabajo en la compilación de línea base.

Para más información, consulte [Validación de las actualizaciones de software de Microsoft](azure-stack-vaas-validate-microsoft-updates.md).

## <a name="get-digitally-signed-solution-partner-packages"></a>Obtención de paquetes de asociados de soluciones con firma digital

Además de validar las actualizaciones de Azure Stack, puede usar el flujo de trabajo de **validación de paquetes** para validar las actualizaciones de paquetes de personalización de OEM, que incluyen controladores específicos del asociado de Azure Stack, así como firmware y otro software que se usa durante la implementación del software de Azure Stack. Implemente el paquete que está validando para ver si tiene la versión actual del software de Azure Stack con al menos la solución de tamaño mínimo que se admitirá. El paquete se envía a VaaS antes de ejecutar las pruebas. Si se superan las pruebas, indique a [vaashelp@microsoft.com](mailto:vaashelp@microsoft.com) que el paquete ha completado las pruebas y que se deber firmar digitalmente con la firma digital de Azure Stack. Microsoft firma el paquete y notifica al asociado de Azure Stack que el paquete está disponible para descarga en el portal de VaaS.

Para más información, consulte [Validación de paquetes de OEM](azure-stack-vaas-validate-oem-package.md).

## <a name="preview-vaas-test-collateral"></a>Vista previa de la documentación y el material adjunto de pruebas de VaaS

Microsoft presenta periódicamente nuevas características en Azure Stack. Como parte del proceso de desarrollo para comercializar estas características, se presenta nueva documentación y material adjunto de pruebas en el flujo de trabajo de **pruebas superadas**. El flujo de trabajo de pruebas superadas incluye documentación y material adjunto de pruebas de otros flujos de trabajo para permitir la ejecución de pruebas no oficiales. No use el flujo de trabajo de pruebas superadas para enviar resultados para su aprobación. Use el flujo de trabajo de validación de soluciones y de validaciones de paquetes para obtener la aprobación oficial de su solución.

Para más información, consulte [Guía de inicio rápido: Uso del portal de validación como servicio para programar la primera prueba](azure-stack-vaas-schedule-test-pass.md).

## <a name="next-steps"></a>Pasos siguientes

- [Set up your Validation as a Service resources](azure-stack-vaas-set-up-resources.md) (Configuración de los recursos de validación como servicio)
- Más información sobre [Conceptos clave de la validación como servicio](azure-stack-vaas-key-concepts.md)
