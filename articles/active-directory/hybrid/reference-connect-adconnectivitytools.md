---
title: 'Azure AD Connect: Referencia de PowerShell ADConnectivityTools | Microsoft Docs'
description: En este documento se proporciona información de referencia para el módulo de PowerShell ADConnectivityTools.psm1.
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.date: 10/19/2018
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: 9738c745064607493fb2660c033dd3f2499fee58
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55158696"
---
# <a name="azure-ad-connect--adconnectivitytools-powershell-reference"></a>Azure AD Connect:  Referencia de PowerShell ADConnectivityTools
En la siguiente documentación se proporciona información de referencia para el módulo de PowerShell ADConnectivityTools.psm1 que se incluye con Azure AD Connect.

## <a name="confirm-dnsconnectivity"></a>Confirm-DnsConnectivity

### <a name="synopsis"></a>SINOPSIS
Detecta problemas de DNS locales.

### <a name="syntax"></a>SINTAXIS

```
Confirm-DnsConnectivity [-Forest] <String> [-DCs] <Array> [-ReturnResultAsPSObject] [<CommonParameters>]
```

### <a name="description"></a>Description
Ejecuta pruebas de conectividad de DNS locales.
Para configurar el conector de Active Directory, el usuario debe tener tanto la resolución de nombres del bosque al que intenta conectarse, así como en los controladores de dominio asociados a este bosque.

### <a name="examples"></a>EJEMPLOS

#### <a name="example-1"></a>EJEMPLO 1
```
Confirm-DnsConnectivity -Forest "TEST.CONTOSO.COM" -DCs "MYDC1.CONTOSO.COM","MYDC2.CONTOSO.COM"
```

#### <a name="example-2"></a>EJEMPLO 2
```
Confirm-DnsConnectivity -Forest "TEST.CONTOSO.COM"
```

### <a name="parameters"></a>PARÁMETROS

#### <a name="-forest"></a>-Forest
Especifica el nombre del bosque en el que se realizará la prueba.

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-dcs"></a>-DCs
Especifica los controladores de dominio en los que se realizará la prueba.

```yaml
Type: Array
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-returnresultaspsobject"></a>-ReturnResultAsPSObject
Devuelve el resultado de este diagnóstico en forma de objeto PSObject.
No es necesario durante la interacción manual con esta herramienta.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet admite los parámetros comunes siguientes: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction y -WarningVariable.
Para más información, consulte about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="confirm-forestexists"></a>Confirm-ForestExists

### <a name="synopsis"></a>SINOPSIS
Determina si existe un bosque especificado.

### <a name="syntax"></a>SINTAXIS

```
Confirm-ForestExists [-Forest] <String> [<CommonParameters>]
```

### <a name="description"></a>Description
Consulta un servidor DNS para obtener las direcciones IP asociadas con un bosque.

### <a name="examples"></a>EJEMPLOS

#### <a name="example-1"></a>EJEMPLO 1
```
Confirm-TargetsAreReachable -Forest "TEST.CONTOSO.COM"
```

### <a name="parameters"></a>PARÁMETROS

#### <a name="-forest"></a>-Forest
Especifica el nombre del bosque en el que se realizará la prueba.

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet admite los parámetros comunes siguientes: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction y -WarningVariable.
Para más información, consulte about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="confirm-functionallevel"></a>Confirm-FunctionalLevel

### <a name="synopsis"></a>SINOPSIS
Comprueba el nivel funcional del bosque de AD.

### <a name="syntax"></a>SINTAXIS

#### <a name="samaccount"></a>SamAccount
```
Confirm-FunctionalLevel -Forest <String> [-RunWithCurrentlyLoggedInUserCredentials] [<CommonParameters>]
```

#### <a name="forestfqdn"></a>ForestFQDN
```
Confirm-FunctionalLevel -ForestFQDN <Forest> [-RunWithCurrentlyLoggedInUserCredentials] [<CommonParameters>]
```

### <a name="description"></a>Description
Comprueba que el nivel funcional del bosque de AD es igual o mayor que una versión MinAdForestVersion determinada (WindowsServer2003).
Se pueden solicitar la cuenta (dominio\nombre de usuario) y la contraseña.

### <a name="examples"></a>EJEMPLOS

#### <a name="example-1"></a>EJEMPLO 1
```
Confirm-FunctionalLevel -Forest "test.contoso.com"
```

#### <a name="example-2"></a>EJEMPLO 2
```
Confirm-FunctionalLevel -Forest "test.contoso.com" -RunWithCurrentlyLoggedInUserCredentials -Verbose
```

#### <a name="example-3"></a>EJEMPLO 3
```
Confirm-FunctionalLevel -ForestFQDN $ForestFQDN -RunWithCurrentlyLoggedInUserCredentials -Verbose
```

### <a name="parameters"></a>PARÁMETROS

#### <a name="-forest"></a>-Forest
Bosque de destino.
El valor predeterminado es el bosque del usuario que tiene una sesión iniciada actualmente.

```yaml
Type: String
Parameter Sets: SamAccount
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-forestfqdn"></a>-ForestFQDN
Objeto ForestFQDN de destino.

