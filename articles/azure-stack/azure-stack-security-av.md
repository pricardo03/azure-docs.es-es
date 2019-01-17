---
title: Actualización de Antivirus de Windows Defender en Azure Stack
description: Más información sobre cómo se mantiene actualizado el antivirus en Azure Stack
services: azure-stack
author: PatAltimore
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 01/14/2019
ms.author: patricka
ms.reviewer: fiseraci
ms.openlocfilehash: d47e11be88c4a8ca453475c35e9e0f02d9b531ff
ms.sourcegitcommit: 70471c4febc7835e643207420e515b6436235d29
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2019
ms.locfileid: "54305135"
---
# <a name="update-windows-defender-antivirus-on-azure-stack"></a>Actualización de Antivirus de Windows Defender en Azure Stack

[Antivirus de Windows Defender](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10) es una solución antimalware que proporciona seguridad y protección contra virus. Todos los componentes de la infraestructura de Azure Stack (los hosts de Hyper-V y las máquinas virtuales) están protegidos con el antivirus de Windows Defender. Para obtener una protección actualizada, las definiciones de Antivirus de Windows Defender, el motor y la plataforma requieren actualizaciones periódicas. La forma de aplicar las actualizaciones depende de la configuración.

## <a name="connected-scenario"></a>Escenario conectado

Para las actualizaciones del motor y las definiciones de antimalware, el [proveedor de recursos de actualización](azure-stack-updates.md#the-update-resource-provider) de Azure Stack descarga las definiciones de antimalware y las actualizaciones del motor varias veces al día. Cada componente de la infraestructura de Azure Stack obtiene la actualización del proveedor de recursos de actualización y la aplica de forma automática.

Para las actualizaciones de la plataforma de antimalware, aplique la [actualización mensual de Azure Stack](azure-stack-apply-updates.md). La actualización mensual de Azure Stack incluye las actualizaciones de la plataforma de Antivirus de Windows Defender para el mes en curso.

## <a name="disconnected-scenario"></a>Escenario sin conexión

 Aplique la [actualización mensual de Azure Stack](azure-stack-apply-updates.md). La actualización mensual de Azure Stack incluye las actualizaciones de las definiciones de Antivirus de Windows Defender, el motor y la plataforma para el mes en curso.

## <a name="next-steps"></a>Pasos siguientes

[Más información acerca de la seguridad de Azure Stack](azure-stack-security-foundations.md)
