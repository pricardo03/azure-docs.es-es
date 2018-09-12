---
title: Parámetros comunes del flujo de trabajo en la validación de Azure Stack como servicio | Microsoft Docs
description: Parámetros comunes del flujo de trabajo en la validación de Azure Stack como servicio
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: c50e4b5c9eb81c9386e2cb0db96a88de70dcb9e9
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44157810"
---
# <a name="workflow-common-parameters-for-azure-stack-validation-as-a-service"></a>Parámetros comunes del flujo de trabajo en la validación de Azure Stack como servicio

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Los parámetros comunes incluyen valores como variables de entorno y las credenciales de usuario necesarias en todas las pruebas de validación como servicio (VaaS). Usted define estos valores a nivel del flujo de trabajo. Guarda los valores al crear o modificar un flujo de trabajo. En tiempo de programación, el flujo de trabajo carga los valores de la prueba. 

## <a name="environment-parameters"></a>Parámetros del entorno

Los parámetros del entorno describen el entorno de Azure Stack sometido a prueba. Estos valores deben proporcionarse al generar y cargar el archivo de configuración de marca `&lt;link&gt;. [How to get the stamp info link].`

| Nombre de parámetro | Obligatorio | Escriba | DESCRIPCIÓN |
|----------------------------------|----------|------|---------------------------------------------------------------------------------------------------------------------------------|
| Compilación de Azure Stack | Obligatorio |  | Número de compilación de la implementación de Azure Stack (por ejemplo, 1.0.170330.9) |
| Versión de OEM | SÍ |  | Número de versión del paquete de OEM que se usa durante la implementación de Azure Stack. |
| Firma de OEM | SÍ |  | Firma del paquete de OEM que se usa durante la implementación de Azure Stack. |
| Id. de inquilino de AAD | Obligatorio |  | GUID del inquilino de Azure Active Directory especificado durante la implementación de Azure Stack.|
| Region | Obligatorio |  | Región de implementación de Azure Stack. |
| Punto de conexión de Resource Manager del inquilino | Obligatorio |  | Punto de conexión para las operaciones de Azure Resource Manager del inquilino (por ejemplo, https://management.<ExternalFqdn>) |
| Punto de conexión de Resource Manager de administrador | SÍ |  | Punto de conexión para las operaciones de Azure Resource Manager del inquilino (por ejemplo, https://adminmanagement.<ExternalFqdn>) |
| FQDN externo | SÍ |  | Nombre de dominio completo externo utilizado como sufijo para los puntos de conexión. (Por ejemplo, local.azurestack.external o redmond.contoso.com). |
| Número de nodos | SÍ |  | Número de nodos en la implementación. |

## <a name="test-parameters"></a>Parámetros de prueba

Los parámetros de prueba comunes incluyen información confidencial que no se puede almacena en archivos de configuración y que deben proporcionarse manualmente.

| Nombre de parámetro | Obligatorio | Escriba | DESCRIPCIÓN |
|--------------------------------|------------------------------------------------------------------------------|------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nombre de usuario del inquilino | Obligatorio |  | Administrador de inquilinos de Azure Active Directory que ya se ha aprovisionado o que el administrador del servicio debe aprovisionar en el directorio de AAD. Para obtener más información sobre el aprovisionamiento de la cuenta de inquilino, consulte [Introducción a Azure AD](https://docs.microsoft.com/azure/active-directory/get-started-azure-ad). La prueba utiliza este valor para realizar las operaciones a nivel del inquilino, como la implementación de plantillas para aprovisionar los recursos (VM, cuentas de almacenamiento etc.) y ejecutar cargas de trabajo. La prueba utiliza este valor para realizar las operaciones a nivel del inquilino, como la implementación de plantillas para aprovisionar los recursos (VM, cuentas de almacenamiento etc.) y ejecutar cargas de trabajo. |
| Contraseña del inquilino | Obligatorio |  | Contraseña del usuario inquilino. |
| Nombre de usuario del administrador de servicios | Obligatorio: Validación de soluciones, validación de paquetes<br>No obligatorio: superación de la prueba |  | Administrador de Azure Active Directory del inquilino del directorio de AAD especificado durante la implementación de Azure Stack. |
| Contraseña del administrador de servicios | Obligatorio: Validación de soluciones, validación de paquetes<br>No obligatorio: superación de la prueba |  | Contraseña del usuario administrador de servicios. |
| Nombre de usuario del administrador de la nube | Obligatorio |  | Cuenta de administrador del dominio de Azure Stack (por ejemplo, contoso\cloudadmin). Busque User Role="CloudAdmin" en el archivo de configuración y seleccione el valor en la etiqueta UserName en el archivo de configuración. |
| Contraseña del administrador de la nube | Obligatorio |  | Contraseña del usuario administrador de la nube. |
| Cadena de conexión de diagnósticos | Obligatorio |  | Un URI de SAS a una cuenta de Azure Storage donde se copiarán los registro de diagnóstico durante la ejecución de pruebas. Las instrucciones para generar el URI de SAS se encuentran en [Set up a blob storage account](azure-stack-vaas-set-up-account.md) (Configuración de una cuenta de almacenamiento de blobs). |


## <a name="next-steps"></a>Pasos siguientes

- Para más información, consulte [Validación como servicio de Azure Stack](https://docs.microsoft.com/azure/azure-stack/partner).