```yaml
Type: Forest
Parameter Sets: ForestFQDN
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-runwithcurrentlyloggedinusercredentials"></a>-RunWithCurrentlyLoggedInUserCredentials
La función usará las credenciales del usuario que tiene una sesión iniciada en el equipo, en lugar de solicitar las credenciales personalizadas del usuario.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet admite los parámetros comunes siguientes: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction y -WarningVariable.
Para más información, consulte about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="confirm-networkconnectivity"></a>Confirm-NetworkConnectivity

### <a name="synopsis"></a>SINOPSIS
Detecta problemas de conectividad de la red local.

### <a name="syntax"></a>SINTAXIS

```
Confirm-NetworkConnectivity [-DCs] <Array> [-SkipDnsPort] [-ReturnResultAsPSObject] [<CommonParameters>]
```

### <a name="description"></a>Description
Ejecuta pruebas de conectividad de la red local.

Para las pruebas de redes locales, AAD Connect debe ser capaz de comunicarse con los controladores de dominio con nombre en los puertos 53 (DNS), 88 (Kerberos) y 389 (LDAP). La mayoría de las organizaciones ejecutan DNS en sus controladores de dominio, motivo por el que esta prueba está integrada actualmente.
Si se ha especificado otro servidor DNS, se debe omitir el puerto 53.

### <a name="examples"></a>EJEMPLOS

#### <a name="example-1"></a>EJEMPLO 1
```
Confirm-NetworkConnectivity -SkipDnsPort -DCs "MYDC1.CONTOSO.COM","MYDC2.CONTOSO.COM"
```

#### <a name="example-2"></a>EJEMPLO 2
```
Confirm-NetworkConnectivity -DCs "MYDC1.CONTOSO.COM","MYDC2.CONTOSO.COM" -Verbose
```

### <a name="parameters"></a>PARÁMETROS

#### <a name="-dcs"></a>-DCs
Especifica los controladores de dominio en los que se realizará la prueba.

```yaml
Type: Array
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipdnsport"></a>-SkipDnsPort
Si el usuario no usa los servicios DNS proporcionados por el controlador de dominio de inicio de sesión o el sitio de AD, es posible que prefiera omitir la comprobación del puerto 53. El usuario debe poder resolver _.ldap._tcp.\<forestfqdn\> para que la configuración del conector de Active Directory se realice correctamente.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-returnresultaspsobject"></a>-ReturnResultAsPSObject
Devuelve el resultado de este diagnóstico en forma de objeto PSObject.
No es necesario durante la interacción manual con esta herramienta.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet admite los parámetros comunes siguientes: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction y -WarningVariable.
Para más información, consulte about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="confirm-targetsarereachable"></a>Confirm-TargetsAreReachable

### <a name="synopsis"></a>SINOPSIS
Determina si un bosque especificado y sus controladores de dominio asociados son accesibles.

### <a name="syntax"></a>SINTAXIS

```
Confirm-TargetsAreReachable [-Forest] <String> [-DCs] <Array> [<CommonParameters>]
```

