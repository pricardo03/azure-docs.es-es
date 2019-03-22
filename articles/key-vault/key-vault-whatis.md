---
title: ¿Qué es Azure Key Vault? | Microsoft Docs
description: Obtenga información sobre cómo Azure Key Vault protege las claves criptográficas y secretos que las aplicaciones y servicios en la nube usar.
services: key-vault
documentationcenter: ''
author: barclayn
manager: barbkess
tags: azure-resource-manager
ms.assetid: e759df6f-0638-43b1-98ed-30b3913f9b82
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/18/2019
ms.author: barclayn
ms.openlocfilehash: 48ac0c3efe74723099e87a77871aa1a78834efbd
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2019
ms.locfileid: "56958548"
---
# <a name="what-is-azure-key-vault"></a>¿Qué es Azure Key Vault?

Aplicaciones en la nube y los servicios utilizan las claves criptográficas y secretos para ayudar a mantener la información segura. Azure Key Vault protege estos secretos y claves. Al usar Key Vault, puede cifrar las claves de autenticación, claves de cuenta de almacenamiento, claves de cifrado de datos, archivos .pfx y contraseñas mediante el uso de claves que están protegidas por módulos de seguridad de hardware (HSM).

Key Vault ayuda a solucionar los problemas siguientes:

- **Administración de secretos**: Almacenar de forma segura y controlar estrechamente el acceso a los tokens, contraseñas, certificados, claves de API y otros secretos.
- **Administración de claves**: Crear y controlar las claves de cifrado que cifrar los datos. 
- **Administración de certificados**: Aprovisionar, administrar e implementar certificados de Secure Sockets Layer/Transport Layer Security (SSL/TLS) públicos y privados para su uso con Azure y sus recursos internos conectados. 
- **Store secretos respaldados por HSM**: Use el software o validación FIPS 140-2 nivel 2 HSM para ayudar a proteger los secretos y claves.

## <a name="basic-concepts"></a>Conceptos básicos

Azure Key Vault es una herramienta para almacenar y acceder a los secretos de forma segura. Un secreto es todo aquello a lo cual se desea controlar estrechamente el acceso, como certificados, contraseñas o claves de API. Un almacén es un grupo lógico de secretos.

Estos son otros términos importantes:

- **Tenant**: un inquilino es la organización que posee y administra una instancia específica de los servicios en la nube de Microsoft. Lo más a menudo se usa para referirse al conjunto de servicios de Azure y Office 365 para una organización.

- **Propietario del almacén**: un propietario del almacén puede crear un almacén de claves y obtener acceso completo y control sobre él. El propietario del almacén también puede configurar una auditoría para registrar quién accede a los secretos y claves. Los administradores pueden controlar el ciclo de vida de la clave. Pueden revertir a una nueva versión de la clave, realizar copias de seguridad de ella y efectuar otras tareas relacionadas.

- **Consumidor del almacén**: un consumidor del almacén puede realizar acciones en los recursos del almacén de claves si el propietario del almacén le concede acceso de consumidor. Las acciones disponibles dependen de los permisos concedidos.

- **Recursos**: un recurso es un elemento administrable que está disponible a través de Azure. Algunos ejemplos comunes son la máquina virtual, cuenta de almacenamiento, aplicación web, base de datos y red virtual. Hay muchos más.

- **Grupo de recursos**: Un grupo de recursos es un contenedor que almacena los recursos relacionados con una solución de Azure. El grupo de recursos puede incluir todos los recursos de la solución o solo aquellos que se desean administrar como grupo. Para decidir cómo asignar los recursos a los grupos de recursos, tenga en cuenta lo que más conviene a su organización.

- **Entidad de servicio**: Una entidad de servicio de Azure es una identidad de seguridad que usan las aplicaciones creadas por el usuario, servicios y herramientas de automatización para tener acceso a recursos específicos de Azure. Puede considerar como una "identidad de usuario" (nombre de usuario y una contraseña o certificado) con un rol específico y permisos estrechamente controlados. A diferencia de una identidad de usuario general, una entidad de servicio solo necesita poder realizar acciones específicas. Mejora la seguridad si se concede solo el nivel de permiso mínimo que necesita para realizar sus tareas de administración.

- [Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md): Azure AD es el servicio de Active Directory para un inquilino. Cada directorio tiene uno o varios dominios. Un directorio puede tener varias suscripciones asociadas, pero solo un inquilino.

- **Identificador de inquilino de Azure**: un identificador de inquilino es una manera única para identificar una instancia de Azure Active Directory dentro de una suscripción de Azure.

- **Administra las identidades**: Azure Key Vault proporciona una manera de almacenar de forma segura las credenciales y otras claves y secretos, pero el código tiene que autenticarse en Key Vault para recuperarlos. El uso de una identidad administrada permite solucionar este problema más fácilmente, al proporcionar a los servicios de Azure una identidad administrada automáticamente en Azure AD. Puede usar esta identidad para autenticarse Key Vault o en cualquier servicio que admita la autenticación de Azure AD, sin necesidad de tener credenciales en el código. Para obtener más información, vea la siguiente imagen y el [información general de las identidades administradas para los recursos de Azure](../active-directory/managed-identities-azure-resources/overview.md).

    ![Diagrama de las identidades administradas cómo para el trabajo de los recursos de Azure](./media/key-vault-whatis/msi.png)

