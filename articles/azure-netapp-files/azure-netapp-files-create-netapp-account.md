---
title: Creación de una cuenta de NetApp para obtener acceso a Azure NetApp Files | Microsoft Docs
description: Describe cómo tener acceso a Azure NetApp Files y crear una cuenta de NetApp para poder configurar un grupo de capacidad y crear un volumen.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/28/2018
ms.author: b-juche
ms.openlocfilehash: bb43a75b6a221c15c8724302797d04c22e04c8d2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61086122"
---
# <a name="create-a-netapp-account"></a>Creación de una cuenta de NetApp
La creación de una cuenta de NetApp le permite configurar un grupo de capacidad y crear después un volumen. Use la hoja de Azure NetApp Files para crear una nueva cuenta de NetApp.

## <a name="before-you-begin"></a>Antes de empezar
Debe haber registrado su suscripción para usar el proveedor de recursos de NetApp y la característica de versión preliminar pública.

[Registro de Azure NetApp Files](azure-netapp-files-register.md)

## <a name="steps"></a>Pasos 

1. Inicie sesión en el Portal de Azure. 
2. Acceda a la hoja de Azure NetApp Files mediante uno de los métodos siguientes:  
   * Busque **Azure NetApp Files** en el cuadro de búsqueda de Azure Portal.  
   * Haga clic en **Todos los servicios** en la navegación y, a continuación, filtre Azure NetApp Files.  

   Puede incluir en "Favoritos" la hoja de Azure NetApp Files haciendo clic en el icono de estrella. 

3. Haga clic en **+ Agregar** para crear una nueva cuenta de NetApp.  
   Aparecerá la ventana de la nueva cuenta de NetApp.  

4. Especifique la siguiente información acerca de la cuenta de NetApp: 
   * **Nombre de cuenta**  
     Especifique un nombre único para la suscripción.
   * **Suscripción**  
     Seleccione una suscripción de las existentes.
   * **Grupo de recursos**   
     Use un grupo de recursos existente o cree uno.
   * **Ubicación**  
     Seleccione la región donde desea que la cuenta y sus recursos secundarios se encuentren.  

     ![Nueva cuenta de NetApp](../media/azure-netapp-files/azure-netapp-files-new-netapp-account.png)


5. Haga clic en **Create**(Crear).     
   La cuenta de NetApp que creó aparece ahora en la hoja de Azure NetApp Files. 

## <a name="next-steps"></a>Pasos siguientes  

[Configuración de un grupo de capacidad](azure-netapp-files-set-up-capacity-pool.md)