### <a name="description"></a>Description
Ejecuta pruebas de "ping" (si un equipo puede comunicarse con un equipo de destino a través de la red o de Internet)

### <a name="examples"></a>EJEMPLOS

#### <a name="example-1"></a>EJEMPLO 1
```
Confirm-TargetsAreReachable -Forest "TEST.CONTOSO.COM" -DCs "MYDC1.CONTOSO.COM","MYDC2.CONTOSO.COM"
```

#### <a name="example-2"></a>EJEMPLO 2
```
Confirm-TargetsAreReachable -Forest "TEST.CONTOSO.COM"
```

### <a name="parameters"></a>PARÁMETROS

#### <a name="-forest"></a>-Forest
Especifica el nombre del bosque en el que se realizará la prueba.

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-dcs"></a>-DCs
Especifica los controladores de dominio en los que se realizará la prueba.

```yaml
Type: Array
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet admite los parámetros comunes siguientes: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction y -WarningVariable.
Para más información, consulte about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="confirm-validdomains"></a>Confirm-ValidDomains

### <a name="synopsis"></a>SINOPSIS
Validar que los dominios del FQDN del bosque obtenido sean accesibles

### <a name="syntax"></a>SINTAXIS

#### <a name="samaccount"></a>SamAccount
```
Confirm-ValidDomains [-Forest <String>] [-RunWithCurrentlyLoggedInUserCredentials] [<CommonParameters>]
```

#### <a name="forestfqdn"></a>ForestFQDN
```
Confirm-ValidDomains -ForestFQDN <Forest> [-RunWithCurrentlyLoggedInUserCredentials] [<CommonParameters>]
```

### <a name="description"></a>Description
Validar que todos los dominios del FQDN del bosque obtenido sean accesibles mediante un intento de recuperación de DomainGuid y DomainDN.
Se pueden solicitar la cuenta (dominio\nombre de usuario) y la contraseña.

### <a name="examples"></a>EJEMPLOS

#### <a name="example-1"></a>EJEMPLO 1
```
Confirm-ValidDomains -Forest "test.contoso.com" -Verbose
```

#### <a name="example-2"></a>EJEMPLO 2
```
Confirm-ValidDomains -Forest "test.contoso.com" -RunWithCurrentlyLoggedInUserCredentials -Verbose
```

#### <a name="example-3"></a>EJEMPLO 3
```
Confirm-ValidDomains -ForestFQDN $ForestFQDN -RunWithCurrentlyLoggedInUserCredentials -Verbose
```

### <a name="parameters"></a>PARÁMETROS

#### <a name="-forest"></a>-Forest
Bosque de destino.

```yaml
Type: String
Parameter Sets: SamAccount
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-forestfqdn"></a>-ForestFQDN
Objeto ForestFQDN de destino.

```yaml
Type: Forest
Parameter Sets: ForestFQDN
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-runwithcurrentlyloggedinusercredentials"></a>-RunWithCurrentlyLoggedInUserCredentials
La función usará las credenciales del usuario que tiene una sesión iniciada en el equipo, en lugar de solicitar las credenciales personalizadas del usuario.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet admite los parámetros comunes siguientes: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction y -WarningVariable.
Para más información, consulte about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="confirm-validenterpriseadmincredentials"></a>Confirm-ValidEnterpriseAdminCredentials

### <a name="synopsis"></a>SINOPSIS
Validar si un usuario tiene credenciales de administrador de empresa.

### <a name="syntax"></a>SINTAXIS

```
Confirm-ValidEnterpriseAdminCredentials [-RunWithCurrentlyLoggedInUserCredentials] [<CommonParameters>]
```

### <a name="description"></a>Description
Busca si el usuario proporcionado tiene credenciales de administrador de empresa.
Se pueden solicitar la cuenta (dominio\nombre de usuario) y la contraseña.

### <a name="examples"></a>EJEMPLOS

#### <a name="example-1"></a>EJEMPLO 1
```
Confirm-ValidEnterpriseAdminCredentials -DomainName test.contoso.com -Verbose
```

#### <a name="example-2"></a>EJEMPLO 2
```
Confirm-ValidEnterpriseAdminCredentials -RunWithCurrentlyLoggedInUserCredentials -Verbose
```

### <a name="parameters"></a>PARÁMETROS

#### <a name="-runwithcurrentlyloggedinusercredentials"></a>-RunWithCurrentlyLoggedInUserCredentials
La función usará las credenciales del usuario que tiene una sesión iniciada en el equipo, en lugar de solicitar las credenciales personalizadas del usuario.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet admite los parámetros comunes siguientes: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction y -WarningVariable.
Para más información, consulte about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="get-domainfqdndata"></a>Get-DomainFQDNData

### <a name="synopsis"></a>SINOPSIS
Recupera un objeto DomainFQDN de una combinación de cuenta y contraseña.

### <a name="syntax"></a>SINTAXIS

```
Get-DomainFQDNData [[-DomainFQDNDataType] <String>] [-RunWithCurrentlyLoggedInUserCredentials]
 [-ReturnExceptionOnError] [<CommonParameters>]