## <a name="authentication"></a>Authentication
Para realizar cualquier operación con Key Vault, primero debe autenticarse en él. Hay tres maneras para autenticarse en Key Vault:

- [Identidades administradas de recursos de Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview): Al implementar una aplicación en una máquina virtual en Azure, puede asignar una identidad a la máquina virtual que tiene acceso al almacén de claves. También puede asignar las identidades a [otros recursos de Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview). La ventaja de este enfoque es que la aplicación o servicio no está administrando la rotación del primer secreto. Azure alterna automáticamente la identidad. Este enfoque se recomienda como procedimiento recomendado. 
- **Entidad de seguridad y el certificado de servicio**: Puede usar una entidad de servicio y un certificado asociado que tiene acceso al almacén de claves. No se recomienda este enfoque porque el propietario de la aplicación o el desarrollador debe girar el certificado.
- **Entidad de servicio y el secreto**: Aunque puede usar una entidad de servicio y un secreto para autenticarse en Key Vault, no es aconsejable. Es difícil girar automáticamente el secreto de arranque que se utiliza para autenticarse en Key Vault.


## <a name="key-vault-roles"></a>Roles de Key Vault

Utilice la tabla siguiente para comprender mejor cómo Key Vault puede ayudar a satisfacer las necesidades de los programadores y de los administradores de seguridad.

| Rol | Declaración del problema | Resuelto por Azure Key Vault |
| --- | --- | --- |
| Desarrollador para una aplicación de Azure |"Quiero escribir una aplicación de Azure que usa claves de firma y cifrado. Pero quiero que estas claves sean externas a mi aplicación para que la solución sea adecuada para una aplicación distribuida geográficamente. <br/><br/>Quiero que tanto las claves como los secretos estén protegidos sin tener que escribir el código. Por último, quiero poder usar fácilmente las claves y los secretos desde mis aplicaciones, y que el rendimiento sea óptimo". |√ Las claves se almacenan en un almacén y las invoca un identificador URI cuando se necesitan.<br/><br/> √ Las claves se protegen mediante Azure, para lo que se usan algoritmos estándar del sector, longitudes de clave y módulos de seguridad de hardware.<br/><br/>  √ Las claves se procesan en los HSM que residen en los mismos centros de datos de Azure que la aplicaciones. Este método proporciona mayor confiabilidad y menor latencia que las claves que se encuentran en una ubicación independiente, como por ejemplo si son locales. |
| Desarrollador para software como servicio (SaaS) |“No quiero asumir la responsabilidad, ni tampoco la posible responsabilidad, de las claves y los secretos de inquilino de mis clientes. <br/><br/>Quiero que los clientes posean y administren sus claves, de modo que pueda concentrarme en mi trabajo, que es proporcionar las características de software principales". |√ Los clientes pueden importar sus propias claves a Azure y administrarlas. Cuando una aplicación SaaS necesita realizar operaciones criptográficas con claves de los clientes, Key Vault realiza estas operaciones en nombre de la aplicación. La aplicación no ve las claves de los clientes. |
| Responsable principal de la seguridad (CSO) |“Quiero saber que nuestras aplicaciones cumplen con HSM FIPS 140-2 de nivel 2 para administración de claves segura. <br/><br/>Deseo asegurarme de que mi organización tiene el control del ciclo de vida de las claves y puedo supervisar el uso de estas. <br/><br/>Y aunque usamos varios servicios y recursos de Azure, quiero administrar las claves desde una ubicación única en Azure.” |√ Los HSM tienen la validación FIPS 140-2 de nivel 2.<br/><br/>√ Key Vault está diseñado de modo que Microsoft no pueda ver ni extraer sus claves.<br/><br/>El uso de la clave √ se registra en tiempo casi real.<br/><br/>√ El almacén proporciona una sola interfaz, independientemente del número de almacenes que tenga en Azure, las regiones que admitan y las aplicaciones que los usen. |

Cualquiera con una suscripción a Azure puede crear y usar instancias de Key Vault. Aunque Key Vault beneficia a los desarrolladores y administradores de seguridad, pueden implementarse y administrado por el Administrador de la organización que administra otros servicios de Azure. Por ejemplo, este administrador puede iniciar sesión con una suscripción de Azure, cree un almacén para la organización en la que se va a almacenar las claves y asumir la responsabilidad de las tareas operativas como estas:

- Crear o importar una clave o un secreto
- Revocar o eliminar una clave o un secreto
- Autorizar usuarios o aplicaciones para acceder al almacén de claves para que puedan administrar o usar sus claves y secretos
- Configurar el uso de claves (por ejemplo, para firmar o cifrar)
- Supervisar el uso de claves

Este administrador, a continuación, proporciona a los desarrolladores los URI para llamar desde sus aplicaciones. Este administrador también ofrece información de registro de uso de la clave para el Administrador de seguridad. 

![Introducción al funcionamiento de Azure Key Vault][1]

Los desarrolladores también pueden administrar las claves directamente mediante API. Para más información, consulte la [guía para desarrolladores de Key Vault](key-vault-developers-guide.md).

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo [proteger un almacén](key-vault-secure-your-key-vault.md).

<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png
Azure Key Vault está disponible en la mayoría de las regiones. Para obtener más información, consulte la [página de precios de Key Vault](https://azure.microsoft.com/pricing/details/key-vault/).
