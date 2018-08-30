---
title: Parámetros de la prueba para la validación como servicio de Azure Stack | Microsoft Docs
description: Tema de referencia sobre el archivo de configuración y los parámetros de prueba superada de la validación como servicio de Azure Stack.
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
ms.openlocfilehash: 65cae4d10a4683207474008a18bac39751ce8e96
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2018
ms.locfileid: "40234592"
---
# <a name="test-parameters-for-validation-as-a-service-azure-stack"></a>Parámetros de prueba para la validación como servicio de Azure Stack

[!INCLUDE[Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Antes de ejecutar cualquier conjunto de pruebas de validaciones como servicio (VaaS), seleccione una solución y cree una prueba superada.

- Inicie sesión con sus credenciales de inquilino registradas de VaaS.
- Puede crear una solución mediante **Add Solution** (Agregar solución) o seleccionar una ya existente.
- Seleccione el botón **Start** (Iniciar) en el icono de flujo de trabajo **Test Passes** (Pruebas superadas).

> [!Note]  
> Cree una entrada de solución para cada conjunto único de máquinas virtuales o configuración de hardware que vaya a validar, y una prueba superada para cada implementación de compilación en ese conjunto de máquinas.

Deberá especificar los parámetros necesarios para ejecutar las pruebas en la página **Test Pass Information** (Información de prueba superada). Proporcione estos parámetros al iniciar una nueva prueba superada o una ejecución de validación. La mayoría de los parámetros tiene los mismos valores que proporcionó al implementar Azure Stack.

Puede especificar manualmente los valores enumerados en la [tabla de parámetros de prueba](#test-parameters), o cargar el archivo de configuración de implementación, que contiene toda la información de marca de Azure Stack. Una vez cargado, el portal carga los valores del archivo.

> [!Note]  
> Puede buscar y escribir los valores de parámetros de prueba buscando y cargando el archivo de configuración en el portal.

## <a name="export-the-test-parameters-using-powershell"></a>Exportación de los parámetros de prueba con PowerShell

1. Inicie sesión en la máquina de DVM o en cualquier máquina que tenga acceso al entorno de Azure Stack.
2. Abra una ventana de PowerShell con privilegios elevados y ejecute:

    ````PowerShell  
        $params = Invoke-RestMethod -Method Get -Uri 'https://ASAppGateway:4443/ServiceTypeId/4dde37cc-6ee0-4d75-9444-7061e156507f/CloudDefinition/GetStampInformation'
        ConvertTo-Json $params > stampinfoproperties.json
    ````

3. Cargue **stampinfoproperties.json** en el portal de VaaS.

## <a name="find-the-test-parameters-in-the-configuration-file"></a>Búsqueda de los parámetros de prueba en el archivo de configuración

También puede encontrar los valores de parámetros de prueba si abre el **archivo de configuración** de ECE. Puede encontrar el archivo en la siguiente ruta de acceso en la máquina de DVM:  
`C:\EceStore\403314e1-d945-9558-fad2-42ba21985248\80e0921f-56b5-17d3-29f5-cd41bf862787`

## <a name="test-parameters"></a>Parámetros de prueba 

| Parámetro    | DESCRIPCIÓN |
|-------------|-----------------|
| Compilación de Azure Stack                      | Compilación o número de versión de Azure Stack que se implementó, por ejemplo, 1.0.170330.0. | 
| Id. de inquilino                              | Inquilino de Azure Active Directory especificado durante la implementación de Azure Stack. Busque **AADTenant** en el archivo de configuración y seleccione el valor de **GUID** en la etiqueta `Id`. | 
| Region                                 | Región de implementación de Azure Stack. Busque **Region** en el archivo de configuración. | 
| Resource Manager de inquilino                | Punto de conexión de Azure Resource Manager del inquilino, por ejemplo, `https://management.<ExternalFqdn>`. | 
| Resource Manager de administrador                 | Punto de conexión de Azure Resource Manager del administrador. por ejemplo, `https://adminmanagement.<ExternalFqdn>` | 
| FQDN externo                          | FQDN externo del entorno. Busque **ExternalFqdn** en el archivo de configuración. | 
| Usuario inquilino                            | Administrador de inquilinos de Azure Active Directory que ya se ha aprovisionado o que el administrador del servicio debe aprovisionar en el directorio de Azure AAD. Para más información sobre el aprovisionamiento de cuentas de inquilino, consulte [Adición de una nueva cuenta de inquilino de Azure Stack en Azure Active Directory](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-new-user-aad). Este valor se usa en la prueba para realizar operaciones de nivel de inquilino, como la implementación de plantillas para aprovisionar recursos (máquinas virtuales, cuentas de almacenamiento, etc.) y ejecutar cargas de trabajo. | 
| Usuario administrador de servicios             | Administrador de Azure Active Directory del inquilino de directorio de Azure AD especificado durante la implementación de Azure Stack. Busque **AADTenant** en el archivo de configuración y seleccione el valor en la etiqueta `UniqueName`. | 

## <a name="checks-before-starting-the-tests"></a>Comprobaciones antes de iniciar las pruebas

Las pruebas realizan operaciones remotas. La máquina que ejecuta las pruebas debe tener acceso a los puntos de conexión de Azure Stack. En caso contrario, la prueba no funcionará. Si usa el agente local de VaaS, utilice la máquina donde se va a ejecutar el agente. Puede comprobar que la máquina tiene acceso a los puntos de Azure Stack mediante la ejecución de las siguientes comprobaciones.

1. Compruebe que es posible acceder al URI base. Abra un símbolo del sistema CMD o un shell de bash y ejecute el siguiente comando:

    ```bash  
    nslookup adminmanagement.<EXTERNALFQDN>
    ```

2. Abra un explorador web y vaya a `https://adminportal.<EXTERNALFQDN>` para comprobar que se puede acceder al portal de MAS.

3. Inicie sesión con los valores de nombre y contraseña del administrador de servicios de Azure AD proporcionados al crear la prueba superada.

## <a name="next-steps"></a>Pasos siguientes

- Para más información, consulte [Validación como servicio de Azure Stack](https://docs.microsoft.com/azure/azure-stack/partner).