```

### <a name="description"></a>Description
Intenta obtener un objeto domainFQDN de las credenciales proporcionadas.
Si el objeto domainFQDN es válido, se devolverá DomainFQDNName RootDomainName, según la elección del usuario.
Se pueden solicitar la cuenta (dominio\nombre de usuario) y la contraseña.

### <a name="examples"></a>EJEMPLOS

#### <a name="example-1"></a>EJEMPLO 1
```
Get-DomainFQDNData -DomainFQDNDataType DomainFQDNName -Verbose
```

#### <a name="example-2"></a>EJEMPLO 2
```
Get-DomainFQDNData -DomainFQDNDataType RootDomainName -RunWithCurrentlyLoggedInUserCredentials
```

### <a name="parameters"></a>PARÁMETROS

#### <a name="-domainfqdndatatype"></a>-DomainFQDNDataType
Tipo de datos deseado que se recuperará.
Se limita actualmente a "DomainFQDNName" o "RootDomainName".

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-runwithcurrentlyloggedinusercredentials"></a>-RunWithCurrentlyLoggedInUserCredentials
La función usará las credenciales del usuario que tiene una sesión iniciada en el equipo, en lugar de solicitar las credenciales personalizadas del usuario.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-returnexceptiononerror"></a>-ReturnExceptionOnError
Parámetro auxiliar usado por la función Start-NetworkConnectivityDiagnosisTools

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet admite los parámetros comunes siguientes: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction y -WarningVariable.
Para más información, consulte about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="get-forestfqdn"></a>Get-ForestFQDN

### <a name="synopsis"></a>SINOPSIS
Recupera un objeto ForestFQDN de una combinación de cuenta y contraseña.

### <a name="syntax"></a>SINTAXIS

```
Get-ForestFQDN [-Forest] <String> [-RunWithCurrentlyLoggedInUserCredentials] [<CommonParameters>]
```

### <a name="description"></a>Description
Intenta obtener un objeto ForestFQDN de las credenciales proporcionadas.
Se pueden solicitar la cuenta (dominio\nombre de usuario) y la contraseña.

### <a name="examples"></a>EJEMPLOS

#### <a name="example-1"></a>EJEMPLO 1
```
Get-ForestFQDN -Forest CONTOSO.MICROSOFT.COM -Verbose
```

#### <a name="example-2"></a>EJEMPLO 2
```
Get-ForestFQDN -Forest CONTOSO.MICROSOFT.COM -RunWithCurrentlyLoggedInUserCredentials -Verbose
```

### <a name="parameters"></a>PARÁMETROS

#### <a name="-forest"></a>-Forest
Bosque de destino. El valor predeterminado es el dominio del usuario que tiene una sesión iniciada actualmente.

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-runwithcurrentlyloggedinusercredentials"></a>-RunWithCurrentlyLoggedInUserCredentials
La función usará las credenciales del usuario que tiene una sesión iniciada en el equipo, en lugar de solicitar las credenciales personalizadas del usuario.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet admite los parámetros comunes siguientes: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction y -WarningVariable.
Para más información, consulte about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="start-connectivityvalidation"></a>Start-ConnectivityValidation

### <a name="synopsis"></a>SINOPSIS
Función principal.

### <a name="syntax"></a>SINTAXIS

```
Start-ConnectivityValidation [-Forest] <String> [-AutoCreateConnectorAccount] <Boolean> [[-UserName] <String>]
 [<CommonParameters>]
