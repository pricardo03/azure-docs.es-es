---
title: Exportar o eliminar la configuración de Azure Portal| Microsoft Docs
description: Obtenga información sobre cómo exportar o eliminar la configuración de usuario, los paneles privados y configuración personalizada en el portal de Azure.
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
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59361956"
---
# <a name="export-or-delete-user-settings"></a>Exportación o eliminación de la configuración de usuario

Puede usar la configuración y las características en el portal de Azure para crear una experiencia personalizada. Información acerca de la configuración personalizada se almacena en Azure. Puede exportar o eliminar los datos de usuario siguientes:

* Paneles privados en el portal de Azure
* Configuración de usuario como suscripciones favoritas o directorios y último directorio ha iniciado sesión
* Los temas y otra configuración del portal personalizado

Es una buena idea para exportar y revisar la configuración antes de eliminarlos. Volver a generar paneles o rehacer la configuración personalizada puede llevar mucho tiempo.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="export-or-delete-your-portal-settings"></a>Exportar o eliminar la configuración del portal

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. En el encabezado del portal, seleccione **configuración**.

    ![Captura de pantalla que muestra el engranaje de configuración del portal](media/azure-portal-export-delete-settings/azure-portal-settings-icon.png)

3. Seleccione **Exportar todas las opciones de configuración** o **Eliminar todas las opciones de configuración y los paneles privados**.

    ![Captura de pantalla que muestra el portal de exportar y eliminar la configuración](media/azure-portal-export-delete-settings/azure-portal-export-delete-settings.png)

      La tabla siguiente describe estas acciones.

      | . | DESCRIPCIÓN |
      | --- | --- |
      | **Exportar todas las opciones de configuración** | Crea un archivo .json que contiene la configuración de usuario como el tema de color, favoritos y los paneles privados.|
      | **Eliminar todas las opciones de configuración y los paneles privados** | Elimina todos los vínculos a los paneles privados y otra configuración personalizada que haya realizado en el portal. |

> [!NOTE]
> Debido a la naturaleza dinámica de la configuración de usuario y el riesgo de daños en los datos, no se puede importar la configuración desde el archivo .json.
>
>


## <a name="next-steps"></a>Pasos siguientes

* [Creación y uso compartido de paneles de Azure](azure-portal-dashboard-share-access.md)
* [Agregar, quitar y ordenar los favoritos](azure-portal-add-remove-sort-favorites.md)
