---
title: P+F de la protección con contraseña de Azure AD local
description: P+F de la protección con contraseña de Azure AD local
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 10/30/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.openlocfilehash: 9046090a45d52a283d28d1c4cc79b44503cb5a9d
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54427600"
---
# <a name="preview-azure-ad-password-protection-on-premises---frequently-asked-questions"></a>Vista previa: protección con contraseña de Azure AD local: preguntas más frecuentes

|     |
| --- |
| La protección con contraseña de Azure AD es una característica en versión preliminar pública de Azure Active Directory. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="general-questions"></a>Preguntas generales

**P: ¿Cuándo estará disponible de forma general la protección con contraseña de Azure AD?**

Aún no hemos anunciado ninguna fecha de disponibilidad general.

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
