---
title: Contraseñas prohibidas dinámicamente en Azure AD
description: Prohibición de contraseñas no seguras del entorno con las contraseñas prohibidas dinámicamente de Azure AD
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 06/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: rogoya
ms.openlocfilehash: 89cbe386d87c6ccb81df7fabd86b197bb69e41e1
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291960"
---
# <a name="eliminate-bad-passwords-in-your-organization"></a>Elimine las contraseñas incorrectas de su organización

|     |
| --- |
| Protección con contraseña y Lista personalizada de contraseñas prohibidas de Azure AD son versiones preliminares públicas de Azure Active Directory. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Los líderes del sector recomiendan que no se use la misma contraseña en varios sitios, que sea compleja y que no sea sencilla del tipo Contraseña123. ¿Cómo pueden las organizaciones asegurarse de que los usuarios siguen las instrucciones? ¿Cómo pueden asegurarse de que los usuarios no usan contraseñas comunes o contraseñas conocidas por su uso en vulneraciones recientes de datos?

## <a name="global-banned-password-list"></a>Lista global de contraseñas prohibidas

Microsoft se esfuerza siempre por situarse un paso por delante de los delincuentes cibernéticos. Por ello, el equipo de Azure AD Identity Protection busca continuamente las contraseñas que se utilizan normalmente y aquellas que están en peligro. A continuación, se bloquean esas contraseñas que se consideran demasiado comunes en lo que se denomina la lista global de contraseñas prohibidas. Los delincuentes cibernéticos también usan estrategias similares en sus ataques, por lo que Microsoft no publica el contenido de esta lista públicamente. Estas contraseñas vulnerables se bloquean antes de que se conviertan en una amenaza real para los clientes de Microsoft. Para más información sobre los esfuerzos de seguridad actuales, consulte [Microsoft Security Intelligence Report](https://www.microsoft.com/security/intelligence-report) (Informe de inteligencia de seguridad de Microsoft).

## <a name="preview-custom-banned-password-list"></a>Versión preliminar: Lista personalizada de contraseñas prohibidas

Puede que algunas organizaciones quieran ir un paso más allá y agregar sus propias personalizaciones a la lista global de contraseñas prohibidas con lo que Microsoft llama listas personalizadas de contraseñas prohibidas. Los clientes empresariales como Contoso, podrían bloquear variantes de sus nombres de marcas, términos específicos de la empresa u otros elementos.

La lista personalizada de contraseñas prohibidas y la posibilidad de habilitar la integración local de Active Directory se administra mediante Azure Portal.

![Modifique la lista personalizada de contraseñas prohibidas en Método de autenticación de Azure Portal.](./media/concept-password-ban-bad/authentication-methods-password-protection.png)

## <a name="on-premises-hybrid-scenarios"></a>Escenarios híbridos locales

La protección de cuentas que están solo en la nube es muy útil, pero muchas organizaciones conservan escenarios híbridos que incluyen implementaciones locales de Windows Server Active Directory. Es posible instalar Protección con contraseña de Azure AD para agentes de Windows Server Active Directory (versión preliminar) de forma local para ampliar las listas de contraseñas prohibidas para la infraestructura existente. Ahora es obligatorio que los usuarios y administradores que cambien, establezcan o restablezcan contraseñas locales cumplan con la misma directiva de contraseñas que los usuarios que solo están en la nube.

## <a name="how-does-the-banned-password-list-work"></a>Funcionamiento de la lista de contraseñas prohibidas

La lista de contraseñas prohibidas busca coincidencias con las contraseñas de la lista convirtiendo la cadena a letras minúsculas y comparándola con contraseñas prohibidas conocidas con una distancia de edición de 1 con una coincidencia aproximada.

Por ejemplo, la palabra "contraseña" está bloqueada para una organización.
   - Un usuario intenta establecer la contraseña "P@ssword" que, posteriormente, se convierte en "contraseña" y esta se bloquea porque es una variante de "contraseña".
   - Un administrador intenta establecer una contraseña de usuarios en "Contraseña123!" que se convierte en "contraseña123!", y como es una variante de "contraseña", se bloquea.

Cada vez que un usuario restablece o cambia la contraseña de Azure AD, esta pasa a través de este proceso para confirmar que no está en la lista de contraseñas prohibidas. Esta comprobación se incluye en escenarios híbridos que usan restablecimiento de contraseña de autoservicio, sincronización de hash de contraseñas y autenticación de paso a través.

## <a name="license-requirements"></a>Requisitos de licencia

Las ventajas de la lista global de contraseñas prohibidas se aplican a todos los usuarios de Azure Active Directory (Azure AD).

La lista personalizada de contraseñas prohibidas requiere licencias básicas de Azure AD.

Protección con contraseña de Azure AD para Windows Server Active Directory requiere licencias premium de Azure AD. 

Puede encontrar información adicional sobre licencias, incluidos los costos, en la [página de precios de Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="what-do-users-see"></a>Lo que ven los usuarios

Si un usuario intenta restablecer una contraseña a algo que estaría prohibido, verá el siguiente mensaje de error:

Lamentablemente, la contraseña contiene una palabra, frase o patrón que permite adivinar la contraseña fácilmente. Vuelva a intentarlo con una contraseña diferente.

## <a name="next-steps"></a>Pasos siguientes

* [Configuración de la lista personalizada de contraseñas prohibidas](howto-password-ban-bad.md)
* [Habilitar agentes de Protección con contraseña de Azure AD locales](howto-password-ban-bad-on-premises.md)
