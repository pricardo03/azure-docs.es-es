---
author: mmacy
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: marsma
ms.openlocfilehash: ce49e9e26160017d64d745c9c7ad5402bc3ae2ca
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73641549"
---
#### <a name="applicationstabapplications"></a>[Aplicaciones](#tab/applications/)

1. Seleccione **Aplicaciones** y, después, seleccione la aplicación web que debe tener acceso a la API. Por ejemplo, *webapp1*.
1. Seleccione **Acceso de API** y, a continuación, seleccione **Agregar**.
1. En la lista desplegable **Seleccionar API** , seleccione la API a la que se debe conceder acceso a la aplicación web. Por ejemplo, *webapi1*.
1. En la lista desplegable **Seleccionar ámbitos**, seleccione los ámbitos que ha definido anteriormente. Por ejemplo, *demo.read* y *demo.write*.
1. Seleccione **Aceptar**.

#### <a name="app-registrations-previewtabapp-reg-preview"></a>[Registros de aplicaciones (versión preliminar)](#tab/app-reg-preview/)

1. Seleccione **Registros de aplicaciones (versión preliminar)** y, después, seleccione la aplicación web que debe tener acceso a la API. Por ejemplo, *webapp1*.
1. En **Administrar**, seleccione **Permisos de API**.
1. En **Permisos configurados**, seleccione **Agregar un permiso**.
1. Seleccione la pestaña **Mis API**.
1. Seleccione la API a la que la aplicación web debe tener acceso. Por ejemplo, *webapi1*.
1. En **Permiso**, expanda **demo** y, a continuación, seleccione los ámbitos que definió anteriormente. Por ejemplo, *demo.read* y *demo.write*.
1. Seleccione **Agregar permisos**. Como se indicó, espere unos minutos antes de continuar con el paso siguiente.
1. Seleccione **Conceder consentimiento de administrador para (el nombre de inquilino)** .
1. Seleccione la cuenta de administrador que tiene actualmente la sesión iniciada o inicie sesión con una cuenta en el inquilino de Azure AD B2C que tenga asignado al menos el rol *Administrador de aplicaciones en la nube*.
1. Seleccione **Aceptar**.
1. Seleccione **Actualizar** y, a continuación, compruebe que aparece "Concedido para..." en **Estado** para ambos ámbitos. Los permisos pueden tardar unos minutos en propagarse.