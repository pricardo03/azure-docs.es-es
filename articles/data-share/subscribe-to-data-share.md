---
title: 'Tutorial: Aceptación y recepción de datos mediante Azure Data Share, versión preliminar'
description: 'Tutorial: Aceptación y recepción de datos mediante Azure Data Share, versión preliminar'
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: tutorial
ms.date: 07/10/2019
ms.openlocfilehash: 235ef25b2d655c4388dee5bdcf88d179f3373697
ms.sourcegitcommit: e9936171586b8d04b67457789ae7d530ec8deebe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71327397"
---
# <a name="tutorial-accept-and-receive-data-using-azure-data-share-preview"></a>Tutorial: Aceptación y recepción de datos mediante Azure Data Share, versión preliminar

En este tutorial, aprenderá a aceptar una invitación para compartir datos mediante Azure Data Share, versión preliminar. En él aprenderá a recibir los datos que se hayan compartido contigo, así como a habilitar un intervalo de actualización periódico para asegurarse de que siempre tiene la instantánea más reciente de los datos que se comparten con usted. 

> [!div class="checklist"]
> * Aceptación de una invitación de Azure Data Share, versión preliminar
> * Creación de una cuenta de Azure Data Share, versión preliminar
> * Especificación de un destino para los datos
> * Creación de una suscripción en su recurso compartido de datos para la actualización programada

## <a name="prerequisites"></a>Requisitos previos
Para poder aceptar una invitación a compartir datos, previamente debe aprovisionar un número de recursos de Azure, que se enumeran a continuación. 

Asegúrese de que se cumplen todos los requisitos previos antes de aceptar una invitación para compartir datos. 

* Suscripción de Azure: Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
* Una cuenta de Azure Storage: si no la tiene, puede crear una cuenta de [Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account). 
* Una invitación de Azure Data Share: Una invitación de Microsoft Azure con el asunto "Azure Data Share invitation from **<yourdataprovider@domain.com>** " (Invitación para Azure Data Share de yourdataprovider@domain.com).
* Permiso para agregar la asignación de roles a la cuenta de almacenamiento, que está presente en el permiso *Microsoft.Authorization/role assignments/write*. Este permiso existe en el rol de propietario. 
* Registro de proveedor de recursos para Microsoft.DataShare. Consulte la documentación sobre [proveedores de recursos de Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services) para obtener información sobre cómo hacerlo. 

