---
title: Seguridad de Azure Blockchain Service
description: Azure conceptos Blockchain Service de seguridad y acceso a datos
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: seal
manager: femila
ms.openlocfilehash: dd0a33364ed9395a85478798e47352c533bd47dc
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028206"
---
# <a name="azure-blockchain-service-security"></a>Seguridad de Azure Blockchain Service

Azure Blockchain Service usa varias funcionalidades de Azure para mantener los datos seguros y estén disponibles. Datos están protegidos mediante aislamiento, cifrado y autenticación.

## <a name="isolation"></a>Aislamiento

Recursos de Azure Blockchain Service están aislados en una red privada virtual. Cada nodo de la transacción y la validación es una máquina virtual (VM). Máquinas virtuales en una red virtual no se comunican directamente a las máquinas virtuales en una red virtual diferente. El aislamiento garantiza la comunicación sigan siendo privada en la red virtual. Para obtener más información sobre el aislamiento de red virtual de Azure, consulte [aislamiento en la nube pública de Azure](../../security/azure-isolation.md#networking-isolation).

![Diagrama de red virtual](./media/data-security/vnet.png)

## <a name="encryption"></a>Cifrado

Datos de usuario se almacenan en almacenamiento de Azure. Datos de usuario se cifran en movimiento y en reposo para la seguridad y confidencialidad. Para más información, consulte: [Guía de seguridad de Azure Storage](../../storage/common/storage-security-guide.md).

## <a name="authentication"></a>Authentication

Las transacciones se pueden enviar a los nodos de la cadena de bloques a través de un punto de conexión RPC. Los clientes comunicarse con un nodo de transacciones con un servidor proxy inverso que controla de la autenticación de usuario y cifra los datos a través de SSL.

![Diagrama de autenticación](./media/data-security/authentication.png)

Existen tres modos de autenticación para el acceso RPC.

### <a name="basic-authentication"></a>Autenticación básica

La autenticación básica utiliza un encabezado de autenticación HTTP que contiene el nombre de usuario y la contraseña. Nombre de usuario es el nombre del nodo de la cadena de bloques. Se establece la contraseña durante el aprovisionamiento de un miembro o un nodo. La contraseña puede cambiarse mediante el portal de Azure o la CLI.

### <a name="access-keys"></a>Claves de acceso

Las claves de acceso usan una cadena generada aleatoriamente que se incluyen en la dirección URL del extremo. Dos claves de acceso ayuda a habilitar la rotación de claves. Pueden volver a generar las claves desde el portal de Azure y la CLI.

### <a name="azure-active-directory"></a>Azure Active Directory

Azure Active Directory (Azure AD) basado en notificaciones de usos mecanismo de autenticación donde el usuario se autentica en Azure AD utilizando las credenciales de usuario de Azure AD. Azure AD proporciona administración de identidades basada en la nube y permite a los clientes usar una identidad única a través de un todo aplicaciones empresariales y acceso en la nube. Azure Blockchain Service se integra con habilitación de Id. federación, solo inicio de sesión y autenticación multifactor de Azure AD. Puede asignar a los usuarios, grupos y roles de aplicación en su organización para el acceso de miembro y nodo de cadena de bloques.

El proxy de cliente de Azure AD está disponible en [GitHub](https://github.com/Microsoft/azure-blockchain-connector/releases). El proxy de cliente dirige al usuario a la página de inicio de sesión de Azure AD y obtiene un token de portador tras una autenticación correcta. Posteriormente, el usuario conecta a una aplicación de cliente de Ethereum como Geth o Truffle al punto de conexión del proxy de cliente. Por último, cuando se envía una transacción, el proxy de cliente inserta el token de portador en el encabezado http y el proxy inverso valida el token mediante el protocolo OAuth.

## <a name="keys-and-ethereum-accounts"></a>Las claves y las cuentas de Ethereum

Al aprovisionar un miembro de servicio de Azure Blockchain, Ethereum de una cuenta de servicio y un par de claves público y privado se genera. La clave privada se utiliza para enviar las transacciones a la cadena de bloques. La cuenta de Ethereum es los últimos 20 bytes del hash de la clave pública. La cuenta de Ethereum también se denomina una cartera.

El par de claves público y privado se almacena como un archivo de clave en formato JSON. La clave privada se cifra con la contraseña que escribió cuando se crea el servicio de libro de contabilidad de la cadena de bloques.

Las claves privadas se utilizan para firmar digitalmente las transacciones. En las cadenas de bloques privada, un contrato inteligente firmado por una clave privada representa la identidad del firmante. Para comprobar la validez de la firma, el receptor puede comparar la clave pública del firmante con la dirección que se calcula a partir de la firma.

Las claves de constellation se usan para identificar de forma única un nodo de quórum. Claves de constellation se generan en el momento de aprovisionamiento del nodo y se especifican en el parámetro privateFor de una transacción privada en el quórum.

## <a name="next-steps"></a>Pasos siguientes

[Configurar los nodos de transacción de servicio de Azure Blockchain](configure-transaction-nodes.md)
