---
title: Cómo configurar la directiva de Azure Active Directory para Azure Data Catalog
description: Puede encontrar una situación que puede iniciar sesión el portal de Azure Data Catalog, pero cuando se intenta iniciar sesión en la herramienta de registro del origen de datos, se mostrará un mensaje de error.
author: markingmyname
ms.author: maghan
ms.service: data-catalog
ms.topic: conceptual
ms.date: 04/06/2019
ms.openlocfilehash: 558f8845f5469bf157188e20f1ec65a07ff8355f
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59363142"
---
# <a name="azure-active-directory-policy-configuration"></a>Configuración de directivas de Azure Active Directory

Se puede dar el caso de que pueda iniciar sesión en el portal de Azure Data Catalog, pero al intentar iniciar sesión en la herramienta de registro de orígenes de datos, se mostrará un mensaje de error que le impedirá hacerlo. Este error puede producirse cuando se encuentra en la red de empresa o cuando se conecta desde fuera de la red de empresa.

## <a name="registration-tool"></a>Herramienta de registro

La herramienta de registro usa la *autenticación de formularios* para validar los inicios de sesión de usuario en Azure Active Directory. Para que el inicio de sesión sea correcto, un administrador de Azure Active Directory tiene que habilitar la autenticación de formularios en la *directiva de autenticación global*.

Con la directiva de autenticación global se puede habilitar la autenticación de forma independiente para las conexiones de extranet y de intranet, como se muestra en la siguiente imagen. Errores de inicio de sesión pueden producirse si no está habilitada la autenticación de formularios para la red desde la que se conecta.

 ![Directiva de autenticación global de Azure Active Directory](./media/troubleshoot-policy-configuration/global-auth-policy.png)

Para más información, consulte [Configuración de directivas de autenticación](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn486781(v=ws.11)).

## <a name="next-steps"></a>Pasos siguientes

* [Creación de una instancia de Azure Data Catalog](data-catalog-get-started.md)