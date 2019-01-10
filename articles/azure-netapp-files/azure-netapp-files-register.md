---
title: Registro en Azure NetApp Files | Microsoft Docs
description: Describe cómo enviar una solicitud para inscribirse en el servicio Azure NetApp Files.
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
ms.date: 01/04/2018
ms.author: b-juche
ms.openlocfilehash: 0a5483cd5a6316cb5d6cba3a110ca757eb2171c8
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2019
ms.locfileid: "54104002"
---
# <a name="register-for-azure-netapp-files"></a>Registro en Azure NetApp Files
Antes de usar Azure NetApp Files, debe enviar una solicitud para inscribirse en el servicio.  Después de la inscripción, podrá registrarse para usar el servicio.

## <a name="request-to-enroll-in-the-service"></a>Solicitud para inscribirse en el servicio
Tiene que formar parte del programa de la versión preliminar pública y figurar en la lista blanca para acceder al proveedor de recursos de Microsoft.NetApp. Para conocer más detalles sobre cómo unirse al programa de versión preliminar pública, visite la [página de registro en la versión preliminar pública de Azure NetApp Files](https://aka.ms/nfspublicpreview). 


## <a name="register-the-netapp-resource-provider"></a>Registro del proveedor de recursos de NetApp

Para usar el servicio, debe registrar el proveedor de recursos de Azure para Azure NetApp Files. 

1. En Azure Portal, haga clic en el icono de Azure Cloud Shell en la esquina superior derecha:

      ![Icono de Azure Cloud Shell](../media/azure-netapp-files/azure-netapp-files-azure-cloud-shell.png)

2. Si tiene varias suscripciones en su cuenta de Azure, seleccione una que figure en la lista blanca para Azure NetApp Files:
    
        az account set --subscription <subscriptionId>

3. En la consola de Azure Cloud Shell, escriba el siguiente comando para comprobar que su suscripción se ha incluido en la lista blanca:
    
        az feature list | grep NetApp

   La salida del comando tiene el siguiente aspecto:
   
       "id": "/subscriptions/<SubID>/providers/Microsoft.Features/providers/Microsoft.NetApp/features/publicPreviewADC",  
       "name": "Microsoft.NetApp/publicPreviewADC" 
       
   `<SubID>` es su identificador de suscripción.

4. En la consola de Azure Cloud Shell, escriba el comando siguiente para registrar el proveedor de recursos de Azure: 
    
        az provider register --namespace Microsoft.NetApp --wait

   El parámetro `--wait` le indica a la consola que espere a que se complete el registro. El proceso de registro puede tardar algún tiempo en completarse.

5. En la consola de Azure Cloud Shell, escriba el comando siguiente para comprobar que se ha registrado el proveedor de recursos de Azure: 
    
        az provider show --namespace Microsoft.NetApp

  La salida del comando tiene el siguiente aspecto:
   
        {
        "id": "/subscriptions/<SubID>/providers/Microsoft.NetApp",
        "namespace": "Microsoft.NetApp", 
        "registrationState": "Registered", 
        "resourceTypes": […. 

   `<SubID>` es su identificador de suscripción.  El valor del parámetro `state` indica `Registered`.

6. Desde Azure Portal, haga clic en la hoja **Suscripciones**.
7. En la hoja Suscripciones, haga clic en su identificador de suscripción. 
8. En la configuración de la suscripción, haga clic en **Proveedores de recursos** para comprobar que el proveedor de Microsoft.NetApp muestra el estado registrado: 

      ![Microsoft.NetApp registrado](../media/azure-netapp-files/azure-netapp-files-registered-resource-providers.png)


## <a name="next-steps"></a>Pasos siguientes  

[Creación de una cuenta de NetApp](azure-netapp-files-create-netapp-account.md)