---
title: Creación de una cuenta de Video Indexer conectada a Azure | Microsoft Docs
description: En este artículo se muestra cómo crear una cuenta de Video Indexer conectada a Azure.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 08/05/2018
ms.author: juliako
ms.openlocfilehash: 66ba79926a949371cb2280de408835862dd1a099
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2018
ms.locfileid: "41929741"
---
# <a name="create-a-video-indexer-account-connected-to-azure"></a>Creación de una cuenta de Video Indexer conectada a Azure

Al crear una cuenta de Video Indexer, puede elegir una cuenta de evaluación gratuita (donde obtendrá un número determinado de minutos gratuitos de indexación) o una opción de pago (donde no está limitado por la cuota). Con la versión de evaluación gratuita, Video Indexer proporciona hasta 600 horas de indexación gratuita a los usuarios de sitios web y hasta 2400 horas de indexación gratuita a los usuarios de API. Con la opción de pago, se crea una cuenta de Video Indexer que está conectada a su suscripción de Azure y a una cuenta de Azure Media Services. Se paga por minutos de indexación y por los cargos relacionados con la cuenta de Media Services. 

En este artículo se muestra cómo crear una cuenta de Video Indexer que está vinculada a su suscripción de Azure y a una cuenta de Azure Media Services. 

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. 

    Si aún no tiene una suscripción de Azure, [regístrese para obtener una cuenta de evaluación gratuita de Azure](https://azure.microsoft.com/free/).

* Un dominio de Azure Active Directory (AD). 

    Si no tiene un dominio de Azure AD, créelo con su suscripción de Azure. Para más información, consulte [Administración de nombres de dominio personalizados en Azure Active Directory](../../active-directory/users-groups-roles/domains-manage.md).

* Un usuario y un miembro del dominio de Azure AD. Usará a este miembro para conectar su cuenta de Video Indexer a Azure.

    El usuario debe cumplir estos criterios:

    * Ser un usuario de Azure AD con una cuenta profesional o educativa, no una cuenta personal, como outlook.com, live.com o hotmail.com.
        
        ![todos los usuarios de AAD](./media/create-account/all-aad-users.png)

    *  Ser miembro de la suscripción de Azure con un rol de Propietario, o con los roles de Colaborador y Administrador de acceso de usuario. Un usuario se puede agregar dos veces, con 2 roles. Una vez como Colaborador y otra como Administrador de acceso de usuario.

        ![control de acceso](./media/create-account/access-control-iam.png)

* Registrar el proveedor de recursos EventGrid mediante Azure Portal.

    En Azure Portal, vaya a **Suscripciones** > [suscripción] > **Proveedores de recursos** > **Microsoft.EventGrid**. Si no se encuentra en el estado "Registrado", haga clic en **Registrarse**. Tarda unos minutos en registrarse. 

    ![EventGrid](./media/create-account/event-grid.png)

## <a name="connect-to-azure"></a>Conexión a Azure

1. Inicie sesión con ese usuario y haga clic en el botón **Conectarse a Azure**:

    ![conectarse a Azure](./media/create-account/connect-to-azure.png)

2. Cuando aparezca la lista de suscripciones, seleccione la suscripción que desea usar. 

    ![conectar Video Indexer a Azure](./media/create-account/connect-vi-to-azure-subscription.png)

3. Seleccione una región de Azure en las ubicaciones admitidas: Oeste de EE. UU. 2, Europa del Norte o Asia Oriental.
4. En **Cuenta de Azure Media Services**, elija una de estas opciones:

    * Para crear una cuenta de Media Services nueva, seleccione **Crear nuevo grupo de recursos**. Escriba un nombre para el grupo de recursos.

        Azure creará la nueva cuenta en su suscripción, incluida una nueva cuenta de Azure Storage. La nueva cuenta de Media Services tiene una configuración inicial predeterminada con un punto de conexión de streaming y 10 unidades reservadas S3.
    * Para usar una cuenta de Media Services existente, seleccione **Usar el recurso existente**. En la lista de cuentas, seleccione su cuenta.

        La cuenta de Media Services debe tener la misma región que la cuenta de Video Indexer. Para minimizar la duración de la indexación y la reducción en la capacidad de proceso, ajuste el tipo y el número de unidades reservadas para **10 unidades reservadas S3** en su cuenta de Media Services.
    * Para configurar manualmente la conexión, haga clic en el vínculo **Cambiar a la configuración manual** y proporcione la información necesaria:

    ![conectar Video Indexer a Azure](./media/create-account/connect-vi-to-azure-subscription-2.png)

5. Cuando termine, elija **Conectar**. Esta operación puede tardar unos minutos. 

    Después de conectarse a Azure, la nueva cuenta de Video Indexer aparece en la lista de cuentas:

    ![nueva cuenta](./media/create-account/new-account.png)

6. Vaya a la nueva cuenta: 

    ![Cuenta de Video Indexer](./media/create-account/vi-account.png)

## <a name="considerations"></a>Consideraciones

Tenga en cuenta las siguientes consideraciones con relación a Azure Media Services:

* Si se conecta a una nueva cuenta de Media Services, verá un nuevo grupo de recursos, la cuenta de Media Services y una cuenta de almacenamiento en su suscripción de Azure.
* Si se ha conectado a una nueva cuenta de Media Services, Video Indexer establecerá los medios **Unidades reservadas** en 10 unidades S3:

    ![Unidades reservadas de Media Services](./media/create-account/ams-reserved-units.png)

* Si se ha conectado a una cuenta de Media Services existente, Video Indexer no cambia la configuración existente de **Unidades reservadas**.

    Es posible que tenga que ajustar el tipo y número de **Unidades reservadas**, según la carga planeada. Tenga en cuenta que si la carga es alta y no tiene suficientes unidades o velocidad, el procesamiento de los vídeos pueden producir errores de tiempo de espera agotado.

* Si se ha conectado a una nueva cuenta de Media Services, Video Indexer inicia automáticamente un **punto de conexión de streaming** en ella:

    ![Punto de conexión de streaming de Media Services](./media/create-account/ams-streaming-endpoint.png)

* Si se ha conectado a una cuenta de Media Services existente, Video Indexer no cambia la configuración de los puntos de conexión de streaming. Si no hay ningún **punto de conexión de streaming** en ejecución, no podrá ver vídeos desde esta cuenta de Media Services ni en Video Indexer.

## <a name="use-video-indexer-apis-v2"></a>Uso de Video Indexer API v2

Para interactuar mediante programación con su cuenta de evaluación gratuita o con sus cuentas de Video Indexer conectadas a Azure, siga las instrucciones descritas en [Uso de las API](video-indexer-use-apis.md).

Debe utilizar el mismo usuario de Azure AD que usa al conectarse a Azure.

## <a name="next-steps"></a>Pasos siguientes

[Examen detallado de la salida JSON](video-indexer-output-json-v2.md).

