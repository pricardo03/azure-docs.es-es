---
title: Uso de varios inquilinos en Azure Sentinel para proveedores de servicios de MSSP | Microsoft Docs
description: Uso de varios inquilinos en Azure Sentinel para proveedores de servicios de MSSP
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: caa79b572d0024b93abd2d32ca99d92cc2a8b4bb
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/25/2020
ms.locfileid: "77582082"
---
# <a name="work-with-multiple-tenants-in-azure-sentinel"></a>Uso de varios inquilinos en Azure Sentinel 

Si es un proveedor de servicios de seguridad administrada (MSSP) y está usando [Azure Lighthouse](../lighthouse/overview.md) para administrar los centros de operaciones de seguridad (SOC) de sus clientes, podrá administrar los recursos de Azure Sentinel de sus clientes sin necesidad de conectarse directamente al inquilino del cliente, desde su propio inquilino de Azure. 

## <a name="prerequisites"></a>Prerrequisitos
- [Incorporación de Azure Lighthouse](../lighthouse/how-to/onboard-customer.md)
- Para que esto funcione correctamente, el inquilino debe estar registrado en el proveedor de recursos de Azure Sentinel en al menos una suscripción. Si tiene una cuenta de Azure Sentinel registrada en el inquilino, está listo para empezar. Si no es así, en Azure Portal, seleccione **Suscripciones** seguido de **Proveedores de recursos** y, después, busque `Microsoft.Security.Insights` y seleccione **Registrar**.
   ![Comprobar los proveedores de recursos](media/multiple-tenants-service-providers/check-resource-provider.png)
## <a name="how-to-access-azure-sentinel-from-other-tenants"></a>Acceso a Azure Sentinel desde otros inquilinos
1. En **Directorio + suscripción**, seleccione los directorios delegados y las suscripciones en las que se encuentran las áreas de trabajo de Azure Sentinel del cliente.

   ![Generar incidentes de seguridad](media/multiple-tenants-service-providers/directory-subscription.png)

1. Abra Azure Sentinel. Verá todas las áreas de trabajo de las suscripciones seleccionadas y podrá trabajar con ellas sin problemas, como con cualquier área de trabajo de su propio inquilino.

> [!NOTE]
> No podrá implementar conectores en Azure Sentinel desde un área de trabajo administrada. Para implementar un conector, debe iniciar sesión directamente en el inquilino en el que desea implementar un conector y autenticarlo con los permisos necesarios.





## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a administrar varios inquilinos de Azure Sentinel sin problemas. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).

