---
title: Cómo configurar la directiva de Azure Active Directory para Azure Data Catalog
description: Puede encontrar una situación que puede iniciar sesión el portal de Azure Data Catalog, pero cuando se intenta iniciar sesión en la herramienta de registro del origen de datos, se mostrará un mensaje de error.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: conceptual
ms.date: 04/06/2019
ms.openlocfilehash: e69a7e3bd104d0fb82b248b6560d4fd082c88426
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "59996448"
---
# <a name="azure-active-directory-policy-configuration"></a>Configuración de directivas de Azure Active Directory

Se puede dar el caso de que pueda iniciar sesión en el portal de Azure Data Catalog, pero al intentar iniciar sesión en la herramienta de registro de orígenes de datos, se mostrará un mensaje de error que le impedirá hacerlo. Este error puede producirse cuando se encuentra en la red de empresa o cuando se conecta desde fuera de la red de empresa.

## <a name="registration-tool"></a>Herramienta de registro

La herramienta de registro usa la *autenticación de formularios* para validar los inicios de sesión de usuario en Azure Active Directory. Para que el inicio de sesión sea correcto, un administrador de Azure Active Directory tiene que habilitar la autenticación de formularios en la *directiva de autenticación global*.

Con la directiva de autenticación global se puede habilitar la autenticación de forma independiente para las conexiones de extranet y de intranet, como se muestra en la siguiente imagen. Errores de inicio de sesión pueden producirse si no está habilitada la autenticación de formularios para la red desde la que se conecta.

 ![Directiva de autenticación global de Azure Active Directory](./media/troubleshoot-policy-configuration/global-auth-policy.png)

Para más información, consulte [Configuración de directivas de autenticación](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn486781(v=ws.11)).

## <a name="next-steps"></a>Pasos siguientes

* [Crear un catálogo de datos de Azure](data-catalog-get-started.md)