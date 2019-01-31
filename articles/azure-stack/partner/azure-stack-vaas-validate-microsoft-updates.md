---
title: Validación de las actualizaciones de software de Microsoft en la validación como servicio de Azure Stack | Microsoft Docs
description: Aprenda a validar las actualizaciones de software de Microsoft con la validación como servicio.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/14/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 01/14/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 4e6f5a17544c1419eb6101acdd6590f034ea4aa3
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55241465"
---
# <a name="validate-software-updates-from-microsoft"></a>Validación de las actualizaciones de software de Microsoft

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Microsoft lanzará periódicamente actualizaciones del software de Azure Stack. Estas actualizaciones se proporcionan para los asociados de ingeniería conjunta de Azure Stack. Las actualizaciones se proporcionan antes de que estén públicamente disponibles. Puede comprobar las actualizaciones en función de su solución y proporcionar comentarios a Microsoft.

[!INCLUDE [azure-stack-vaas-workflow-validation-completion](includes/azure-stack-vaas-workflow-validation-completion.md)]

## <a name="apply-monthly-update"></a>Aplicación de actualización mensual

[!INCLUDE [azure-stack-vaas-workflow-section_update-azs](includes/azure-stack-vaas-workflow-section_update-azs.md)]

## <a name="create-a-workflow"></a>Creación de un flujo de trabajo

Las validaciones de actualización usan el mismo flujo de trabajo que la **validación de soluciones**.

## <a name="run-tests"></a>Ejecución de las pruebas

1. Las validaciones de actualización usan el mismo flujo de trabajo que la **validación de soluciones**. 

2. Siga las instrucciones que se indican en [Ejecución de pruebas de validación de soluciones](azure-stack-vaas-validate-oem-package.md#run-package-validation-tests). En su lugar, seleccione las siguientes pruebas:
    - Monthly Azure Stack Update Verification
    - Cloud Simulation Engine

No es necesario solicitar la firma de paquetes para validaciones de actualización.

## <a name="next-steps"></a>Pasos siguientes

- [Supervisión y administración de pruebas en el portal de VaaS](azure-stack-vaas-monitor-test.md)