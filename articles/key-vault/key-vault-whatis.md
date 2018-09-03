---
title: ¿Qué es Azure Key Vault? | Microsoft Docs
description: Azure Key Vault ayuda a proteger claves criptográficas y secretos usados por servicios y aplicaciones en la nube. Con Azure Key Vault, los usuarios pueden cifrar claves y secretos (por ejemplo, claves de autenticación, claves de cuenta de almacenamiento, claves de cifrado de datos, archivos .PFX y contraseñas) usando claves que están protegidas por módulos de seguridad de hardware (HSM).
services: key-vault
documentationcenter: ''
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e759df6f-0638-43b1-98ed-30b3913f9b82
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/02/2018
ms.author: barclayn
ms.openlocfilehash: 26828efedac9953ce1c7375fc62269e93019ce50
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "43094877"
---
# <a name="what-is-azure-key-vault"></a>¿Qué es Azure Key Vault?

Azure Key Vault ayuda a solucionar los problemas siguientes
- Azure Key Vault se puede utilizar para almacenar de forma segura y controlar estrechamente el acceso a los tokens, contraseñas, certificados, claves de API y otros secretos
- También se puede usar Azure Key Vault como una solución de administración de claves. Azure Key Vault facilita la creación y control de las claves de cifrado utilizadas para cifrar los datos. 
- Azure Key Vault también es un servicio que le permite aprovisionar, administrar e implementar fácilmente certificados públicos y privados de la Capa de sockets seguros y de Seguridad de la capa de transporte (SSL/TLS) para su uso con Azure y sus recursos internos conectados. 
- Las claves y secretos se pueden proteger mediante software o mediante dispositivos HSM validados FIPS 140-2 de nivel 2

## <a name="basic-concepts"></a>Conceptos básicos

Azure Key Vault es una herramienta para almacenar y acceder a los secretos de forma segura. Un secreto es todo aquello a lo cual se desea controlar estrechamente el acceso, como certificados, contraseñas o claves de API.
A continuación se muestran algunos términos clave:
- **Inquilino**: un inquilino es la organización que posee y administra una instancia específica de los servicios en la nube de Microsoft. Se usa a menudo de manera exacta para hacer referencia al conjunto de servicios de Azure y Office 365 de una organización.
- **Propietario del almacén**: un propietario del almacén puede crear un almacén de claves y obtener acceso completo y control sobre él. El propietario del almacén también puede configurar una auditoría para registrar quién accede a los secretos y claves. Los administradores pueden controlar el ciclo de vida de la clave. Pueden revertir a una nueva versión de la clave, realizar copias de seguridad de ella y efectuar otras tareas relacionadas.
- **Recurso**: un recurso es un elemento administrable que está disponible a través de Azure. Algunos recursos comunes son una máquina virtual, una cuenta de almacenamiento, una aplicación web, una base de datos y una red virtual, pero hay muchos más.
- **Grupo de recursos**: un grupo de recursos de Azure es un contenedor que almacena los recursos relacionados con una solución de Azure. El grupo de recursos puede incluir todos los recursos de la solución o solo aquellos que se desean administrar como grupo. Para decidir cómo asignar los recursos a los grupos de recursos, tenga en cuenta lo que más conviene a su organización.
- **Consumidor del almacén**: un consumidor del almacén puede realizar acciones en los recursos del almacén de claves si el propietario del almacén le concede acceso de consumidor. Las acciones disponibles dependen de los permisos concedidos.
- **[Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md)**: Azure AD es el servicio de Active Directory para un inquilino. Cada directorio tiene uno o varios dominios. Un directorio puede tener varias suscripciones asociadas, pero solo un inquilino. 
- **Identificador de inquilino de Azure**: un identificador de inquilino es una manera única para identificar una instancia de Azure Active Directory dentro de una suscripción de Azure.
- **Managed Service Identity**: Azure Key Vault proporciona una manera de almacenar de forma segura las credenciales y otras claves y secretos, pero el código tiene que autenticarse en Key Vault para recuperarlos. Para solucionar este problema, Managed Service Identity proporciona a los servicios de Azure una identidad administrada automáticamente en Azure AD. Puede usar esta identidad para autenticarse Key Vault o en cualquier servicio que admita la autenticación de Azure AD, sin necesidad de tener credenciales en el código. Para más información, consulte [Managed Service Identity para recursos de Azure](../active-directory/managed-service-identity/overview.md).

    ![Diagrama con el funcionamiento de Managed Service Identity](./media/key-vault-whatis/msi.png)

## <a name="key-vault-roles"></a>Roles de Key Vault

