---
title: 'Azure Front Door Service: referencia de PowerShell | Microsoft Docs'
description: En esta referencia se detallan los distintos cmdlets de PowerShell compatibles con Azure Front Door Service
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2018
ms.author: sharadag
Module Name: AzureRM.FrontDoor
online version: https://docs.microsoft.com/powershell/module/azurerm.frontdoor/new-azurermfrontdoorbackendobject
schema: 2.0.0
ms.openlocfilehash: 6ec904f581258d8254e7cb69a0ed7ee858a41154
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47046849"
---
# New-AzureRmFrontDoorBackendObject
Crear un objeto PSBackend

## SINTAXIS

```
New-AzureRmFrontDoorBackendObject -Address <String> [-HttpPort <Int32>] [-HttpsPort <Int32>]
 [-Priority <Int32>] [-Weight <Int32>] [-EnabledState <PSEnabledState>] [-BackendHostHeader <String>]
 [-DefaultProfile <IAzureContextContainer>] [<CommonParameters>]
```

## Description
Crear un objeto PSBackendPool para la creación de Front Door

## EJEMPLOS

### Ejemplo 1
```powershell
PS C:\>New-AzureRmFrontDoorBackendObject -Address "contoso1.azurewebsites.net"


Address           : contoso1.azurewebsites.net
HttpPort          : 80
HttpsPort         : 443
Priority          : 1
Weight            : 50
BackendHostHeader :
EnabledState      : Enabled
```

Crear un objeto PSBackendPool para la creación de Front Door

## PARÁMETROS

### -Address
Ubicación del servidor back-end (dirección IP o FQDN)

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -BackendHostHeader
Valor que se usará como el encabezado de host que se envía al servidor back-end. El valor predeterminado es la dirección de back-end.

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -DefaultProfile
Credenciales, cuenta, inquilino y suscripción usados para la comunicación con Azure.

```yaml
Type: Microsoft.Azure.Commands.Common.Authentication.Abstractions.IAzureContextContainer
Parameter Sets: (All)
Aliases: AzureRmContext, AzureCredential

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -EnabledState
Si desea habilitar el uso de este servidor back-end. El valor predeterminado es Enabled.

```yaml
Type: Microsoft.Azure.Commands.FrontDoor.Models.PSEnabledState
Parameter Sets: (All)
Aliases:
Accepted values: Enabled, Disabled

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -HttpPort
Número de puerto TCP de HTTP.
Debe estar entre 1 y 65535.
El valor predeterminado es 80.

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -HttpsPort
Número de puerto TCP de HTTPS.
Debe estar entre 1 y 65535.
El valor predeterminado es 443.

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Priority
Prioridad que se usará para el equilibrio de carga.
Debe estar entre 1 y 5.
El valor predeterminado es 1.

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Weight
Peso de este punto de conexión con fines de equilibrio de carga.
Debe estar entre 1 y 1000.
El valor predeterminado es 50.

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### CommonParameters
Este cmdlet admite los parámetros comunes siguientes: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction y -WarningVariable. Para más información, consulte about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
