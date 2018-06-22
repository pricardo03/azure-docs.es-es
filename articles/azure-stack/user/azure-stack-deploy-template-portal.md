---
title: Implementación de plantillas con el portal en Azure Stack | Microsoft Docs
description: Aprenda a usar el portal de Azure Stack para implementar plantillas.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: eafa60f2-16c9-4ef1-b724-47709e9ea29e
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: eea4f568f21693764222c8fdbe3316bf6008cc05
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34604240"
---
# <a name="deploy-templates-using-the-azure-stack-portal"></a>Implementación de plantillas con el portal de Azure Stack

*Se aplica a: sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

Puede usar el portal para implementar plantillas de Azure Resource Manager en Azure Stack.

## <a name="to-deploy-a-template"></a>Para implementar una plantilla

1. Inicie sesión en el portal, seleccione **Nuevo** y, a continuación, seleccione **Personalizado**.
2. Seleccione **Implementación de plantillas**.
3. Seleccione **Editar plantilla** y, a continuación, pegue el código de la plantilla JSON en la ventana de código. Seleccione **Guardar**.
4. Seleccione **Editar parámetros**, escriba los valores para los parámetros que se muestran y, a continuación, seleccione **Aceptar**.
5. Seleccione**Suscripción**. Elija la suscripción que quiere usar y, a continuación, seleccione **Aceptar**.
6. Seleccione **Grupo de recursos**. Elija un grupo de recursos existente o cree uno y, a continuación, seleccione **Aceptar**.
7. Seleccione **Crear**. Un icono nuevo en el panel realiza el seguimiento del progreso de la implementación de la plantilla.

## <a name="next-steps"></a>Pasos siguientes

* [Implementación de plantillas con PowerShell](azure-stack-deploy-template-powershell.md)
