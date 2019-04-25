---
title: 'Contraseñas: Azure Active Directory prohibidas dinámicamente'
description: Prohibición de contraseñas no seguras del entorno con las contraseñas prohibidas dinámicamente de Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: cc2c125df2e3455b0e90919dbca92fe497a4b1b7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60415723"
---
# <a name="eliminate-bad-passwords-in-your-organization"></a>Elimine las contraseñas incorrectas de su organización

Los líderes del sector recomiendan que no se use la misma contraseña en varios sitios, que sea compleja y que no sea sencilla del tipo Contraseña123. ¿Cómo pueden las organizaciones asegurarse de que los usuarios siguen las instrucciones? ¿Cómo pueden asegurarse de que los usuarios no usan contraseñas comunes o contraseñas conocidas por su uso en vulneraciones recientes de datos?

## <a name="global-banned-password-list"></a>Lista global de contraseñas prohibidas

Microsoft se esfuerza siempre por situarse un paso por delante de los delincuentes cibernéticos. Por ello, el equipo de Azure AD Identity Protection busca continuamente las contraseñas que se utilizan normalmente y aquellas que están en peligro. A continuación, se bloquean esas contraseñas que se consideran demasiado comunes en lo que se denomina la lista global de contraseñas prohibidas. Los delincuentes cibernéticos también usan estrategias similares en sus ataques, por lo que Microsoft no publica el contenido de esta lista públicamente. Estas contraseñas vulnerables se bloquean antes de que se conviertan en una amenaza real para los clientes de Microsoft. Para más información sobre los esfuerzos de seguridad actuales, consulte [Microsoft Security Intelligence Report](https://www.microsoft.com/security/operations/security-intelligence-report) (Informe de inteligencia de seguridad de Microsoft).

## <a name="custom-banned-password-list"></a>Lista personalizada de contraseñas prohibidas

Puede que algunas organizaciones quieran ir un paso más allá y agregar sus propias personalizaciones a la lista global de contraseñas prohibidas con lo que Microsoft llama listas personalizadas de contraseñas prohibidas. Los clientes empresariales como Contoso, podrían bloquear variantes de sus nombres de marcas, términos específicos de la empresa u otros elementos.

La lista personalizada de contraseñas prohibidas y la posibilidad de habilitar la integración local de Active Directory se administra mediante Azure Portal.

![Modificar la lista personalizada de contraseñas prohibidas en métodos de autenticación](./media/concept-password-ban-bad/authentication-methods-password-protection.png)

## <a name="on-premises-hybrid-scenarios"></a>Escenarios híbridos locales

La protección de cuentas que están solo en la nube es muy útil, pero muchas organizaciones conservan escenarios híbridos que incluyen implementaciones locales de Windows Server Active Directory. Es posible instalar protección con contraseña para Windows Server Active Directory agentes locales ampliar las listas de contraseñas prohibidas en su infraestructura de Azure AD. Ahora es obligatorio que los usuarios y administradores que cambien, establezcan o restablezcan contraseñas locales cumplan con la misma directiva de contraseñas que los usuarios que solo están en la nube.

## <a name="how-are-passwords-evaluated"></a>Cómo se evalúan las contraseñas

Cada vez que un usuario cambia o restablece su contraseña, se comprueba la solidez y la complejidad de la nueva contraseña validándola con la lista global y personalizada de contraseñas prohibidas (si esta última está configurada).

Incluso si una contraseña de usuario contiene una contraseña prohibida, la contraseña podría aceptarse si la contraseña global es lo suficientemente segura. Una contraseña recién configurada recorrerá los pasos siguientes para evaluar su solidez general a fin de determinar si debe aceptarse o rechazarse.

### <a name="step-1-normalization"></a>Paso 1: Normalización

En primer lugar, una nueva contraseña pasa por un proceso de normalización. Esto permite que un pequeño conjunto de contraseñas prohibidas se asigne a un conjunto mucho más grande de contraseñas potencialmente inseguras.

La normalización consta de dos partes.  En primer lugar, todas las letras mayúsculas se cambian a minúsculas.  En segundo lugar, se realizan las sustituciones de caracteres comunes, por ejemplo:  

| Letra original  | Letra sustituida |
| --- | --- |
| "0"  | "o" |
| '1'  | "l" |
| "$"  | "s" |
| "\@"  | "a" |

Ejemplo: suponga que está prohibida la contraseña "blank" y que un usuario intenta cambiar su contraseña a "Bl@nK". Aunque "Bl@nk" no está expresamente prohibida, el proceso de normalización convierte esta contraseña a "blank", que es una contraseña no permitida.

### <a name="step-2-check-if-password-is-considered-banned"></a>Paso 2: Comprobar si la contraseña se considera prohibida

#### <a name="fuzzy-matching-behavior"></a>Comportamiento de la coincidencia aproximada

La coincidencia aproximada se usa en la contraseña normalizada para identificar si contiene una contraseña que se encuentra en la lista global o personalizada de contraseñas prohibidas. El proceso de búsqueda de coincidencias se basa en una distancia de edición de una (1) comparación.  

Ejemplo: Suponga que está prohibida la contraseña "abcdef" y que un usuario intenta cambiar su contraseña a una de las siguientes:

"abcdeg"     *(el último carácter se ha cambiado de "f" a "g")* "abcdefg"   *' ("g"anexada al final)* "abcde"      *(se eliminó la "f" final)*

Ninguna de las contraseñas anteriores coincide específicamente con la contraseña prohibida "abcdef". Sin embargo, dado que cada ejemplo se encuentra dentro de una distancia de 1 edición del token prohibido "abcdef", se considera que todas coinciden con "abcdef".

#### <a name="substring-matching-on-specific-terms"></a>Coincidencia de subcadenas (en términos específicos)

La coincidencia de subcadenas se utiliza en la contraseña normalizada para comprobar el nombre y apellido del usuario, así como el nombre del inquilino (tenga en cuenta que no se realiza la coincidencia de nombres de inquilino cuando se validan contraseñas en un controlador de dominio de Active Directory).

Ejemplo: suponga que tenemos un usuario llamado John Doe que desea restablecer su contraseña a "J0hn123fb". Después de la normalización, esta contraseña convertiría en "john123fb". La coincidencia de subcadenas detecta que la contraseña contiene el nombre del usuario "John". Aunque "J0hn123fb" no se encontraba específicamente en ninguna lista de contraseñas prohibidas, la coincidencia de subcadenas encontró "John" en la contraseña. Por lo tanto, esta contraseña se rechazaría.

#### <a name="score-calculation"></a>Cálculo de puntuación

El siguiente paso consiste en identificar todas las instancias de contraseñas prohibidas en la nueva contraseña normalizada del usuario. A continuación:

1. A cada contraseña prohibida que se encuentre en la contraseña de un usuario se le concede un punto.
2. Cada carácter único restante recibe un punto.
3. Una contraseña debe tener al menos 5 puntos para que se acepte.

En los dos ejemplos siguientes, supongamos que Contoso usa la protección de contraseñas de Azure AD y tiene "contoso" en la lista personalizada. Supongamos también que "blank" está en la lista global.

Ejemplo: un usuario cambia su contraseña a "C0ntos0Blank12".

Después de la normalización, esta contraseña se convierte en "contosoblank12". El proceso de coincidencia encuentra que esta contraseña contiene dos contraseñas prohibidas: contoso y blank. La puntuación entonces sería la siguiente:

[contoso] + [en blanco] + [1] + [2] = 4 puntos puesto que esta contraseña es en 5 puntos, se rechazarán.

Ejemplo: Un usuario cambia su contraseña a "ContoS0Bl@nkf9!".

Después de la normalización, esta contraseña se convierte en "contosoblankf9!". El proceso de coincidencia encuentra que esta contraseña contiene dos contraseñas prohibidas: contoso y blank. La puntuación entonces sería la siguiente:

[contoso] + [blank] + [f] + [9] + [!] = 5 puntos. Como esta contraseña tiene al menos 5 puntos, se acepta.

   > [!IMPORTANT]
   > Tenga en cuenta que el algoritmo de contraseñas prohibidas, junto con la lista global, puede cambiar (y de hecho cambia) en cualquier momento en Azure en función de la investigación y el análisis de seguridad en curso. Para el servicio de agente del controlador de dominio local, algoritmos actualizados solo tendrá efecto después de volver a instalar el software del agente de controlador de dominio.

## <a name="license-requirements"></a>Requisitos de licencia

|   | Protección con contraseña de Azure AD con la lista global de contraseñas prohibidas | Protección con contraseña de Azure AD con la lista personalizada de contraseñas prohibidas|
| --- | --- | --- |
| Usuarios solo en la nube | Azure AD Free | Azure AD Premium (P1 o P2) |
| Usuarios sincronizados desde un entorno local de Windows Server Active Directory | Azure AD Premium (P1 o P2) | Azure AD Premium (P1 o P2) |

> [!NOTE]
> Los usuarios de Windows Server Active Directory local que no se sincronizan con Azure Active Directory también aprovechar las ventajas de protección de contraseña de Azure AD basados en licencia existente para los usuarios sincronizados.

Puede encontrar información adicional sobre licencias, incluidos los costos, en la [página de precios de Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="what-do-users-see"></a>Lo que ven los usuarios

Si un usuario intenta restablecer una contraseña a algo que estaría prohibido, verá el siguiente mensaje de error:

Lamentablemente, la contraseña contiene una palabra, frase o patrón que permite adivinar la contraseña fácilmente. Vuelva a intentarlo con una contraseña diferente.

## <a name="next-steps"></a>Pasos siguientes

* [Configuración de la lista personalizada de contraseñas prohibidas](howto-password-ban-bad.md)
* [Habilitar agentes de Protección con contraseña de Azure AD locales](howto-password-ban-bad-on-premises-deploy.md)
