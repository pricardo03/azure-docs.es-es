---
title: archivo de inclusión
description: archivo de inclusión
services: azure-app-configuration
author: yegu
ms.service: azure-app-configuration
ms.topic: include
ms.date: 01/22/2019
ms.author: yegu
ms.custom: include file
ms.openlocfilehash: 9b86f2e05e2cb42470061bd6398b4200607f2418
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66133068"
---
1. Para crear un almacén de configuración de aplicaciones, primero inicie sesión en [Azure Portal](https://aka.ms/azconfig/portal). En la esquina superior izquierda de la página, seleccione **+ Crear un recurso**. En el cuadro **Buscar en Marketplace**, escriba **App Configuration** y presione Entrar.

    ![Buscar App Configuration](../articles/azure-app-configuration/media/quickstarts/azure-app-configuration-new.png)

2. Seleccione **Configuración de la aplicación** en los resultados de la búsqueda y, después, **Crear**.

3. En la página **Configuración de la aplicación** > **Crear**, escriba la siguiente configuración.

    | Configuración | Valor sugerido | DESCRIPCIÓN |
    |---|---|---|
    | **Nombre del recurso** | Nombre único globalmente | Escriba un nombre de recurso único para usarlo en el recurso del almacén de configuración de aplicaciones. El nombre debe ser una cadena de entre 1 y 63 caracteres y solo puede contener números, letras y el carácter `-`. El nombre no puede comenzar ni terminar por el carácter `-` y no se pueden usar varios caracteres `-` consecutivos.  |
    | **Suscripción** | Su suscripción | Seleccione la suscripción de Azure que desee usar para probar App Configuration. Si su cuenta solo dispone de una suscripción, esta se seleccionará automáticamente y la lista desplegable **Suscripción** no aparecerá. |
    | **Grupos de recursos** | *AppConfigTestResources* | Seleccione o cree un grupo de recursos para el recurso del almacén de configuración de aplicaciones. Este grupo es útil para organizar los distintos recursos que quiera eliminar al mismo tiempo mediante la eliminación del grupo de recursos. Para más información, consulte [Administración de los recursos de Azure a través del Portal](/azure/azure-resource-manager/resource-group-overview). |
    | **Ubicación** | *Centro de EE. UU.* | Use **Ubicación** para especificar la ubicación geográfica en la que se hospeda el almacén de App Configuration. Para optimizar el rendimiento, cree el recurso en la misma región que los demás componentes de la aplicación. |

    ![Creación de un recurso del almacén de App Configuration](../articles/azure-app-configuration/media/quickstarts/azure-app-configuration-create.png)

4. Seleccione **Crear**. La implementación puede tardar unos minutos en finalizar.

5. Una vez finalizada la implementación, seleccione **Configuración** > **Claves de acceso**. Anote la cadena de conexión de la clave principal de solo lectura o de la clave principal de lectura y escritura. Usará esta cadena de conexión más adelante para configurar la aplicación y que se comunique con el almacén de configuración de aplicaciones recién creado.
