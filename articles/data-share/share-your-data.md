---
title: 'Tutorial: Uso compartido fuera de la organización mediante Azure Data Share'
description: 'Tutorial: Uso compartido de datos con clientes y asociados mediante Azure Data Share'
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: tutorial
ms.date: 07/10/2019
ms.openlocfilehash: 4ef9256404b0d0d4d6379e4f5a76c0d41a38c7cd
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73499322"
---
# <a name="tutorial-share-data-using-azure-data-share"></a>Tutorial: Uso compartido de datos mediante Azure Data Share  

En este tutorial aprenderá a configurar una nueva instancia de Azure Data Share y a empezar a compartir sus datos con clientes y asociados externos a la organización de Azure. 

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una instancia de Azure Data Share.
> * Agregar conjuntos de datos a la instancia de Azure Data Share.
> * Habilitar una programación de sincronización para una instancia de Azure Data Share. 
> * Agregar destinatarios a una instancia de Azure Data Share. 

## <a name="prerequisites"></a>Requisitos previos

* Suscripción de Azure: Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
* La dirección de correo electrónico de inicio de sesión de Azure del destinatario (con el alias de correo electrónico no funcionará).

### <a name="share-from-a-storage-account"></a>Recurso compartido de una cuenta de almacenamiento:

* Una cuenta de Azure Storage: si no la tiene, puede crear una cuenta de [Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)
* Permiso para agregar la asignación de roles a la cuenta de almacenamiento, que está presente en el permiso *Microsoft.Authorization/role assignments/write*. Este permiso existe en el rol de propietario. 

### <a name="share-from-a-sql-based-source"></a>Recurso compartido de un origen basado en SQL:

* Una instancia de Azure SQL Database o Azure SQL Data Warehouse con las tablas y vistas que desee compartir.
* Permiso para que el recurso compartido acceda al almacenamiento de datos. Esto se puede hacer mediante los siguientes pasos: 
    1. Establézcase como administrador de Azure Active Directory para el servidor.
    1. Conéctese a Azure SQL Database o Data Warehouse mediante Azure Active Directory.
    1. Use el editor de consultas (versión preliminar) para ejecutar el siguiente script con el fin de agregar la identidad MSI del recurso compartido de datos como db_owner. Debe conectarse mediante Active Directory y no con la autenticación de SQL Server. 
    
```sql
    create user <share_acct_name> from external provider;     
    exec sp_addrolemember db_owner, <share_acct_name>; 
```                   
Tenga en cuenta que *<share_acc_name>* es el nombre de la cuenta de Data Share. Si aún no ha creado una cuenta de Data Share, puede volver a este requisito previo más adelante.  

* Acceso al firewall de SQL Server de la IP del cliente: Esto se puede hacer mediante los siguientes pasos: 1. Vaya a *Firewalls y redes virtuales*. 2. Haga clic en la alternancia **activado** para permitir el acceso a los servicios de Azure. 

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Azure Portal](https://portal.azure.com/).

## <a name="create-a-data-share-account"></a>Creación de una cuenta de Azure Data Share

Cree un recurso de Azure Data Share en un grupo de recursos de Azure.

1. Seleccione el botón **Crear un recurso** (+) de la esquina superior izquierda del portal.

1. Busque *Data Share*.

1. Seleccione Data Share y, después, **Crear**.

1. Rellene los detalles básicos del recurso de Azure Data Share con la siguiente información. 

     **Configuración** | **Valor sugerido** | **Descripción del campo**
    |---|---|---|
    | NOMBRE | *datashareacount* | Especifique un nombre para la cuenta del recurso compartido de datos. |
    | Subscription | Su suscripción | Seleccione la suscripción de Azure que desea usar para la cuenta del recurso compartido de datos.|
    | Resource group | *test-resource-group* | Use un grupo de recursos existente o cree uno. |
    | Location | *Este de EE. UU. 2* | Seleccione una región para la cuenta de Azure Data Share.
    | | |

1. Seleccione **Crear** para aprovisionar la cuenta de Azure Data Share. El aprovisionamiento de una nueva cuenta de Azure Data Share normalmente tarda aproximadamente 2 minutos, o menos. 

1. Cuando la implementación se complete, seleccione **Ir al grupo de recursos**.

## <a name="create-a-data-share"></a>Creación de una instancia de Azure Data Share

1. Vaya a la página de información general de Azure Data Share.

    ![Uso compartido de los datos](./media/share-receive-data.png "Uso compartido de los datos") 

1. Seleccione **Start sharing your data** (Empezar a compartir datos).

1. Seleccione **Crear**.   

1. Rellene los detalles de Azure Data Share. Especifique un nombre, una descripción del contenido de los recursos compartidos y las condiciones de uso (opcional). 

    ![EnterShareDetails](./media/enter-share-details.png "Introducción de detalles del recurso compartido") 

1. Seleccione **Continuar**

1. Para agregar conjuntos de datos a Azure Data Share, seleccione **Add Datasets** (Agregar conjuntos de datos). 

    ![Conjuntos de datos](./media/datasets.png "Conjuntos de datos")

1. Seleccione el tipo de conjunto de datos que desea agregar. 

    ![AddDatasets](./media/add-datasets.png "Incorporación de conjuntos de datos")    

1. Vaya hasta el objeto que desea compartir y seleccione "Add Datasets" (Agregar conjuntos de datos). 

    ![SelectDatasets](./media/select-datasets.png "Selección de conjuntos de datos")    

1. En la pestaña Destinatarios, escriba las direcciones de correo electrónico del consumidor de datos, para lo que debe seleccionar "+ Add Recipient" (+ Agregar a destinatario). 

    ![AddRecipients](./media/add-recipient.png "Adición de destinatarios") 

1. Seleccione **Continuar**

1. Si desea que el consumidor de datos pueda obtener actualizaciones incrementales de los datos, habilite la programación de instantáneas. 

    ![EnableSnapshots](./media/enable-snapshots.png "Habilitación de instantáneas") 

1. Seleccione una hora de inicio y un intervalo de periodicidad. 

1. Seleccione **Continuar**

1. En la pestaña Revisar y crear, examine el contenido del paquete, la configuración, los destinatarios y la configuración de la sincronización. Seleccione **Crear**

Ya se ha creado Azure Data Share y el destinatario de Data Share ya está listo para aceptar la invitación. 

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a crear una instancia de Azure Data Share y a invitar a destinatarios. Para obtener información acerca de la forma en que in consumidor de datos puede aceptar y recibir un recurso compartido de datos, vaya al tutorial para [aceptar y recibir datos](subscribe-to-data-share.md). 
