---
author: mmacy
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: marsma
ms.openlocfilehash: 43bcd1f11eb228bd1454b2ad0f2addb851029f2f
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73800109"
---
#### <a name="applicationstabapplications"></a>[Aplicaciones](#tab/applications/)

1. En la página de información general **Aplicación registrada**, seleccione **Configuración**.
1. En **ACCESO DE API**, seleccione **Permisos necesarios**.
1. Seleccione **Microsoft Azure Active Directory**.
1. En **PERMISOS DE LA APLICACIÓN**, seleccione **Leer y escribir datos de directorio**.
1. Seleccione **Guardar**.
1. Haga clic en **Conceder permisos** y, a continuación, haga clic en **Sí**. Los permisos pueden tardar unos minutos en propagarse por completo.

#### <a name="app-registrations-previewtabapp-reg-preview"></a>[Registros de aplicaciones (versión preliminar)](#tab/app-reg-preview/)

1. En **Administrar**, seleccione **Permisos de API**.
1. En **Permisos configurados**, seleccione **Agregar un permiso**.
1. Seleccione **Azure Active Directory Graph**.
1. Seleccione **Permisos de aplicación**.
1. Expanda el **directorio** y seleccione la casilla **Directory.ReadWrite.All**.
1. Seleccione **Agregar permisos**. Tal como se indicó, espere unos minutos antes de continuar con el paso siguiente.
1. Seleccione **Conceder consentimiento de administrador para (el nombre de inquilino)** .
1. Seleccione una cuenta de administrador de inquilinos.
1. Seleccione **Aceptar**.
1. Seleccione **Actualizar** y, a continuación, compruebe que aparece "Concedido para..." en **Estado**. Los permisos pueden tardar unos minutos en propagarse.