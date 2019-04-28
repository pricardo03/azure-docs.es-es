---
title: Terminación SSL con certificados de Key Vault
description: Obtenga información sobre cómo integrar puerta de enlace de aplicación de Azure Key Vault para certificados de servidor que se adjuntan a los agentes de escucha HTTPS habilitado.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 4/25/2019
ms.author: victorh
ms.openlocfilehash: 9ab1a42578081d4e6537f221e3f3a8f4c7babde0
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/24/2019
ms.locfileid: "63759619"
---
# <a name="ssl-termination-with-key-vault-certificates"></a>Terminación SSL con certificados de Key Vault

[Azure Key Vault](../key-vault/key-vault-whatis.md) es un almacén de secretos administrados a la plataforma que puede usar para proteger los secretos, claves y certificados SSL. Application Gateway admite la integración con Key Vault (en versión preliminar pública) para los certificados de servidor que se adjuntan a los agentes de escucha HTTPS habilitado. Esta compatibilidad está limitada a v2 SKU de Application Gateway.

> [!IMPORTANT]
> La integración de almacén de claves de puerta de enlace de aplicaciones está actualmente en versión preliminar pública. Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Hay dos modelos para la terminación SSL con esta versión preliminar pública:

- Puede proporcionar explícitamente los certificados SSL asociados al agente de escucha. Este es el modelo tradicional de pasar los certificados SSL en Application Gateway para la terminación SSL.
- También puede proporcionar una referencia a un certificado de Key Vault existente o secreto durante HTTPS habilitado la creación del agente de escucha.

Hay muchas ventajas con la integración de Key Vault, incluido:

- Una mayor seguridad, puesto que los certificados SSL no administran directamente con el equipo de desarrollo de aplicaciones. Integración con Key Vault permite a un equipo de seguridad independientes para aprovisionar, controlar el ciclo de vida y conceder los permisos apropiados para seleccionar las puertas de enlace de aplicaciones a los certificados de acceso almacenados en Key Vault.
- Soporte técnico para importar los certificados existentes en Key Vault o usar API de Key Vault para crear y administrar certificados nuevos con cualquiera de los asociados de almacén de claves de confianza.
- Compatibilidad con los certificados almacenados en Key Vault para renovar automáticamente.

Application Gateway admite actualmente solo certificados de software que se valida. No se admiten certificados de módulo (HSM) validado de seguridad de hardware. Una vez que la puerta de enlace de aplicaciones está configurado para usar certificados de Key Vault, sus instancias de recuperar el certificado de Key Vault e instalación localmente para la terminación SSL. Las instancias de sondean periódicamente también Key Vault en un intervalo de 24 horas para recuperar una versión renovada del certificado, si existe. Si se encuentra un certificado actualizado, el certificado SSL asociado actualmente con el agente de escucha HTTPS se gira automáticamente.

## <a name="how-it-works"></a>Cómo funciona

Integración con Key Vault requiere un proceso de configuración de tres pasos:

1. **Crear la identidad administrada asignada por el usuario**

   Debe crear o volver a usar un usuario existente que se asigna una identidad administrada que Application Gateway se usa para recuperar los certificados de Key Vault en su nombre. ¿Para obtener más información, consulte [What ' s identidades administradas para los recursos de Azure?](../active-directory/managed-identities-azure-resources/overview.md) Este paso crea una nueva identidad en el inquilino de Azure AD, que es la suscripción utilizada para crear la identidad de confianza.
1. **Configurar el almacén de claves**

   A continuación, debe importarlos o crear un nuevo certificado en el almacén de claves utilizado por aplicaciones que se ejecutan a través de Application Gateway. Secreto de Key Vault que se almacena como archivo en PFX de base sin contraseña 64 codificado también se puede usar en este paso. Uso de un tipo de certificado es preferible debido a que se renueven automáticamente capacidades disponibles con objetos de tipo de certificado en Key Vault. Una vez creado un certificado o un secreto, se deben definir las directivas de acceso en el almacén de claves para permitir que la identidad que se deben conceder *obtener* acceso para capturar el secreto.

1. **Configuración de Application Gateway**

   Una vez completados los dos pasos anteriores, puede aprovisionar o modificar una puerta de enlace de aplicaciones existente para usar la identidad administrada de asignadas por el usuario. También configura certificado SSL de la escucha HTTP para que apunte a del completa URI de almacén de claves certificado o identificador de secreto.

![Certificados de Key Vault](media/key-vault-certs/ag-kv.png)

## <a name="next-steps"></a>Pasos siguientes

[Configure la terminación SSL con certificados de Key Vault con Azure PowerShell](configure-keyvault-ps.md).