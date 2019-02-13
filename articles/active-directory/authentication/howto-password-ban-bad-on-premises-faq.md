---
title: P+F de la protección con contraseña de Azure AD local
description: P+F de la protección con contraseña de Azure AD local
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.openlocfilehash: 62bda3a1c9cb9d53578c2d471b9e63d1f0873234
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2019
ms.locfileid: "55663305"
---
# <a name="preview-azure-ad-password-protection-on-premises---frequently-asked-questions"></a>Vista previa: protección con contraseña de Azure AD local: preguntas más frecuentes

|     |
| --- |
| La protección con contraseña de Azure AD es una característica en versión preliminar pública de Azure Active Directory. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="general-questions"></a>Preguntas generales

**P: ¿Cuándo estará disponible de forma general la protección con contraseña de Azure AD?**

Disponibilidad general prevista para el primer trimestre de 2019 (antes de finales de marzo de 2019). Queremos agradecer a todos los usuarios que han enviado sus comentarios sobre esta característica hasta la fecha.

**P: ¿La protección con contraseña de Azure AD local se admite en las nubes que no son públicas?**

No, la protección con contraseña de Azure AD local solo se admite en las nubes públicas.

**P: ¿Cómo puedo aplicar las ventajas de la protección con contraseña de Azure AD a un subconjunto de mis usuarios locales?**

No compatible. Una vez implementada y habilitada, la protección con contraseña de Azure AD no es discriminatoria: todos los usuarios reciben ventajas de seguridad por igual.

**P: ¿Es posible instalar la protección con contraseña de Azure AD junto con otros productos basados en filtros de contraseña?**

Sí. La compatibilidad con varios archivos DLL de filtro de contraseña registrados es una característica principal de Windows y no es específica de la protección con contraseña de Azure AD. Todos los archivos DLL de filtro de contraseña registrados deben coincidir para que se acepte una contraseña.

**P: ¿Por qué se requiere DFSR para la replicación SYSVOL?**

FRS (la tecnología predecesora a DFSR) tiene muchos problemas conocidos y es totalmente incompatible en las versiones más recientes de Windows Server Active Directory. No se hará ninguna prueba de la protección con contraseña de Azure AD en los dominios configurados para FRS.

Para obtener más información, consulte los artículos siguientes:

[The Case for Migrating sysvol replication to DFSR](https://blogs.technet.microsoft.com/askds/2010/04/22/the-case-for-migrating-sysvol-to-dfsr) (El caso de migrar la replicación de SYSVOL a DFSR)

[The End is Nigh for FRS](https://blogs.technet.microsoft.com/filecab/2014/06/25/the-end-is-nigh-for-frs) (El final de FRS está cerca)

**P: ¿Por qué se requiere un reinicio para instalar o actualizar el software del agente del controlador de dominio?**

Este requisito lo causa un comportamiento principal de Windows.

**P: ¿Existe alguna forma de configurar un agente de controlador de dominio para que use un servidor proxy específico?**

 No.

## <a name="next-steps"></a>Pasos siguientes

Si tiene alguna pregunta sobre la protección con contraseña de Azure AD local que no aparece respondida aquí, envíe un elemento de comentarios a continuación. ¡Gracias!

[Implementación de la protección de contraseñas de Azure AD](howto-password-ban-bad-on-premises-deploy.md)
