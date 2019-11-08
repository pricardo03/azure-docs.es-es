---
title: 'Tutorial: Aceptación y recepción de datos mediante Azure Data Share'
description: 'Tutorial: Aceptación y recepción de datos mediante Azure Data Share'
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: tutorial
ms.date: 07/10/2019
ms.openlocfilehash: 9c24f54fe846459187488b0a65b2582914e25e2a
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73499343"
---
# <a name="tutorial-accept-and-receive-data-using-azure-data-share"></a>Tutorial: Aceptación y recepción de datos con Azure Data Share  

En este tutorial aprenderá a aceptar una invitación a un recurso compartido de datos mediante Azure Data Share. En él aprenderá a recibir los datos que se hayan compartido contigo, así como a habilitar un intervalo de actualización periódico para asegurarse de que siempre tiene la instantánea más reciente de los datos que se comparten con usted. 

> [!div class="checklist"]
> * Aceptación de una invitación de Azure Data Share
> * Creación de una cuenta de Azure Data Share
> * Especificación de un destino para los datos
> * Creación de una suscripción en su recurso compartido de datos para la actualización programada

## <a name="prerequisites"></a>Requisitos previos
Para poder aceptar una invitación a compartir datos, previamente debe aprovisionar un número de recursos de Azure, que se enumeran a continuación. 

Asegúrese de que se cumplen todos los requisitos previos antes de aceptar una invitación para compartir datos. 

* Suscripción de Azure: Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
* Una invitación de Azure Data Share: Una invitación de Microsoft Azure con el asunto "Azure Data Share invitation from **<yourdataprovider@domain.com>** " (Invitación para Azure Data Share de yourdataprovider@domain.com).

### <a name="receive-data-into-a-storage-account"></a>Recepción de los datos en una cuenta de almacenamiento: 

* Una cuenta de Azure Storage: si no la tiene, puede crear una cuenta de [Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account). 
* Permiso para agregar la asignación de roles a la cuenta de almacenamiento, que está presente en el permiso *Microsoft.Authorization/role assignments/write*. Este permiso existe en el rol de propietario. 
* Registro de proveedor de recursos para Microsoft.DataShare. Consulte la documentación sobre [proveedores de recursos de Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services) para obtener información sobre cómo hacerlo. 

> [!IMPORTANT]
> Para aceptar y recibir una instancia de Azure Data Share, primero debe registrar el proveedor de recursos Microsoft.DataShare y debe ser propietario de la cuenta de almacenamiento en la que acepta los datos. Siga las instrucciones documentadas en [Solución de problemas de Azure Data Share](data-share-troubleshoot.md) para registrar el proveedor de recursos del recurso compartido de datos y agregarse como propietario de la cuenta de almacenamiento. 

### <a name="receive-data-into-a-sql-based-source"></a>Recepción de datos en un origen basado en SQL:

* Permiso para que la identidad MSI del recurso compartido de datos acceda a Azure SQL Database o Azure SQL Data Warehouse. Esto se puede hacer mediante los siguientes pasos: 
    1. Establézcase como administrador de Azure Active Directory para el servidor.
    1. Conéctese a Azure SQL Database o Data Warehouse mediante Azure Active Directory.
    1. Use el editor de consultas (versión preliminar) para ejecutar el siguiente script con el fin de agregar la identidad MSI del recurso compartido de datos como db_owner. Debe conectarse mediante Active Directory y no con la autenticación de SQL Server. 

```sql
    create user <share_acct_name> from external provider;     
    exec sp_addrolemember db_owner, <share_acct_name>; 
```      
Tenga en cuenta que *<share_acc_name>* es el nombre de la cuenta de Data Share. Si aún no ha creado una cuenta de Data Share, puede volver a este requisito previo más adelante.         

* Acceso al firewall de SQL Server de la IP del cliente: esto se puede hacer mediante los siguientes pasos: 1. Vaya a *Firewalls y redes virtuales*. 2. Haga clic en la alternancia **activado** para permitir el acceso a los servicios de Azure. 

