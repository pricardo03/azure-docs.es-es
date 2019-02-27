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
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4d3b0f7cdacfb781ba7925be8146c10919c5269b
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2019
ms.locfileid: "56455541"
---
# <a name="preview-azure-ad-password-protection-on-premises---frequently-asked-questions"></a>Vista previa: protección con contraseña de Azure AD local: preguntas más frecuentes

|     |
| --- |
| La protección con contraseña de Azure AD es una característica en versión preliminar pública de Azure Active Directory. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="general-questions"></a>Preguntas generales

**P: ¿Cuándo estará disponible de forma general la protección con contraseña de Azure AD?**

Disponibilidad general prevista para el primer trimestre de 2019 (antes de finales de marzo de 2019). Queremos agradecer a todos los usuarios que han enviado sus comentarios sobre esta característica hasta la fecha.

**P: ¿Qué guía debe proporcionarse a los usuarios sobre cómo seleccionar una contraseña segura?**

La guía actual de Microsoft sobre este tema puede encontrarse en el siguiente vínculo:

[Guía de contraseñas de Microsoft](https://www.microsoft.com/en-us/research/publication/password-guidance)

**P: ¿La protección con contraseña de Azure AD local se admite en las nubes que no son públicas?**

No, la protección con contraseña de Azure AD local solo se admite en las nubes públicas. No se ha anunciado ninguna fecha de disponibilidad de la nube no pública.

**P: ¿Cómo puedo aplicar las ventajas de la protección con contraseña de Azure AD a un subconjunto de mis usuarios locales?**

No compatible. Una vez implementada y habilitada, la característica Protección con contraseña de Azure AD no es discriminatoria: todos los usuarios reciben ventajas de seguridad por igual.

**P: ¿Es posible instalar la protección con contraseña de Azure AD junto con otros productos basados en filtros de contraseña?**

Sí. La compatibilidad con varios archivos DLL de filtro de contraseña registrados es una característica principal de Windows y no es específica de la protección con contraseña de Azure AD. Todos los archivos DLL de filtro de contraseña registrados deben coincidir para que se acepte una contraseña.

**P: ¿Cómo puedo implementar y configurar Protección con contraseña de Azure AD en mi entorno de Active Directory sin usar Azure?**

No compatible. Protección con contraseña de AD Azure es una característica de Azure que puede extenderse a un entorno de Active Directory local.

**P: ¿Cómo puedo modificar el contenido de la directiva en el nivel de Active Directory?**

No compatible. La directiva solo se puede administrar mediante el portal de administración de Azure AD. Consulte también la pregunta anterior.

**P: ¿Por qué se requiere DFSR para la replicación SYSVOL?**

FRS (la tecnología predecesora a DFSR) tiene muchos problemas conocidos y es totalmente incompatible en las versiones más recientes de Windows Server Active Directory. No se hará ninguna prueba de la protección con contraseña de Azure AD en los dominios configurados para FRS.

Para obtener más información, consulte los artículos siguientes:

[The Case for Migrating sysvol replication to DFSR](https://blogs.technet.microsoft.com/askds/2010/04/22/the-case-for-migrating-sysvol-to-dfsr) (El caso de migrar la replicación de SYSVOL a DFSR)

[The End is Nigh for FRS](https://blogs.technet.microsoft.com/filecab/2014/06/25/the-end-is-nigh-for-frs) (El final de FRS está cerca)

**P: ¿Cuánto espacio en disco requiere la característica en el recurso compartido sysvol del dominio?**

El uso exacto del espacio varía puesto que depende de factores como el número y la longitud de los tokens no permitidos en la lista global de Microsoft de no permitidos y la lista personalizada por inquilino, además de la sobrecarga de cifrado. El contenido de estas listas es probable que aumente en el futuro. Teniendo esto en cuenta, la característica debería necesitar al menos cinco (5) megabytes de espacio en el recurso compartido sysvol del dominio.

**P: ¿Por qué se requiere un reinicio para instalar o actualizar el software del agente del controlador de dominio?**

Este requisito lo causa un comportamiento principal de Windows.

**P: ¿Existe alguna forma de configurar un agente de controlador de dominio para que use un servidor proxy específico?**

 No. Puesto que el servidor proxy es sin estado, no es importante qué servidor proxy específico se utilice.

**P: ¿Se puede implementar el servicio de proxy de Protección con contraseña de Azure AD en paralelo a otros servicios como Azure AD Connect?**

Sí. El servicio de proxy de Protección con contraseña de Azure AD y Azure AD Connect no deben nunca entrar en conflicto directamente entre sí.

**P: ¿Debe preocuparme el rendimiento de mis controladores de dominio por implementar esta característica?**

El servicio de agente de controlador de dominio de Protección con contraseña de Azure AD no debería afectar significativamente al rendimiento del controlador de dominio en una implementación existente de Active Directory en buen estado.

En la mayoría de las implementaciones de Active Directory, las operaciones de cambio de contraseña representan una pequeña proporción de la carga de trabajo global de un controlador de dominio determinado. Por ejemplo, imagínese un dominio de Active Directory con 10 000 cuentas de usuario y una directiva MaxPasswordAge establecida en 30 días. De promedio, este dominio sufrirá 10 000/30 = ~333 operaciones de cambio de contraseña cada día; se trata de una cifra baja de operaciones para incluso un solo controlador de dominio. Un escenario con el peor caso posible: imagínese que esos ~333 cambios de contraseña en un único controlador de dominio se realizan en una sola hora. Por ejemplo, este escenario puede ocurrir cuando muchos empleados vienen a trabajar el lunes por la mañana. Incluso en ese caso, se siguen observando ~333/60 minutos = 6 cambios de contraseña por minuto, que tampoco es una carga significativa.

Sin embargo, si los controladores de dominio actuales ya se están ejecutando en los niveles de rendimiento limitado (por ejemplo, si se sobrepasa con respecto a CPU, espacio en disco, E/S de disco, etc.), es conveniente agregar más controladores de dominio o expandir el espacio disponible en disco antes de implementar esta característica. Consulte también la pregunta anterior sobre el uso de espacio en disco de sysvol.

**P: Quiero probar Protección con contraseña de Azure AD en solo unos pocos controladores de dominio de mi dominio. ¿Se pueden forzar los cambios de contraseña de usuario para usar esos controladores de dominio específicos?**

 No. El sistema operativo de cliente Windows controla qué controlador de dominio se utiliza cuando un usuario cambia su contraseña. El controlador de dominio se selecciona en función de diversos factores, como las asignaciones de sitio y subred de Active Directory, la configuración de red específica de entorno, etcétera. Protección con contraseña de Azure AD no controla estos factores y no puede influir en qué controlador de dominio se selecciona para cambiar la contraseña de un usuario.

Una manera de lograr este objetivo sería implementar parcialmente Protección con contraseña de Azure AD en todos los controladores de dominio de un determinado sitio de Active Directory. Este enfoque proporcionará una cobertura razonable a los clientes de Windows que están asignados a ese sitio, y por lo tanto, también a los usuarios que están iniciando sesión en los clientes y cambiando sus contraseñas.

**P: Si instalo el servicio de agente de controlador de dominio de Protección con contraseña de Azure AD solamente en el controlador de dominio principal (PDC), ¿también se protegerán el resto de los controladores de dominio del dominio?**

 No. Cuando se cambia la contraseña de un usuario en un controlador de dominio determinado que no es PDC, nunca se envía la contraseña no cifrada al PDC (esta idea es una percepción errónea común). Una vez que se acepta una contraseña nueva en un controlador de dominio determinado, ese controlador de dominio usa esa contraseña para crear los hashes específicos del protocolo de autenticación de esa contraseña y, a continuación, conserva los hashes en el directorio. La contraseña no cifrada no se mantiene. Los hashes actualizados se replican luego en el PDC. En algunos casos, las contraseñas de usuario se pueden volver a cambiar directamente en el PDC según varios factores, como la topología de red y el diseño del sitio de Active Directory. Consulte la pregunta anterior.

En resumen, se requiere implementar el servicio de agente de controlador de dominio de Protección con contraseña de Azure AD en el PDC para llegar al 100 % de cobertura de seguridad de la característica en todo el dominio. Si se implementa solamente la característica en el PDC, no se proporcionan las ventajas de seguridad de Protección con contraseña de Azure AD para los otros controladores de dominio del dominio.

**P: ¿Hay algún módulo de administración de Operations Manager disponible para Protección con contraseña de Azure AD?**

 No.

## <a name="additional-content"></a>Contenido adicional

Los vínculos siguientes no forman parte de la documentación principal de Protección con contraseña de Azure AD, pero pueden ser una fuente de información adicional útil sobre la característica.

[Email Phishing Protection Guide – Part 15: Implement the Microsoft Azure AD Password Protection Service (for On-Premises too!)](https://blogs.technet.microsoft.com/cloudready/2018/10/14/email-phishing-protection-guide-part-15-implement-the-microsoft-azure-ad-password-protection-service-for-on-premises-too/) (Guía de protección contra la suplantación de identidad en correos electrónicos: Implementación del servicio Protección con contraseña de Microsoft Azure AD)

[Azure AD Password Protection and Smart Lockout are now in Public Preview!](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-Password-Protection-and-Smart-Lockout-are-now-in-Public/ba-p/245423#M529) (Protección con contraseña de Azure AD y Bloqueo inteligente ya están en versión preliminar pública)

## <a name="microsoft-premierunified-support-training-available"></a>Aprendizaje de soporte técnico de Microsoft Premier\Unified disponible

Si le interesa obtener más detalles sobre Protección con contraseña de Azure AD e implementarla en su entorno, puede usar un servicio automático de Microsoft disponible para aquellos clientes que tengan un contrato de soporte técnico Premier o Unified. El servicio se denomina Azure Active Directory: Protección con contraseña. Póngase en contacto con su administrador de cuentas técnica para obtener más información.

## <a name="next-steps"></a>Pasos siguientes

Si tiene alguna pregunta sobre la protección con contraseña de Azure AD local que no aparece respondida aquí, envíe un elemento de comentarios a continuación. ¡Gracias!

[Implementación de la protección de contraseñas de Azure AD](howto-password-ban-bad-on-premises-deploy.md)
