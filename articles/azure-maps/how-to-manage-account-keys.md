---
title: Procedimientos de administración de la cuenta y las claves de Azure Maps | Microsoft Docs
description: Puede usar Azure Portal para administrar su cuenta y sus claves de acceso de Azure Maps.
author: dsk-2015
ms.author: dkshir
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 38926cc42b44e61bd83f6b1e33946a9026cda06d
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2018
ms.locfileid: "38989408"
---
# <a name="how-to-manage-your-azure-maps-account-and-keys"></a>Procedimientos de administración de la cuenta y las claves de Azure Maps

Puede administrar su cuentas y sus claves de Azure Maps mediante Azure Portal. Una vez que tenga una cuenta y una clave, puede implementar las API en su sitio web o aplicación móvil.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="create-a-new-account"></a>Creación de una nueva cuenta

1. Inicie sesión en el [Azure Portal](http://portal.azure.com).

1. Haga clic en **Crear un recurso** en la esquina superior izquierda de Azure Portal.

2. Busque y seleccione **Maps** y, a continuación, haga clic en **Crear**.

3. Escriba la información de la nueva cuenta. 

![Escribir la información de la cuenta en el portal](./media/how-to-manage-account-keys/new-account-portal.png)

## <a name="manage-keys-on-the-account-page"></a>Administración de las claves en la página de la cuenta

Una vez que cree una cuenta, obtendrá dos claves generadas aleatoriamente. Use las claves para autenticarse en las API de Maps cuando desee recuperar datos de mapas o crear una nueva instancia de un mapa de JavaScript. 

Puede encontrar las claves en Azure Portal. Vaya a su cuenta y, a continuación, seleccione **Claves** en el menú.

![Administración de las claves de la cuenta en el portal](./media/how-to-manage-account-keys/account-keys-portal.png)

Desde esta página puede copiar las claves o generar otras nuevas. 

## <a name="delete-an-account"></a>Eliminación de una cuenta

Puede eliminar una cuenta desde Azure Portal. Vaya a la página de información general de la cuenta y seleccione **Eliminar**.

![Eliminación de la cuenta en el portal](./media/how-to-manage-account-keys/account-delete-portal.png)

## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de cómo utilizar la [API de administración de Maps](https://docs.microsoft.com/rest/api/maps-management/accounts) para crear, actualizar y eliminar cuentas de Maps. 