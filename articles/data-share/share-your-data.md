---
title: 'Tutorial: Uso compartido de datos con clientes y asociados mediante Azure Data Share, versión preliminar'
description: 'Tutorial: Uso compartido de datos con clientes y asociados mediante Azure Data Share, versión preliminar'
author: joannapea
ms.service: data-share
ms.topic: tutorial
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: 01888f3656765b922c1b646e7ca8e07d81e799f3
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67838413"
---
# <a name="tutorial-share-your-data-using-azure-data-share-preview"></a>Tutorial: Uso compartido de datos mediante Azure Data Share, versión preliminar

En este tutorial aprenderá a configurar una nueva instancia de Azure Data Share y a empezar a compartir sus datos con clientes y asociado externos a la organización de Azure. 

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una instancia de Azure Data Share.
> * Agregar conjuntos de datos a la instancia de Azure Data Share.
> * Habilitar una programación de sincronización para una instancia de Azure Data Share. 
> * Agregar destinatarios a una instancia de Azure Data Share. 

## <a name="prerequisites"></a>Requisitos previos

* Suscripción de Azure: Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
* Una cuenta de Azure Storage: si no la tiene, puede crear una cuenta de [Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)
* Permiso para agregar la asignación de roles a la cuenta de almacenamiento, que está presente en el permiso *Microsoft.Authorization/role assignments/write*. Este permiso existe en el rol de propietario. 
* La dirección de correo electrónico de inicio de sesión de Azure de los destinatarios (con el alias de correo electrónico no funcionará).

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Azure Portal](https://portal.azure.com/).

## <a name="create-a-data-share-account"></a>Creación de una cuenta de Azure Data Share

Cree un recurso de Azure Data Share en un grupo de recursos de Azure.

1. Seleccione el botón **Crear un recurso** (+) de la esquina superior izquierda del portal.

1. Busque *Data Share*.

1. Seleccione Azure Data Share (versión preliminar) y, después, **Crear**.

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

    ![Compartir sus datos](./media/share-receive-data.png "Share your data") 

1. Seleccione **Start sharing your data** (Empezar a compartir datos).

1. Seleccione **Crear**.   

1. Rellene los detalles de Azure Data Share. Especifique un nombre, una descripción del contenido de los recursos compartidos y las condiciones de uso (opcional). 

    ![EnterShareDetails](./media/enter-share-details.png "Especificar detalles del recurso compartido") 

1. Seleccione **Continuar**

1. Para agregar conjuntos de datos a Azure Data Share, seleccione **Add Datasets** (Agregar conjuntos de datos). 

    ![Conjuntos de datos](./media/datasets.png "Datasets")

1. Seleccione el tipo de conjunto de datos que desea agregar. 

    ![AddDatasets](./media/add-datasets.png "Agregar conjunto de datos")    

1. Vaya hasta el objeto que desea compartir y seleccione "Add Datasets" (Agregar conjuntos de datos). 

    ![SelectDatasets](./media/select-datasets.png "Seleccionar conjuntos de datos")    

1. En la pestaña Destinatarios, escriba las direcciones de correo electrónico del consumidor de datos, para lo que debe seleccionar "+ Add Recipient" (+ Agregar a destinatario). 

    ![AddRecipients](./media/add-recipient.png "Agregar destinatarios") 

1. Seleccione **Continuar**

1. Si desea que el consumidor de datos pueda obtener actualizaciones incrementales de los datos, habilite la programación de instantáneas. 

    ![EnableSnapshots](./media/enable-snapshots.png "Habilitar instantáneas") 

1. Seleccione una hora de inicio y un intervalo de periodicidad. 

1. Seleccione **Continuar**

1. En la pestaña Revisar y crear, examine el contenido del paquete, la configuración, los destinatarios y la configuración de la sincronización. Seleccione **Crear**

Ya se ha creado Azure Data Share y el destinatario de Data Share ya está listo para aceptar la invitación. 

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a crear una instancia de Azure Data Share y a invitar a destinatarios. Para obtener información acerca de la forma en que in consumidor de datos puede aceptar y recibir un recurso compartido de datos, vaya al tutorial para [aceptar y recibir datos](subscribe-to-data-share.md). 