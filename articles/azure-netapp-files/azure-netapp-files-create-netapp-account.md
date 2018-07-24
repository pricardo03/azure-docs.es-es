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
ms.topic: get-started-article
ms.date: 03/28/2018
ms.author: b-juche
ms.openlocfilehash: ad8cc550ce69e4dc4c19a569718fa873a65b3620
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2018
ms.locfileid: "39010351"
---
# <a name="create-a-netapp-account"></a>Creación de una cuenta de NetApp
La creación de una cuenta de NetApp le permite configurar un grupo de capacidad y crear después un volumen. Use la hoja de Azure NetApp Files para crear una nueva cuenta de NetApp.

## <a name="before-you-begin"></a>Antes de empezar
Debe haber sido incluido en la lista de usuarios permitidos para acceder al proveedor de recursos Azure Microsoft.NetApp y haber configurado el uso del servicio Azure NetApp Files.  

[Página de suscripción de la versión preliminar pública de Azure NetApp Files](https://aka.ms/nfspublicpreview). 

## <a name="steps"></a>Pasos 

1. Busque la dirección URL de la versión preliminar de Azure Portal en la invitación a la versión e inicie sesión en el portal. 
2.  Acceda a la hoja de Azure NetApp Files mediante uno de los métodos siguientes:  
  * Busque **Azure NetApp Files** en el cuadro de búsqueda de Azure Portal.  
  * Haga clic en **Todos los servicios** en la navegación y, a continuación, filtre Azure NetApp Files.  

  Puede incluir en "Favoritos" la hoja de Azure NetApp Files haciendo clic en el icono de estrella. 

3. Haga clic en **+ Agregar** para crear una nueva cuenta de NetApp.  
  Aparecerá la ventana de la nueva cuenta de NetApp.  

4. Especifique la siguiente información acerca de la cuenta de NetApp: 
  * **Nombre de cuenta**  
    Especifique un nombre único para la suscripción.
  *  **Suscripción**  
    Seleccione una suscripción de las existentes.
  * **Grupo de recursos**   
    Use un grupo de recursos existente o cree uno.
  * **Ubicación**  
    Seleccione la región donde desea que la cuenta y sus recursos secundarios se encuentren.  
    Actualmente, el servicio Azure NetApp Files solo se admite en la región Este de EE. UU.  

    ![Nueva cuenta de NetApp](../media/azure-netapp-files/azure-netapp-files-new-netapp-account.png)


5. Haga clic en **Create**(Crear).     
  La cuenta de NetApp que creó aparece ahora en la hoja de Azure NetApp Files. 

## <a name="next-steps"></a>Pasos siguientes  

1. [Configuración de un grupo de capacidad](azure-netapp-files-set-up-capacity-pool.md)
2. [Creación de un volumen de Azure NetApp Files](azure-netapp-files-create-volumes.md)
3. [Configuración de la directiva de exportación para un volumen (opcional)](azure-netapp-files-configure-export-policy.md)
