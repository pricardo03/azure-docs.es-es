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
ms.date: 11/26/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 8e0009bf0fc34d3e0d22755d93d941b85db62ffd
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2018
ms.locfileid: "52334472"
---
# <a name="validate-software-updates-from-microsoft"></a>Validación de las actualizaciones de software de Microsoft

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Microsoft lanzará periódicamente actualizaciones del software de Azure Stack. Estas actualizaciones se proporcionan a los asociados de ingeniería conjunta de Azure Stack antes de estar disponibles públicamente para que puedan validar las actualizaciones en sus soluciones y proporcionar comentarios a Microsoft.

[!INCLUDE [azure-stack-vaas-workflow-validation-completion](includes/azure-stack-vaas-workflow-validation-completion.md)]

## <a name="apply-monthly-update"></a>Aplicación de actualización mensual

[!INCLUDE [azure-stack-vaas-workflow-section_update-azs](includes/azure-stack-vaas-workflow-section_update-azs.md)]

## <a name="create-a-workflow"></a>Creación de un flujo de trabajo

Las validaciones de actualización usan el mismo flujo de trabajo que la **validación de paquetes**. Siga las instrucciones que se indican en [Create a Package Validation workflow](azure-stack-vaas-validate-oem-package.md#create-a-package-validation-workflow) (Creación de un flujo de trabajo de Package Validation).

## <a name="run-tests"></a>Ejecución de las pruebas

Las validaciones de actualización usan el mismo flujo de trabajo que la **validación de paquetes**. Siga las instrucciones que se indican en [Execute Package Validation tests](azure-stack-vaas-validate-oem-package.md#run-package-validation-tests) (Ejecución de pruebas de Package Validation).

No es necesario solicitar la firma de paquetes para validaciones de actualización.

## <a name="next-steps"></a>Pasos siguientes

- [Supervisión y administración de pruebas en el portal de VaaS](azure-stack-vaas-monitor-test.md)