```

### <a name="description"></a>Description
Ejecuta todos los mecanismos disponibles que comprueban que las credenciales de AD sean válidas.

### <a name="examples"></a>EJEMPLOS

#### <a name="example-1"></a>EJEMPLO 1
```
Start-ConnectivityValidation -Forest "test.contoso.com" -AutoCreateConnectorAccount $True -Verbose
```

### <a name="parameters"></a>PARÁMETROS

#### <a name="-forest"></a>-Forest
Bosque de destino.

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-autocreateconnectoraccount"></a>-AutoCreateConnectorAccount
Para instalaciones personalizadas: la marca es $True si el usuario selecciona "Crear una cuenta de AD" en la ventana Cuenta del bosque de AD del Asistente de AADConnect.
$False si el usuario elige "Usar una cuenta de AD existente".
Para instalaciones rápidas: el valor de esta variable debe ser $True para las instalaciones rápidas.

```yaml
Type: Boolean
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-username"></a>-UserName
Parámetro que rellena previamente el campo Nombre de usuario cuando se solicitan las credenciales del usuario.

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 3
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet admite los parámetros comunes siguientes: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction y -WarningVariable.
Para más información, consulte about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="start-networkconnectivitydiagnosistools"></a>Start-NetworkConnectivityDiagnosisTools

### <a name="synopsis"></a>SINOPSIS
Función principal para las pruebas de conectividad de red.

### <a name="syntax"></a>SINTAXIS

```
Start-NetworkConnectivityDiagnosisTools [[-Forest] <String>] [-Credentials] <PSCredential>
 [[-LogFileLocation] <String>] [[-DCs] <Array>] [-DisplayInformativeMessage] [-ReturnResultAsPSObject]
 [-ValidCredentials] [<CommonParameters>]
```

### <a name="description"></a>Description
Ejecuta pruebas de conectividad de la red local.

### <a name="examples"></a>EJEMPLOS

#### <a name="example-1"></a>EJEMPLO 1
```
Start-NetworkConnectivityDiagnosisTools -Forest "TEST.CONTOSO.COM"
```

#### <a name="example-2"></a>EJEMPLO 2
```
Start-NetworkConnectivityDiagnosisTools -Forest "TEST.CONTOSO.COM" -DCs "DC1.TEST.CONTOSO.COM", "DC2.TEST.CONTOSO.COM"
```

### <a name="parameters"></a>PARÁMETROS

#### <a name="-forest"></a>-Forest
Especifica el nombre del bosque en el que se realizará la prueba.

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-credentials"></a>-Credentials
Nombre de usuario y contraseña del usuario que está ejecutando la prueba.
Requiere el mismo nivel de permisos que se requiere para ejecutar al Asistente de Azure AD Connect.

```yaml
Type: PSCredential
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-logfilelocation"></a>-LogFileLocation
Especifica la ubicación de un archivo de registro que contendrá la salida de esta función.

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 3
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-dcs"></a>-DCs
Especifica los controladores de dominio en los que se realizará la prueba.

```yaml
Type: Array
Parameter Sets: (All)
Aliases:

Required: False
Position: 4
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-displayinformativemessage"></a>-DisplayInformativeMessage
Marca que permite mostrar un mensaje sobre el propósito de esta función.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-returnresultaspsobject"></a>-ReturnResultAsPSObject
Devuelve el resultado de este diagnóstico en forma de objeto PSObject.
No es necesario que se especifique durante la interacción manual con esta herramienta.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-validcredentials"></a>-ValidCredentials
Indica si las credenciales que ha escrito el usuario son válidas.
No es necesario que se especifique durante la interacción manual con esta herramienta.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Este cmdlet admite los parámetros comunes siguientes: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction y -WarningVariable.
Para más información, consulte about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).
