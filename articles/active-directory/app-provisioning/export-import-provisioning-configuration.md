---
title: Exportación o importación de la configuración de aprovisionamiento mediante Graph API | Microsoft Docs
description: Aprenda a exportar e importar una configuración de aprovisionamiento mediante Graph API.
services: active-directory
author: cmmdesai
documentationcenter: na
manager: daveba
ms.assetid: 1a2c375a-1bb1-4a61-8115-5a69972c6ad6
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/09/2019
ms.author: chmutali
ms.collection: M365-identity-device-management
ms.openlocfilehash: af699fa2201bce5627426dcdefc1b98c1d885ae5
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2020
ms.locfileid: "77065763"
---
# <a name="export-or-import-your-provisioning-configuration-by-using-graph-api"></a>Exportación o importación de la configuración de aprovisionamiento mediante Graph API

Puede usar Microsoft Graph API y Graph Explorer para exportar el esquema y las asignaciones de atributos de aprovisionamiento de usuarios a un archivo JSON e importarlos de nuevo en Azure AD. También puede usar los pasos que se indican aquí para crear una copia de seguridad de la configuración de aprovisionamiento. 

## <a name="step-1-retrieve-your-provisioning-app-service-principal-id-object-id"></a>Paso 1: Recuperar el identificador de la entidad de servicio de la aplicación de aprovisionamiento (identificador de objeto)

1. Inicie [Azure Portal](https://portal.azure.com) y vaya a la sección Propiedades de la aplicación de aprovisionamiento. Por ejemplo, si desea exportar la asignación de la *aplicación de aprovisionamiento de usuarios de Workday a AD*, vaya a la sección Propiedades de dicha aplicación. 
1. En la sección Propiedades de la aplicación de aprovisionamiento, copie el valor GUID asociado con el campo *Id. de objeto*. Este valor también se conoce como el elemento **ServicePrincipalId** de la aplicación y se usará en las operaciones del Probador de Graph.

   ![Identificador de la entidad de servicio de la aplicación de Workday](./media/export-import-provisioning-configuration/wd_export_01.png)

## <a name="step-2-sign-into-microsoft-graph-explorer"></a>Paso 2: Iniciar sesión en el Probador de Graph de Microsoft

1. Iniciar el [Probador de Graph de Microsoft](https://developer.microsoft.com/graph/graph-explorer)
1. Haga clic en el botón "Iniciar sesión con Microsoft" e inicie sesión con las credenciales de administrador de la aplicación o de administrador global de Azure AD.

    ![Inicio de sesión en Graph](./media/export-import-provisioning-configuration/wd_export_02.png)

1. Una vez haya iniciado sesión correctamente, verá los detalles de la cuenta de usuario en el panel izquierdo.

## <a name="step-3-retrieve-the-provisioning-job-id-of-the-provisioning-app"></a>Paso 3: Recuperar el identificador del trabajo de aprovisionamiento de la aplicación de aprovisionamiento

En el Probador de Graph de Microsoft, ejecute la siguiente consulta GET; para ello, reemplace [servicePrincipalId] con la propiedad **ServicePrincipalId** extraído del [paso 1](#step-1-retrieve-your-provisioning-app-service-principal-id-object-id).

```http
   GET https://graph.microsoft.com/beta/servicePrincipals/[servicePrincipalId]/synchronization/jobs
```

Obtendrá una respuesta como la que se muestra a continuación. Copie el "atributo de identificador" que aparece en la respuesta. Este valor se corresponde con la propiedad **ProvisioningJobId** y se utilizará para recuperar los metadatos del esquema subyacente.

   [![Id. del trabajo de aprovisionamiento](./media/export-import-provisioning-configuration/wd_export_03.png)](./media/export-import-provisioning-configuration/wd_export_03.png#lightbox)

## <a name="step-4-download-the-provisioning-schema"></a>Paso 4: Descargar el esquema de aprovisionamiento

En el Probador de Graph de Microsoft, ejecute la siguiente consulta GET, reemplace [servicePrincipalId] y [ProvisioningJobId] con las propiedades ServicePrincipalId y ProvisioningJobId recuperadas en los pasos anteriores.

```http
   GET https://graph.microsoft.com/beta/servicePrincipals/[servicePrincipalId]/synchronization/jobs/[ProvisioningJobId]/schema
```

Copie el objeto JSON de la respuesta y guárdelo en un archivo para crear una copia de seguridad del esquema.

## <a name="step-5-import-the-provisioning-schema"></a>Paso 5: Importar el esquema de aprovisionamiento

> [!CAUTION]
> Realice este paso solo si necesita modificar el esquema de configuración y no puede hacerlo en Azure Portal o si necesita restaurar la configuración desde un archivo de copia de seguridad anterior con un esquema válido que funcione.

En el Probador de Graph de Microsoft, configure la siguiente consulta PUT, reemplace [servicePrincipalId] y [ProvisioningJobId] con las propiedades ServicePrincipalId y ProvisioningJobId recuperadas en los pasos anteriores.

```http
    PUT https://graph.microsoft.com/beta/servicePrincipals/[servicePrincipalId]/synchronization/jobs/[ProvisioningJobId]/schema
```

En la pestaña "Cuerpo de la solicitud", copie el contenido del archivo del esquema JSON.

   [![Cuerpo de la solicitud](./media/export-import-provisioning-configuration/wd_export_04.png)](./media/export-import-provisioning-configuration/wd_export_04.png#lightbox)

En la pestaña "Encabezados de solicitud", agregue el atributo de encabezado Content-Type con el valor “application/json”.

   [![Encabezados de solicitud](./media/export-import-provisioning-configuration/wd_export_05.png)](./media/export-import-provisioning-configuration/wd_export_05.png#lightbox)

Haga clic en el botón "Ejecutar consulta" para importar el nuevo esquema.
