---
title: Cuentas de almacenamiento en Azure Stack | Microsoft Azure
description: Aprenda a crear una cuenta de almacenamiento en Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 1/14/2019
ms.author: mabrigg
ms.openlocfilehash: 66a6fa88956db40ced9ee3fd13a0f74585c528fc
ms.sourcegitcommit: 70471c4febc7835e643207420e515b6436235d29
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2019
ms.locfileid: "54303214"
---
# <a name="storage-accounts-in-azure-stack"></a>Cuentas de almacenamiento en Azure Stack
Las cuentas de almacenamiento incluyen Blob service y Table service y el espacio de nombres único para los objetos de datos de almacenamiento. De forma predeterminada, los datos de su cuenta están disponibles solo para usted, el propietario de la cuenta de almacenamiento.

1. En el equipo de la prueba de concepto de Azure Stack, inicie sesión en `https://adminportal.local.azurestack.external` como [un administrador](azure-stack-connect-azure-stack.md) y, a continuación, haga clic en **+ Crear un recurso** > **Datos y almacenamiento**  >  **Cuenta de almacenamiento**.

   ![](media/azure-stack-provision-storage-account/image01.png)
2. En la hoja **Crear cuenta de almacenamiento**, escriba un nombre para la cuenta de almacenamiento. Cree un nuevo **grupo de recursos**, o seleccione uno existente y, a continuación, haga clic en **Crear** para crear la cuenta de almacenamiento.

   ![](media/azure-stack-provision-storage-account/image02.png)
3. Para ver la nueva cuenta de almacenamiento, haga clic en **Todos los recursos** y, a continuación, busque la cuenta de almacenamiento y haga clic en su nombre.

    ![](media/azure-stack-provision-storage-account/image03.png)

### <a name="next-steps"></a>Pasos siguientes
[Utilización de plantillas del Administrador de recursos de Azure](user/azure-stack-arm-templates.md)

[Más información acerca de las cuentas de Almacenamiento de Azure](../storage/common/storage-create-storage-account.md)

[Descargue la guía de validación de almacenamiento coherente con Azure para Azure Stack](https://aka.ms/azurestacktp1doc)