> [!IMPORTANT]
> Para aceptar y recibir una instancia de Azure Data Share, primero debe registrar el proveedor de recursos Microsoft.DataShare y debe ser propietario de la cuenta de almacenamiento en la que acepta los datos. Siga las instrucciones documentadas en [Troubleshoot Azure Data Share Preview](data-share-troubleshoot.md) (Solución de problemas de Azure Data Share, versión preliminar) para registrar el proveedor de recursos del recurso compartido de datos, además de agregarse como propietario de la cuenta de almacenamiento. 

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Azure Portal](https://portal.azure.com/).

## <a name="open-invitation"></a>Apertura de una invitación

Compruebe si en la bandeja de entrada hay alguna invitación de su proveedor de datos. La invitación es de Microsoft Azure y su título es **Azure Data Share invitation from <yourdataprovider@domain.com>** (Invitación para Azure Data Share de yourdataprovider@domain.com). Anote el nombre del recurso compartido para asegurarse de que acepta el recurso compartido correcto, en caso de que haya varias invitaciones. 

Selecciónelo en **Ver invitación** para ver su invitación en Azure. Pasará a la vista de recursos compartidos recibidos.

![Invitaciones](./media/invitations.png "Lista de invitaciones") 

Seleccione el recurso compartido que desea ver. 

## <a name="accept-invitation"></a>Aceptación de la invitación
Asegúrese de que se han revisado todos los campos, incluido el de **condiciones de uso**. Si acepta las condiciones de uso, se le solicitará que seleccione la casilla para indicarlo. 

![Condiciones de uso](./media/terms-of-use.png "Terms of use") 

En *Target Data Share Account* (Cuenta de Azure Data Share de destino), seleccione la suscripción y el grupo de recursos en los que va a implementar su instancia de Azure Data Share. 

En el campo **Data Share Account** (Cuenta de Azure Data Share), seleccione **Crear nuevo** si no tiene una cuenta de Azure Data Share existente. En caso contrario, seleccione la cuenta de Azure Data Share existente en la que desea aceptar los datos compartidos. 

En el campo *Received Share Name* (Nombre de recurso compartido recibido), puede dejar el valor predeterminado que ha especificado el proveedor de datos o especifique un nombre nuevo para el recurso compartido recibido. 

![Cuenta de Azure Data Share de destino](./media/target-data-share.png "Target data share account") 

Una vez que haya aceptado las condiciones de uso y haya especificado una ubicación para el recurso compartido, seleccione *Accept and Configure* (Aceptar y configurar). Si elige esta opción, se creará una suscripción a un recurso compartido y en la siguiente pantalla se le pedirá que seleccione una cuenta de almacenamiento de destino para que sus datos se copien en ella. 

![Opciones para aceptar](./media/accept-options.png "Accept options") 

Si prefiere aceptar la invitación ahora, pero configurar el almacenamiento más adelante, seleccione *Accept and Configure later* (Aceptar y configurar más adelante). Esta opción le permite configurar su cuenta de almacenamiento de destino más adelante. Para continuar con la configuración del almacenamiento más adelante, consulte la página en la que se indica[cómo configurar una cuenta de almacenamiento](how-to-configure-mapping.md), donde encontrará pasos detallados para reanudar la configuración del recurso compartido de datos. 

Si no desea aceptar la invitación, seleccione *Reject* (Rechazar). 

## <a name="configure-storage"></a>Configurar el almacenamiento
En *Target Storage Settings* (Configuración del almacenamiento de destino), seleccione la suscripción, el grupo de recursos y la cuenta de almacenamiento en los que desea recibir los datos. 

![Configuración del almacenamiento de destino](./media/target-storage-settings.png "Almacenamiento de destino") 

Para recibir actualizaciones periódicas de los datos, asegúrese de que habilita la configuración de las instantáneas. Tenga en cuenta que solo verá una programación de la configuración de las instantáneas si el proveedor de datos la ha incluido en el recurso compartido de datos. 

![Configuración de instantáneas](./media/snapshot-settings.png "Snapshot settings") 

Seleccione *Guardar*. 

## <a name="trigger-a-snapshot"></a>Desencadenamiento de una instantánea

Para desencadenar a una instantánea, diríjase a la pestaña Received Shares -> Details (Recursos compartidos recibidos -> Detalles) y seleccione**Trigger snapshot** (Desencadenar instantánea). En este caso, puede desencadenar una instantánea completa o incremental de los datos. Si es la primera vez que recibe datos del proveedor de datos, seleccione una copia completa. 

![Desencadenar instantánea](./media/trigger-snapshot.png "Trigger snapshot") 

Cuando el último estado de ejecución sea *correcta*, abra la cuenta de almacenamiento para ver los datos recibidos. 

Para seleccionar la cuenta de almacenamiento que ha usado, selecciónela en **Conjuntos de datos**. 

![Conjuntos de datos del consumidor](./media/consumer-datasets.png "Asignación de conjuntos de datos del consumidor") 

## <a name="view-history"></a>Visualización del historial
Para ver un historial de las instantáneas, vaya a Received Shares -> History (Recursos compartidos recibidos > Historial). Aquí encontrará un historial de todas las instantáneas que se generaron en los últimos 60 días. 

## <a name="next-steps"></a>Pasos siguientes
En este tutorial, ha aprendido a aceptar y recibir una instancia de Azure Data Share. Para más información acerca de los conceptos de Azure Data Share, diríjase a [Conceptos: terminología de Azure Data Share](terminology.md).