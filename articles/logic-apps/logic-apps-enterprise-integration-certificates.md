---
title: Protección de mensajes B2B con certificados en Azure Logic Apps | Microsoft Docs
description: Adición de certificados para proteger mensajes B2B en Azure Logic Apps con el Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
author: divyaswarnkar
ms.author: divswa
ms.reviewer: estfan, LADocs
manager: jeconnoc
ms.assetid: 4cbffd85-fe8d-4dde-aa5b-24108a7caa7d
ms.suite: integration
ms.topic: article
ms.date: 08/17/2018
ms.openlocfilehash: 38bc1615c0849a33ddfa5790a66fc05d681ce339
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "66167173"
---
# <a name="secure-b2b-messages-with-certificates"></a>Protección de mensajes B2B con certificados

Cuando necesite mantener confidencial la comunicación B2B, puede protegerla para sus aplicaciones de integración empresarial (en particular, las aplicaciones lógicas), al añadir certificados a su cuenta de integración. Los certificados son documentos digitales que comprueban la identidad de los participantes de las comunicaciones electrónicas y ayudan a proteger su comunicación de las siguientes formas:

* Cifrado del contenido de los mensajes.
* Firma digital de los mensajes. 

Puede usar los siguientes certificados en las aplicaciones de integración empresarial:

* Los [Certificados públicos](https://en.wikipedia.org/wiki/Public_key_certificate), que debe adquirir desde la [entidad de certificación (CA)](https://en.wikipedia.org/wiki/Certificate_authority) de una red pública de internet, pero no requieren ninguna clave. 

* Los certificados privados o [ *certificados autofirmados*](https://en.wikipedia.org/wiki/Self-signed_certificate), que crea y emite usted mismo, pero que además requieren claves privadas. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="upload-a-public-certificate"></a>Carga de un certificado público

Para usar un *certificado público* en las aplicaciones lógicas con funcionalidades B2B, antes hay que cargarlo en la cuenta de integración. Después de definir las propiedades en los [contratos](logic-apps-enterprise-integration-agreements.md) que cree, el certificado estará disponible para que pueda proteger los mensajes B2B.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). En el menú principal de Azure, seleccione **Todos los recursos**. En el cuadro de búsqueda, escriba el nombre de la cuenta de integración y, a continuación, seleccione la cuenta de integración que desee.

   ![Busque y seleccione su cuenta de integración](media/logic-apps-enterprise-integration-certificates/select-integration-account.png)  

2. En **Componentes**, seleccione el mosaico **Certificados**.

   ![Elección de "Certificados"](media/logic-apps-enterprise-integration-certificates/add-certificates.png)

3. En **Certificados**, elija **Agregar**. En **Agregar certificado**, proporcione los detalles de su certificado. Cuando termine, elija **Aceptar**.

   | Propiedad | Valor | DESCRIPCIÓN | 
   |----------|-------|-------------|
   | **Nombre** | <*nombre-certificado*> | El nombre del certificado, que es "publicCert" en este ejemplo | 
   | **Tipo de certificado** | Público | Su tipo de certificado |
   | **Certificate** | <*nombre-archivo-del-certificado*> | Para buscar y seleccionar el archivo de certificado que desea cargar, elija el icono de carpeta junto al cuadro **Certificado**. |
   ||||

   ![Elija "Añadir" y proporcione los detalles del certificado](media/logic-apps-enterprise-integration-certificates/public-certificate-details.png)

   Azure valida el certificado que eligió y después lo carga.

   ![Azure muestra el nuevo certificado](media/logic-apps-enterprise-integration-certificates/new-public-certificate.png) 

## <a name="upload-a-private-certificate"></a>Carga de un certificado privado

Para usar un *certificado privado* en las aplicaciones lógicas con funcionalidades B2B, antes hay que cargarlo en la cuenta de integración. También debe tener una clave privada que añadirá primero a [Azure Key Vault](../key-vault/key-vault-get-started.md). 

Después de definir las propiedades en los [contratos](logic-apps-enterprise-integration-agreements.md) que cree, el certificado estará disponible para que pueda proteger los mensajes B2B.

> [!NOTE]
> En el caso de los certificados privados, asegúrese de añadir un certificado público correspondiente para que aparezca en las configuraciones de [contrato AS2](logic-apps-enterprise-integration-as2.md) y **envío y recepción** para la firma y el cifrado de mensajes.

1. [Añada la clave privada a Azure Key Vault](../key-vault/certificate-scenarios.md#import-a-certificate) y especifique un **nombre de clave**.
   
2. Autorice a Azure Logic Apps para que pueda realizar operaciones en Azure Key Vault. Para conceder acceso a la entidad de seguridad de servicio de Logic Apps, use el comando de PowerShell [Set-AzKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy), como en el ejemplo siguiente:

   `Set-AzKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName 
   '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`
 
3. Inicie sesión en el [Azure Portal](https://portal.azure.com). En el menú principal de Azure, seleccione **Todos los recursos**. En el cuadro de búsqueda, escriba el nombre de la cuenta de integración y, a continuación, seleccione la cuenta de integración que desee.

   ![Búsqueda de la cuenta de integración](media/logic-apps-enterprise-integration-certificates/select-integration-account.png) 

4. En **Componentes**, seleccione el mosaico **Certificados**.  

   ![Selección del icono Certificados](media/logic-apps-enterprise-integration-certificates/add-certificates.png)

5. En **Certificados**, elija **Agregar**. En **Agregar certificado**, proporcione los detalles de su certificado. Cuando termine, elija **Aceptar**.

   | Propiedad | Valor | DESCRIPCIÓN | 
   |----------|-------|-------------|
   | **Nombre** | <*nombre-certificado*> | El nombre del certificado, que es "publicCert" en este ejemplo | 
   | **Tipo de certificado** | Privado | Su tipo de certificado |
   | **Certificate** | <*nombre-archivo-del-certificado*> | Para buscar y seleccionar el archivo de certificado que desea cargar, elija el icono de carpeta junto al cuadro **Certificado**. | 
   | **Grupo de recursos** | <*grupo-recursos-de-cuenta-integración*> | Grupo de recursos de su cuenta de integración, que es "MyResourceGroup" en este ejemplo | 
   | **Key Vault** | <*nombre-almacén-claves*> | Nombre de su almacén de claves de Azure |
   | **Key name** | <*key-name*> | Nombre de la clave |
   ||||

   ![Elija "Añadir" y proporcione los detalles del certificado](media/logic-apps-enterprise-integration-certificates/private-certificate-details.png)

   Azure valida el certificado que eligió y después lo carga.

   ![Azure muestra el nuevo certificado](media/logic-apps-enterprise-integration-certificates/new-private-certificate.png) 

## <a name="next-steps"></a>Pasos siguientes

* [Creación de un contrato B2B](logic-apps-enterprise-integration-agreements.md)