Una vez completados estos requisitos previos, estará listo para recibir datos en la instancia de SQL Server.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Azure Portal](https://portal.azure.com/).

## <a name="open-invitation"></a>Apertura de una invitación

Compruebe si en la bandeja de entrada hay alguna invitación de su proveedor de datos. La invitación es de Microsoft Azure y su título es **Azure Data Share invitation from <yourdataprovider@domain.com>** (Invitación para Azure Data Share de yourdataprovider@domain.com). Anote el nombre del recurso compartido para asegurarse de que acepta el recurso compartido correcto, en caso de que haya varias invitaciones. 

Selecciónelo en **Ver invitación** para ver su invitación en Azure. Pasará a la vista de recursos compartidos recibidos.

![Invitaciones](./media/invitations.png "Lista de invitaciones") 

Seleccione el recurso compartido que desea ver. 

## <a name="accept-invitation"></a>Aceptación de la invitación
Asegúrese de que se han revisado todos los campos, incluido el de **condiciones de uso**. Si acepta las condiciones de uso, se le solicitará que seleccione la casilla para indicarlo. 

![Condiciones de uso](./media/terms-of-use.png "Términos de uso") 

En *Target Data Share Account* (Cuenta de Azure Data Share de destino), seleccione la suscripción y el grupo de recursos en los que va a implementar su instancia de Azure Data Share. 

En el campo **Data Share Account** (Cuenta de Azure Data Share), seleccione **Crear nuevo** si no tiene una cuenta de Azure Data Share existente. En caso contrario, seleccione la cuenta de Azure Data Share existente en la que desea aceptar los datos compartidos. 

En el campo *Received Share Name* (Nombre de recurso compartido recibido), puede dejar el valor predeterminado que ha especificado el proveedor de datos o especifique un nombre nuevo para el recurso compartido recibido. 

![Cuenta de Data Share de destino](./media/target-data-share.png "Cuenta de Data Share de destino") 

Una vez que haya aceptado las condiciones de uso y haya especificado una ubicación para el recurso compartido, seleccione *Accept and Configure* (Aceptar y configurar). Si elige esta opción, se creará una suscripción a un recurso compartido y en la siguiente pantalla se le pedirá que seleccione una cuenta de almacenamiento de destino para que sus datos se copien en ella. 

![Opciones de aceptación](./media/accept-options.png "Opciones de aceptación") 

Si prefiere aceptar la invitación ahora, pero configurar el almacenamiento más adelante, seleccione *Accept and Configure later* (Aceptar y configurar más adelante). Esta opción le permite configurar su cuenta de almacenamiento de destino más adelante. Para continuar con la configuración del almacenamiento más adelante, consulte la página en la que se indica[cómo configurar una cuenta de almacenamiento](how-to-configure-mapping.md), donde encontrará pasos detallados para reanudar la configuración del recurso compartido de datos. 

Si no desea aceptar la invitación, seleccione *Reject* (Rechazar). 

## <a name="configure-storage"></a>Configurar el almacenamiento
En *Target Storage Settings* (Configuración del almacenamiento de destino), seleccione la suscripción, el grupo de recursos y la cuenta de almacenamiento en los que desea recibir los datos. 

![Configuración del almacenamiento de destino](./media/target-storage-settings.png "Almacenamiento de destino") 

Para recibir actualizaciones periódicas de los datos, asegúrese de que habilita la configuración de las instantáneas. Tenga en cuenta que solo verá una programación de la configuración de las instantáneas si el proveedor de datos la ha incluido en el recurso compartido de datos. 

![Configuración de instantáneas](./media/snapshot-settings.png "Configuración de instantáneas") 

Seleccione *Guardar*. 

> [!IMPORTANT]
> Si recibe datos basados en SQL y desea hacerlo en un origen basado en SQL, visite nuestra guía paso a paso de [configuración de una asignación de conjunto de datos](how-to-configure-mapping.md) para aprender a configurar una instancia de SQL Server como destino del conjunto de datos. 

## <a name="trigger-a-snapshot"></a>Desencadenamiento de una instantánea

Para desencadenar a una instantánea, diríjase a la pestaña Received Shares -> Details (Recursos compartidos recibidos -> Detalles) y seleccione**Trigger snapshot** (Desencadenar instantánea). En este caso, puede desencadenar una instantánea completa o incremental de los datos. Si es la primera vez que recibe datos del proveedor de datos, seleccione una copia completa. 

![Desencadenamiento de instantánea](./media/trigger-snapshot.png "Desencadenamiento de instantánea") 

Cuando el último estado de ejecución sea *correcta*, abra la cuenta de almacenamiento para ver los datos recibidos. 

Para seleccionar la cuenta de almacenamiento que ha usado, selecciónela en **Conjuntos de datos**. 

![Conjuntos de datos de consumidor](./media/consumer-datasets.png "Asignación de conjunto de datos de consumidor") 

## <a name="view-history"></a>Visualización del historial
Para ver un historial de las instantáneas, vaya a Received Shares -> History (Recursos compartidos recibidos > Historial). Aquí encontrará un historial de todas las instantáneas que se generaron en los últimos 60 días. 

## <a name="next-steps"></a>Pasos siguientes
En este tutorial, ha aprendido a aceptar y recibir una instancia de Azure Data Share. Para más información acerca de los conceptos de Azure Data Share, diríjase a [Conceptos: terminología de Azure Data Share](terminology.md).