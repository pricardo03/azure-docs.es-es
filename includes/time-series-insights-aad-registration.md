---
title: archivo de inclusión
description: archivo de inclusión
ms.topic: include
ms.custom: include file
services: time-series-insights
ms.service: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.date: 11/14/2019
ms.openlocfilehash: fa6921f8c4309f17cbd63e242a5416e7e81bb6c3
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2019
ms.locfileid: "74133451"
---
1. En [Azure Portal](https://ms.portal.azure.com/), seleccione **Azure Active Directory** > **Registros de aplicaciones** > **Nuevo registro**.

   [![Nuevo registro de aplicaciones en Azure Active Directory](media/time-series-insights-aad-registration/active-directory-new-application-registration.png)](media/time-series-insights-aad-registration/active-directory-new-application-registration.png#lightbox)

    La aplicación se mostrará aquí después de registrarla.

1. Asigne un nombre a la aplicación y seleccione **Solo las cuentas de este directorio organizativo** para especificar los **tipos de cuenta admitidos** que pueden acceder a la API. Elija un URI válido al que redirigir a los usuarios después de autenticarse y, luego, seleccione **Registrar**.

   [![Creación de la aplicación en Azure Active Directory](media/time-series-insights-aad-registration/active-directory-registration.png)](media/time-series-insights-aad-registration/active-directory-registration.png#lightbox)

1. En la hoja **Información general** de la aplicación mostrada, aparece información importante de la aplicación de Azure Active Directory. Seleccione la aplicación en **Aplicaciones propias** y, luego, **Información general**.

   [![Copia del identificador de la aplicación](media/time-series-insights-aad-registration/active-directory-copy-application-id.png)](media/time-series-insights-aad-registration/active-directory-copy-application-id.png#lightbox)

   Copie el **identificador de la aplicación (cliente)** para usar en la aplicación cliente.

1. En la hoja **Autenticación** se especifican valores de configuración de autenticación importantes. 

    1. El valor de **URI de redirección** debe coincidir con la dirección proporcionada en la solicitud de autenticación:

        * Si se trata de aplicaciones hospedadas en un entorno de desarrollo local, seleccione **Cliente público (móvil y escritorio)** . Asegúrese de establecer el **tipo de cliente predeterminado** en Sí.
        * Si son aplicaciones de página única hospedadas en Azure App Service, seleccione **Web**.

    1. Marque **Tokens de acceso** o **Tokens de id.** para habilitar el flujo de concesión implícita.

   [![Creación de un secreto de cliente](media/time-series-insights-aad-registration/active-directory-auth-blade.png)](media/time-series-insights-aad-registration/active-directory-auth-blade.png#lightbox)

   Haga clic en **Save**(Guardar).

1. Seleccione **Certificados y secretos** y, después, haga clic en **Nuevo secreto de cliente** para crear una contraseña de aplicación que el cliente puede usar para demostrar su identidad.

   [![Creación de un secreto de cliente](media/time-series-insights-aad-registration/active-directory-application-keys-save.png)](media/time-series-insights-aad-registration/active-directory-application-keys-save.png#lightbox)

   Se mostrará la contraseña del secreto de cliente. Copie la clave en el editor de texto que prefiera.

   > [!NOTE]
   > En su lugar, también puede importar un certificado. Para mejorar la seguridad, le recomendamos que use un certificado. Para usar un certificado, seleccione **Cargar certificado**.

1. Asocie la aplicación de Azure Active Directory a Azure TIme Series Insights. Seleccione **Permisos de API** > **Agregar un permiso** > **API usadas en mi organización**. 

    [![Asociación de una API a la aplicación de Azure Active Directory](media/time-series-insights-aad-registration/active-directory-app-api-permission.png)](media/time-series-insights-aad-registration/active-directory-app-api-permission.png#lightbox)

   Escriba `Azure Time Series Insights` en la barra de búsqueda y, luego, seleccione `Azure Time Series Insights`.

1. A continuación, especifique el tipo de permiso de API que requiere la aplicación. De forma predeterminada, se resalta la opción **Permisos delegados**. Elija un tipo de permiso y, luego, seleccione **Agregar permisos**.

    [![Especificación del tipo de permiso de API que requiere la aplicación](media/time-series-insights-aad-registration/active-directory-app-permission-grant.png)](media/time-series-insights-aad-registration/active-directory-app-permission-grant.png#lightbox)