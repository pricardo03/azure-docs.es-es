---
title: Complejidad de las contraseñas en Azure Active Directory B2C | Microsoft Docs
description: Configuración de los requisitos de complejidad de contraseñas proporcionados por los consumidores en Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e728fb037895908882bbe6dc7e8414457ccf273f
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54845561"
---
# <a name="azure-ad-b2c-configure-complexity-requirements-for-passwords"></a>Azure AD B2C: Configuración de los requisitos de complejidad para las contraseñas

> [!NOTE]
> **Esta característica está en versión preliminar pública.**

Azure Active Directory B2C (Azure AD B2C) admite el cambio de los requisitos de complejidad de las contraseñas proporcionadas por un usuario final al crear una cuenta.  De forma predeterminada, Azure AD B2C utiliza contraseñas `Strong`.  Azure AD B2C también admite opciones de configuración para controlar la complejidad de las contraseñas que los clientes pueden usar.

## <a name="when-password-rules-are-enforced"></a>Cuándo se aplican las reglas de contraseña

Al realizar el registro o el restablecimiento de contraseña, un usuario final tiene que proporcionar una contraseña que cumpla las reglas de complejidad.  Las reglas de complejidad de contraseña se aplican por flujo de usuario.  Es posible tener un flujo de usuario que exija un pin de cuatro dígitos durante el registro mientras otro flujo de usuario requiere una cadena de ocho caracteres al realizar el registro.  Por ejemplo, puede utilizar un flujo de usuario con complejidad de contraseña diferente para adultos y para niños.

La complejidad de la contraseña nunca se aplica durante el inicio de sesión.  Nunca se pide a los usuarios durante el inicio de sesión que cambien su contraseña porque no cumple los requisitos de complejidad actuales.

Estos son los tipos de flujos de usuario en los que se puede configurar la complejidad de la contraseña:

* Flujo de usuario de registro o de inicio de sesión
* Flujo de usuario de restablecimiento de contraseña
* Directiva personalizada ([Configuración de la complejidad de contraseñas en las directivas personalizadas](active-directory-b2c-reference-password-complexity-custom.md))

## <a name="how-to-configure-password-complexity"></a>Procedimientos para la configuración de la complejidad de contraseñas

1. Abra **Flujos de usuario**.
2. Seleccione un flujo de usuario y haga clic en **Propiedades**.
3. En **Complejidad de la contraseña**, cambie la complejidad de contraseña de este flujo de usuario a **Simple**, **Segura** o **Personalizada**.

### <a name="comparison-chart"></a>Gráfico de comparación

| Complejidad | DESCRIPCIÓN |
| --- | --- |
| Simple | Una contraseña que tenga al menos de 8 a 64 caracteres. |
| Alta | Una contraseña que tenga al menos de 8 a 64 caracteres. Requiere 3 de 4 de minúsculas, mayúsculas, números o símbolos. |
| Personalizado | Esta opción proporciona el máximo control sobre las reglas de complejidad de contraseña.  Permite configurar una longitud personalizada.  También permite aceptar contraseñas compuestas solo por números (PIN). |

## <a name="options-available-under-custom"></a>Opciones disponibles para la personalización

### <a name="character-set"></a>Juego de caracteres

Permite aceptar solo dígitos (PIN) o el juego completo de caracteres.

* **Solo números** permite solamente dígitos (0-9) al escribir una contraseña.
* **Todos** permite cualquier letra, número o símbolo.

### <a name="length"></a>Length

Permite controlar los requisitos de longitud de la contraseña.

* **Longitud mínima** tiene que ser 4 como mínimo.
* **Longitud máxima** tiene que ser mayor o igual que la longitud mínima y como máximo puede ser 64 caracteres.

### <a name="character-classes"></a>Clase de caracteres

Permite controlar los distintos tipos de caracteres utilizados en la contraseña.

* **2 de 4: carácter en minúscula, carácter en mayúscula, número (0-9), símbolo** garantiza que la contraseña contiene al menos dos tipos de caracteres. Por ejemplo, un número y un carácter en minúscula.
* **3 de 4: carácter en minúscula, carácter en mayúscula, número (0-9), símbolo** garantiza que la contraseña contiene al menos dos tipos de caracteres. Por ejemplo, un número, un carácter en minúscula y un carácter en mayúscula.
* **4 de 4: carácter en minúscula, carácter en mayúscula, número (0-9), símbolo** garantiza que la contraseña contiene al menos todos tipos de caracteres.

    > [!NOTE]
    > Solicitar una contraseña de tipo **4 de 4** puede dar lugar a la frustración del usuario final. Algunos estudios han demostrado que este requisito no mejorar la entropía de contraseña. Consulte las [directrices para contraseñas del NIST](https://pages.nist.gov/800-63-3/sp800-63b.html#appA)