Utilice la tabla siguiente para comprender mejor cómo Key Vault puede ayudar a satisfacer las necesidades de los programadores y de los administradores de seguridad.

| Rol | Declaración del problema | Resuelto por Azure Key Vault |
| --- | --- | --- |
| Desarrollador para una aplicación de Azure |“Quiero escribir una aplicación para Azure que use claves para la firma y el cifrado, pero quiero que dichas claves no estén en la propia aplicación, con el fin de que la solución sea adecuada para una aplicación que se distribuya geográficamente. <br/><br/>Quiero que tanto las claves como los secretos estén protegidos sin tener que escribir el código. Por último, quiero poder usar fácilmente las claves y los secretos desde mis aplicaciones, y que el rendimiento sea óptimo". |√ Las claves se almacenan en un almacén y las invoca un identificador URI cuando se necesitan.<br/><br/> √ Las claves se protegen mediante Azure, para lo que se usan algoritmos estándar del sector, longitudes de clave y módulos de seguridad de hardware.<br/><br/> √ Las claves se procesan en los HSM que residen en los mismos centros de datos de Azure que la aplicaciones. Este método proporciona mayor confiabilidad y menor latencia que las claves que se encuentran en una ubicación independiente, como por ejemplo si son locales. |
| Desarrollador para software como servicio (SaaS) |“No quiero asumir la responsabilidad, ni tampoco la posible responsabilidad, de las claves y los secretos de inquilino de mis clientes. <br/><br/>Quiero que los clientes posean y administren sus claves, de modo que pueda concentrarme en mi trabajo, que es proporcionar las características de software principales". |√ Los clientes pueden importar sus propias claves a Azure y administrarlas. Cuando una aplicación SaaS necesita realizar operaciones criptográficas mediante las claves de sus clientes, Key Vault las hace en nombre de la aplicación. La aplicación no ve las claves de los clientes. |
| Responsable principal de la seguridad (CSO) |“Quiero saber que nuestras aplicaciones cumplen con HSM FIPS 140-2 de nivel 2 para administración de claves segura. <br/><br/>Deseo asegurarme de que mi organización tiene el control del ciclo de vida de las claves y puedo supervisar el uso de estas. <br/><br/>Y aunque usamos varios servicios y recursos de Azure, quiero administrar las claves desde una ubicación única en Azure.” |√ Los HSM tienen la validación FIPS 140-2 de nivel 2.<br/><br/>√ Key Vault está diseñado de modo que Microsoft no pueda ver ni extraer sus claves.<br/><br/>El uso de la clave √ se registra en tiempo casi real.<br/><br/>√ El almacén proporciona una sola interfaz, independientemente del número de almacenes que tenga en Azure, las regiones que admitan y las aplicaciones que los usen. |

Cualquiera con una suscripción a Azure puede crear y usar instancias de Key Vault. Aunque Key Vault beneficia a los desarrolladores y los administradores de seguridad, el administrador de una organización que administra otros servicios de Azure, podría implementarlo y administrarlo. Por ejemplo, dicho administrador puede iniciar sesión con una suscripción de Azure, crear un almacén para la organización en el que almacenar las claves y, después, asumir la responsabilidad de las tareas operativas, como:

* Crear o importar una clave o un secreto
* Revocar o eliminar una clave o un secreto
* Autorizar usuarios o aplicaciones para acceder al almacén de claves para que puedan administrar o usar sus claves y secretos
* Configurar el uso de claves (por ejemplo, para firmar o cifrar)
* Supervisar el uso de claves

Este administrador podría ofrecer después a los desarrolladores los URI para llamar desde sus aplicaciones y proporcionar a su administrador de seguridad la información de registro de uso de claves. 

![Información general de Azure Key Vault][1]

Los desarrolladores también pueden administrar las claves directamente mediante API. Para más información, consulte la [guía para desarrolladores de Key Vault](key-vault-developers-guide.md).

## <a name="next-steps"></a>Pasos siguientes

Para consultar un tutorial de introducción para un administrador, consulte [Introducción a Azure Key Vault](key-vault-get-started.md).

Para más información acerca del registro de uso para Key Vault, consulte [Registro de Azure Key Vault](key-vault-logging.md).

Para más información acerca del uso de claves y secretos con Azure Key Vault, consulte [Información acerca de claves, secretos y certificados](https://msdn.microsoft.com/library/azure/dn903623\(v=azure.1\).aspx).

<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png
Azure Key Vault está disponible en la mayoría de las regiones. Para obtener más información, consulte la [página de precios de Key Vault](https://azure.microsoft.com/pricing/details/key-vault/).
