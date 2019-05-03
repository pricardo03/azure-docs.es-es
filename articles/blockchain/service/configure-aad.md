---
title: Cómo configurar el acceso de Azure Active Directory
description: Cómo configurar el servicio de Azure Blockchain con acceso de Azure Active Directory
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: seal
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: seal
manager: femila
ms.openlocfilehash: 616e342f1d52179c40c225c5dafc9de13ce85e06
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028221"
---
# <a name="how-to-configure-azure-active-directory-access"></a>Cómo configurar el acceso de Azure Active Directory

En este artículo, aprenderá a conceder acceso y conectarse a los nodos de Azure Blockchain Service mediante identificadores de aplicación, grupo o usuario de Azure Active Directory (Azure AD).

Azure AD proporciona administración de identidades basada en la nube y le permite usar una identidad única en una toda enterprise y acceso a las aplicaciones de Azure. Azure Blockchain Service se integra con Azure AD y ofrece ventajas como la federación Id. única inicio de sesión y autenticación multifactor.

## <a name="prerequisites"></a>Requisitos previos

* [Crear a un miembro de cadena de bloques con Azure portal](create-member.md)

## <a name="grant-access"></a>Conceder acceso

Puede conceder acceso en el nivel de miembro y el nivel de nodo. Concede derechos de acceso en el nivel de miembro, a su vez le dará acceso a todos los nodos en el miembro.

### <a name="grant-member-level-access"></a>Conceder acceso de nivel de miembro

Para conceder permiso de acceso en el nivel de miembro.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Vaya a **control de acceso (IAM) > Agregar > Agregar asignación de roles**.
1. Seleccione el **acceso de nodo de miembro de cadena de bloques (versión preliminar)** rol y agregue el objeto de identificador de Azure AD que desea conceder acceso a. Puede ser objeto de identificador de AD Azure:

    | Objeto de Azure AD | Ejemplo |
    |-----------------|---------|
    | Usuario de Azure AD   | `frank@contoso.onmicrosoft.com` |
    | Grupo de Azure AD  | `sales@contoso.onmicrosoft.com` |
    | Identificador de aplicación  | `13925ab1-4161-4534-8d18-812f5ca1ab1e` |

    ![Agregar asignación de roles](./media/configure-aad/add-role-assignment.png)

1. Seleccione **Guardar**.

### <a name="grant-node-level-access"></a>Conceder acceso de nivel de nodo

1. Puede conceder acceso en el nivel de nodo, vaya a la seguridad de nodo y haga clic en el nombre del nodo que desea conceder acceso.
1. Seleccione el rol de acceso de nodo de miembro de cadena de bloques (versión preliminar) y agregue el objeto de identificador de Azure AD que desea conceder acceso a. 

## <a name="connect-using-azure-blockchain-connector"></a>Conectarse mediante el conector de Azure Blockchain

Descargue o clone el [conector de Azure Blockchain desde GitHub](https://github.com/Microsoft/azure-blockchain-connector/).

```bash
git clone https://github.com/Microsoft/azure-blockchain-connector.git
```

La sección del tutorial, siga el **Léame** para crear el conector desde el código fuente.

### <a name="connect-using-an-azure-ad-user-account"></a>Conectar con una cuenta de usuario de Azure AD

1. Ejecute el siguiente comando para autenticarse con una cuenta de usuario de Azure AD. Reemplace \<myAADDirectory\> con un dominio de Azure AD. Por ejemplo, `yourdomain.onmicrosoft.com`.

    ```
    connector.exe -remote <myMemberName>.blockchain.azure.com:3200 -method aadauthcode -tenant-id <myAADDirectory> 
    ```

1. Azure AD le pide las credenciales.
1. Inicie sesión con su nombre de usuario y contraseña.
1. Tras una autenticación correcta, el proxy local se conecta al nodo de la cadena de bloques. Ahora puede adjuntar al cliente Geth con el extremo local.

    ```bash
    geth attach http://127.0.0.1:3100
    ```

### <a name="connect-using-an-application-id"></a>Conectarse mediante un identificador de aplicación

Muchas aplicaciones se autentican con Azure AD mediante un identificador de aplicación en lugar de una cuenta de usuario de Azure AD.

Para conectarse a su nodo mediante un identificador de aplicación, reemplace **aadauthcode** con **aadclient**.

```
connector.exe -remote <myBlockchainEndpoint>  -method aadclient -client-id <myClientID> -client-secret "<myClientSecret>" -tenant-id <myAADDirectory>
```

| Parámetro | DESCRIPCIÓN |
|-----------|-------------|
| tenant-id | Dominio de Azure AD, por ejemplo, `yourdomain.onmicrosoft.com`
| client-id | Identificador de cliente de la aplicación registrada en Azure AD
| client-secret | Secreto de cliente de la aplicación registrada en Azure AD

Para obtener más información sobre cómo registrar una aplicación en Azure AD, consulte [Cómo: Uso del portal para crear una aplicación de Azure AD y una entidad de servicio con acceso a los recursos](../../active-directory/develop/howto-create-service-principal-portal.md)

### <a name="connect-a-mobile-device-or-text-browser"></a>Conectarse a un explorador de texto o dispositivo móvil

Para un dispositivo móvil o explorador basado en texto que no sea posible la pantalla emergente de autenticación de Azure AD, Azure AD genera un código de acceso de un solo uso. Puede copiar el código de acceso y continúe con la autenticación de Azure AD en otro entorno.

Para generar el código de acceso, reemplace **aadauthcode** con **aaddevice**. Reemplace \<myAADDirectory\> con un dominio de Azure AD. Por ejemplo, `yourdomain.onmicrosoft.com`.

```
connector.exe -remote <myBlockchainEndpoint>  -method aaddevice -tenant-id <myAADDirectory>
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre la seguridad de datos en Azure Blockchain Service, consulte:

> [!div class="nextstepaction"]
> [Seguridad de Azure Blockchain Service](data-security.md)