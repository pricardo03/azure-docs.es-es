---
title: Exportar o eliminar la configuración de Azure Portal| Microsoft Docs
description: Obtenga información sobre cómo exportar o eliminar la configuración de usuario, los paneles privados y la configuración personalizada en Azure Portal.
services: azure-portal
keywords: ''
author: santhoshsomayajula
ms.date: 04/08/2019
ms.topic: conceptual
ms.service: azure-portal
ms.custom: ''
manager: mtillman
ms.author: kfollis
ms.openlocfilehash: fde7ffbaa3ef4d47eea48302a99948932aeb4f00
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60551674"
---
# <a name="export-or-delete-user-settings"></a>Exportación o eliminación de la configuración de usuario

Puede usar la configuración y las características de Azure Portal para crear una experiencia personalizada. La información sobre la configuración personalizada se almacena en Azure. Puede exportar o eliminar los siguientes datos de usuario:

* Paneles privados de Azure Portal
* Configuración del usuario, como suscripciones o directorios favoritos y último directorio de inicio de sesión
* Temas y otra configuración del portal personalizada

Es una buena idea exportar y revisar la configuración antes de eliminarla. Volver a generar paneles o rehacer la configuración personalizada puede llevar mucho tiempo.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="export-or-delete-your-portal-settings"></a>Exportación o eliminación de la configuración del portal

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. En la parte superior del portal, seleccione **Configuración**.

    ![Captura de pantalla que muestra el engranaje de configuración del portal](media/azure-portal-export-delete-settings/azure-portal-settings-icon.png)

3. Seleccione **Exportar todas las opciones de configuración** o **Eliminar todas las opciones de configuración y los paneles privados**.

    ![Captura de pantalla que muestra la configuración de exportación y eliminación del portal](media/azure-portal-export-delete-settings/azure-portal-export-delete-settings.png)

      En la siguiente tabla se describen estas acciones.

      | . | DESCRIPCIÓN |
      | --- | --- |
      | **Exportar todas las opciones de configuración** | Crea un archivo .json que contiene la configuración de usuario, como el tema de color, los favoritos y los paneles privados.|
      | **Eliminar todas las opciones de configuración y los paneles privados** | Elimina todos los vínculos a paneles privados y otras configuraciones personalizadas que haya realizado en el portal. |

> [!NOTE]
> Debido a la naturaleza dinámica de la configuración de usuario y el riesgo de daños en los datos, no se puede importar la configuración desde el archivo .json.
>
>


## <a name="next-steps"></a>Pasos siguientes

* [Creación y uso compartido de paneles de Azure](azure-portal-dashboard-share-access.md)
* [Adición, eliminación y ordenación de favoritos](azure-portal-add-remove-sort-favorites.md)
