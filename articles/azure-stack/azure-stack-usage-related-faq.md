---
title: Preguntas más frecuentes relacionadas con el uso de API | Microsoft Docs
description: Lista de medidores de Azure Stack, en comparación con los códigos de error, el tiempo notificado y el tiempo de uso y la API de uso de Azure.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2018
ms.author: mabrigg
ms.reviewer: alfredop
ms.openlocfilehash: 95b63b525365a5a31856c2f14ac965aaa1113cff
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2018
ms.locfileid: "51515631"
---
# <a name="frequently-asked-questions-in-azure-stack-usage-api"></a>Preguntas frecuentes sobre la API de uso de Azure Stack

En este artículo se responde a algunas preguntas frecuentes sobre la API de uso de Azure Stack.

## <a name="what-meter-ids-can-i-see"></a>¿Qué identificadores de medidor veo?
Se informa del uso para los siguientes proveedores de recursos:

### <a name="network"></a>Red
  
**Id. del medidor**: F271A8A388C44D93956A063E1D2FA80B  
**Nombre del medidor**: Uso de la dirección IP estática  
**Unidad**: Direcciones IP  
**Notas**: Número de direcciones IP usadas. Si llama a la API de uso con una granularidad diaria, el medidor devuelve la dirección IP multiplicada por el número de horas.  
  
**Id. del medidor**: 9E2739BA86744796B465F64674B822BA  
**Nombre del medidor**: Uso de la dirección IP dinámica  
**Unidad**: Direcciones IP  
**Notas**: Número de direcciones IP usadas. Si llama a la API de uso con una granularidad diaria, el medidor devuelve la dirección IP multiplicada por el número de horas.  
  
### <a name="storage"></a>Storage
  
**Id. del medidor**: B4438D5D-453B-4EE1-B42A-DC72E377F1E4  
**Nombre del medidor**: TableCapacity  
**Unidad**: GB\*horas  
**Notas**: Capacidad total consumida por las tablas.  
  
**Id. del medidor**: B5C15376-6C94-4FDD-B655-1A69D138ACA3  
**Nombre del medidor**: PageBlobCapacity  
**Unidad**: GB\*horas  
**Notas**: Capacidad total consumida por blobs en páginas.  
  
**Id. del medidor**: B03C6AE7-B080-4BFA-84A3-22C800F315C6  
**Nombre del medidor**: QueueCapacity  
**Unidad**: GB\*horas  
**Notas**: Capacidad total consumida por la cola.  
  
**Id. del medidor**: 09F8879E-87E9-4305-A572-4B7BE209F857  
**Nombre del medidor**: BlockBlobCapacity  
**Unidad**: GB\*horas  
**Notas**: Capacidad total consumida por blobs en bloques.  
  
**Id. del medidor**: B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90  
**Nombre del medidor**: TableTransactions  
**Unidad**: Recuento de solicitudes en decenas de millar  
**Notas**: Solicitudes de Table service (en decenas de millar).  
  
**Id. del medidor**: 50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D  
**Nombre del medidor**: TableDataTransIn  
**Unidad**: Datos de entrada en GB  
**Notas**: Entrada de datos de Table service en GB.  
  
**Id. del medidor**: 1B8C1DEC-EE42-414B-AA36-6229CF199370  
**Nombre del medidor**: TableDataTransOut  
**Unidad**: Salida en GB  
**Notas**: Salida de datos de Table service en GB  
  
**Id. del medidor**: 43DAF82B-4618-444A-B994-40C23F7CD438  
**Nombre del medidor**: BlobTransactions  
**Unidad**: Recuento de solicitudes en decenas de millar  
**Notas**: solicitudes de Blob service (en decenas de millar).  
  
**Id. del medidor**: 9764F92C-E44A-498E-8DC1-AAD66587A810  
**Nombre del medidor**: BlobDataTransIn  
**Unidad**: Datos de entrada en GB  
**Notas**: Entrada de datos de Blob service en GB.  
  
**Id. del medidor**: 3023FEF4-ECA5-4D7B-87B3-CFBC061931E8  
**Nombre del medidor**: BlobDataTransOut  
**Unidad**: Salida en GB  
**Notas**: Salida de datos de Blob service en GB.  
  
**Id. del medidor**: EB43DD12-1AA6-4C4B-872C-FAF15A6785EA  
**Nombre del medidor**: QueueTransactions  
**Unidad**: Recuento de solicitudes en decenas de millar  
**Notas**: Solicitudes de Queue service (en decenas de millar).  
  
**Id. del medidor**: E518E809-E369-4A45-9274-2017B29FFF25  
**Nombre del medidor**: QueueDataTransIn  
**Unidad**: Datos de entrada en GB  
**Notas**: Entrada de datos de Queue service en GB.  
  
**Id. del medidor**: DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2  
**Nombre del medidor**: QueueDataTransOut  
**Unidad**: Salida en GB  
**Notas**: Salida de datos de Queue service en GB.  

### <a name="compute"></a>Compute 
  
**Id. del medidor**: FAB6EB84-500B-4A09-A8CA-7358F8BBAEA5  
**Nombre del medidor**: Horas de tamaño de la máquina virtual base  
**Unidad**: Horas de núcleos virtuales  
**Notas**: Número de núcleos virtuales multiplicado por las horas que se ejecutó la máquina virtual.  
  
**Id. del medidor**: 9CD92D4C-BAFD-4492-B278-BEDC2DE8232A  
**Nombre del medidor**: Horas de tamaño de la máquina virtual Windows  
**Unidad**: Horas de núcleos virtuales  
**Notas**: Número de núcleos virtuales multiplicado por las horas que se ejecutó la máquina virtual.  
  
**Id. del medidor**: 6DAB500F-A4FD-49C4-956D-229BB9C8C793  
**Nombre del medidor**: Horas de tamaño de la máquina virtual  
**Unidad** : Horas de la máquina virtual  
**Notas**: Captura de la máquina virtual base y Windows. No se ajusta para los núcleos.  
  
### <a name="managed-disks"></a>Managed Disks

**Id. de medidor**: 380874f9-300c-48e0-95a0-d2d9a21ade8f   
**Nombre del medidor**: S4   
**Unidad**: recuento de discos\*mes   
**Notas**: disco administrado estándar -32 GB 

**Id. de medidor**: 1b77d90f-427b-4435-b4f1-d78adec53222   
**Nombre del medidor**: S6   
**Unidad**: recuento de discos\*mes   
**Notas**: disco administrado estándar -64 GB 

**Id. de medidor**: d5f7731b-f639-404a-89d0-e46186e22c8d   
**Nombre del medidor**: S10   
**Unidad**: recuento de discos\*mes   
**Notas**: disco administrado estándar -128 GB 

**Id. de medidor**: ff85ef31-da5b-4eac-95dd-a69d6f97b18a   
**Nombre del medidor**: S15   
**Unidad**: recuento de discos\*mes   
**Notas**: disco administrado estándar -256 GB 

**Id. de medidor**: 88ea9228-457a-4091-adc9-ad5194f30b6e   
**Nombre del medidor**: S20   
**Unidad**: recuento de discos\*mes      
**Notas**: disco administrado estándar -512 GB 

**Id. de medidor**: 5b1db88a-8596-4002-8052-347947c26940   
**Nombre del medidor**: S30   
**Unidad**: recuento de discos\*mes   
**Notas**: disco administrado estándar -1024 GB 

**Id. de medidor**: 7660b45b-b29d-49cb-b816-59f30fbab011   
**Nombre del medidor**: P4   
**Unidad**: recuento de discos\*mes   
**Notas**: disco administrado premium -32 GB 

**Id. de medidor**: 817007fd-a077-477f-bc01-b876f27205fd   
**Nombre del medidor**: P6   
**Unidad**: recuento de discos\*mes   
**Notas**: disco administrado premium -64 GB 

**Id. de medidor**: e554b6bc-96cd-4938-a5b5-0da990278519   
**Nombre del medidor**: P10   
**Unidad**: recuento de discos\*mes   
**Notas**: disco administrado premium -128 GB  

**Id. de medidor**: cdc0f53a-62a9-4472-a06c-e99a23b02907   
**Nombre del medidor**: P15  
**Unidad**: recuento de discos\*mes   
**Notas**: disco administrado premium -256 GB 

**Id. de medidor**: b9cb2d1a-84c2-4275-aa8b-70d2145d59aa   
**Nombre del medidor**: P20   
**Unidad**: recuento de discos\*mes   
**Notas**: disco administrado premium -512 GB 

**Id. de medidor**: 06bde724-9f94-43c0-84c3-d0fc54538369   
**Nombre del medidor**: P30   
**Unidad**: recuento de discos\*mes   
**Notas**: disco administrado premium -1024 GB 

**Id. de medidor**: 7ba084ec-ef9c-4d64-a179-7732c6cb5e28   
**Nombre del medidor**: ActualStandardDiskSize   
**Unidad**: GB\*mes      
**Notas**: tamaño real en disco del disco administrado estándar  

**Id. de medidor**: daef389a-06e5-4684-a7f7-8813d9f792d5  
**Nombre del medidor**: ActualPremiumDiskSize   
**Unidad**: GB\*mes      
**Notas**: tamaño real en disco del disco administrado premium 

**Id. de medidor**: 75d4b707-1027-4403-9986-6ec7c05579c8  
**Nombre del medidor**: ActualStandardSnapshotSize   
**Unidad**: GB\*mes   
**Notas**: tamaño real en disco de la instantánea estándar administrada.  

**Id. de medidor**: 5ca1cbb9-6f14-4e76-8be8-1ca91547965e   
**Nombre del medidor**: ActualPremiumSnapshotSize   
**Unidad**: GB\*mes   
**Notas**: tamaño real en disco de la instantánea premium administrada.   

**Id. de medidor**: 5d76e09f-4567-452a-94cc-7d1f097761f0   
**Nombre del medidor**: S4   
**Unidad**: recuento de discos\*horas   
**Notas**: disco administrado estándar - 32 GB (en desuso) 

**Id. de medidor**: dc9fc6a9-0782-432a-b8dc-978130457494   
**Nombre del medidor**: S6   
**Unidad**: recuento de discos\*horas   
**Notas**: disco administrado estándar - 64 GB (en desuso) 

**Id. de medidor**: e5572fce-9f58-49d7-840c-b168c0f01fff   
**Nombre del medidor**: S10   
**Unidad**: recuento de discos\*horas   
**Notas**: disco administrado estándar - 128 GB (en desuso) 

**Id. de medidor**: 9a8caedd-1195-4cd5-80b4-a4c22f9302b8   
**Nombre del medidor**: S15   
**Unidad**: recuento de discos\*horas   
**Notas**: disco administrado estándar - 256 GB (en desuso) 

**Id. de medidor**: 5938f8da-0ecd-4c48-8d5a-c7c6c23546be   
**Nombre del medidor**: S20   
**Unidad**: recuento de discos\*horas      
**Notas**: disco administrado estándar - 512 GB (en desuso) 

**Id. de medidor**: 7705a158-bd8b-4b2b-b4c2-0782343b81e6   
**Nombre del medidor**: S30   
**Unidad**: recuento de discos\*horas   
**Notas**: disco administrado estándar - 1024 GB (en desuso) 

**Id. de medidor**: 5c105f5f-cbdf-435c-b49b-3c7174856dcc   
**Nombre del medidor**: P4   
**Unidad**: recuento de discos\*horas   
**Notas**: disco administrado estándar - 32 GB (en desuso) 

**Id. de medidor**: 518b412b-1927-4f25-985f-4aea24e55c4f   
**Nombre del medidor**: P6   
**Unidad**: recuento de discos\*horas   
**Notas**: disco administrado estándar - 64 GB (en desuso) 

**Id. de medidor**: 5cfb1fed-0902-49e3-8217-9add946fd624   
**Nombre del medidor**: P10   
**Unidad**: recuento de discos\*horas   
**Notas**: disco administrado estándar - 128 GB (en desuso)  

**Id. de medidor**: 8de91c94-f740-4d9a-b665-bd5974fa08d4   
**Nombre del medidor**: P15  
**Unidad**: recuento de discos\*horas   
**Notas**: disco administrado estándar - 256 GB (en desuso) 

**Id. de medidor**: c7e7839c-293b-4761-ae4c-848eda91130b   
**Nombre del medidor**: P20   
**Unidad**: recuento de discos\*horas   
**Notas**: disco administrado estándar - 512 GB (en desuso) 

**Id. de medidor**: 9f502103-adf4-4488-b494-456c95d23a9f   
**Nombre del medidor**: P30   
**Unidad**: recuento de discos\*horas   
**Notas**: disco administrado estándar - 1024 GB (en desuso) 

**Id. de medidor**: 8a409390-1913-40ae-917b-08d0f16f3c38   
**Nombre del medidor**: ActualStandardDiskSize   
**Unidad**: byte\*horas      
**Notas**: tamaño real en disco del disco administrado estándar (en desuso)  

**Id. de medidor**: 1273b16f-8458-4c34-8ce2-a515de551ef6  
**Nombre del medidor**: ActualPremiumDiskSize   
**Unidad**: byte\*horas      
**Notas**: tamaño real en disco del disco administrado estándar (en desuso) 

**Id. de medidor**: 89009682-df7f-44fe-aeb1-63fba3ddbf4c  
**Nombre del medidor**: ActualStandardSnapshotSize   
**Unidad**: byte\*horas   
**Notas**: tamaño real en disco de la instantánea estándar administrada (en desuso) 

**Id. de medidor**: 95b0c03f-8a82-4524-8961-ccfbf575f536   
**Nombre del medidor**: ActualPremiumSnapshotSize   
**Unidad**: byte\*horas   
**Notas**: tamaño real en disco de la instantánea premium administrada (en desuso) 

### <a name="sql-rp"></a>Proveedor de recursos de SQL
  
**Id. del medidor**: CBCFEF9A-B91F-4597-A4D3-01FE334BED82  
**Nombre del medidor**: DatabaseSizeHourSqlMeter  
**Unidad**: MB\*horas  
**Notas**: Capacidad total de BD en la creación. Si llama a la API de uso con una granularidad diaria, el medidor devuelve los MB multiplicados por el número de horas.  
  
### <a name="mysql-rp"></a>Proveedor de recursos de MySql   
  
**Id. del medidor**: E6D8CFCD-7734-495E-B1CC-5AB0B9C24BD3  
**Nombre del medidor**: DatabaseSizeHourMySqlMeter  
**Unidad**: MB\*horas  
**Notas**: Capacidad total de BD en la creación. Si llama a la API de uso con una granularidad diaria, el medidor devuelve los MB multiplicados por el número de horas.    
### <a name="key-vault"></a>Key Vault   
  
**Id. del medidor**: EBF13B9F-B3EA-46FE-BF54-396E93D48AB4  
**Nombre del medidor**: Transacciones de Key Vault  
**Unidad**: Recuento de solicitudes en decenas de millar  
**Notas**: Número de solicitudes de la API de REST recibidas por el plan de datos de Key Vault.  
  
**Id. del medidor**: 2C354225-B2FE-42E5-AD89-14F0EA302C87  
**Nombre del medidor**: Transacciones de claves avanzadas  
**Unidad**: Transacciones de 10K  
**Notas**: Transacciones de claves RSA 3K/4K, ECC. (versión preliminar)  
  
### <a name="app-service"></a>App Service   
  
**Id. del medidor**: 190C935E-9ADA-48FF-9AB8-56EA1CF9ADAA  
**Nombre del medidor**: App Service  
**Unidad**: Horas de núcleos virtuales  
**Notas**: Número de núcleos virtuales usados para ejecutar App Service. Nota: Microsoft usa este medidor para realizar los cargos por App Service en Azure Stack. Los proveedores de servicios en la nube pueden usar los otros medidores de App Service (abajo) para calcular el uso en sus inquilinos.  
  
**Id. del medidor**: 67CC4AFC-0691-48E1-A4B8-D744D1FEDBDE  
**Nombre del medidor**: Solicitudes de funciones  
**Unidad**: 10 solicitudes  
**Notas**: Número total de ejecuciones solicitadas (por 10 ejecuciones). Las ejecuciones se cuentan cada vez que se ejecuta una función en respuesta a un evento o se desencadena por un enlace.  
  
**Id. del medidor**: D1D04836-075C-4F27-BF65-0A1130EC60ED  
**Nombre del medidor**: Funciones - Proceso  
**Unidad**: GB-s  
**Notas**: Consumo de recursos medido en gigabytes por segundo (GB/s). El **consumo de recursos observado** se calcula multiplicando el tamaño medio de memoria en GB por el tiempo en milisegundos que tarda en ejecutarse la función. Para medir la memoria que se usa en una función, se redondea al alza a los 128 MB más cercanos hasta un tamaño de memoria máximo de 1536 MB; y para calcular el tiempo de ejecución, se redondea al alza hasta el 1 ms más cercano. El tiempo de ejecución y la memoria mínimos para la ejecución de una única función es de 100 ms y 128 MB, respectivamente.  
  
**Id. del medidor**: 957E9F36-2C14-45A1-B6A1-1723EF71A01D  
**Nombre del medidor**: Horas de App Service compartido  
**Unidad**: 1 hora  
**Notas**: Por cada uso de hora del plan de App Service compartido. Los planes se miden por aplicación.  
  
**Id. del medidor**: 539CDEC7-B4F5-49F6-AAC4-1F15CFF0EDA9  
**Nombre del medidor**: Horas de App Service gratuito  
**Unidad**: 1 hora  
**Notas**: Por cada uso de hora del plan de App Service gratuito. Los planes se miden por aplicación.  
  
**Id. del medidor**: 88039D51-A206-3A89-E9DE-C5117E2D10A6  
**Nombre del medidor**: Horas de App Service para instancias pequeñas estándar  
**Unidad**: 1 hora  
**Notas**: Se calcula en función del tamaño y el número de instancias.  
  
**Id. del medidor**: 83A2A13E-4788-78DD-5D55-2831B68ED825  
**Nombre del medidor**: Horas de App Service para instancias medianas estándar  
**Unidad**: 1 hora  
**Notas**: Se calcula en función del tamaño y el número de instancias.  
  
**Id. del medidor**: 1083B9DB-E9BB-24BE-A5E9-D6FDD0DDEFE6  
**Nombre del medidor**: Horas de App Service para instancias grandes estándar  
**Unidad**: 1 hora  
**Notas**: Se calcula en función del tamaño y el número de instancias.  
  
### <a name="custom-worker-tiers"></a>Niveles de trabajo personalizados   
  
**Id. del medidor**: *Niveles de trabajo personalizados*  
**Nombre del medidor**: Niveles de trabajo personalizados  
**Unidad**: Horas  
**Notas**: El Id. del medidor determinista se crea en función de la SKU y del nombre del nivel de trabajo personalizado. Este identificador de medidor es único para cada nivel de trabajo personalizado.  
  
**Id. del medidor**: 264ACB47-AD38-47F8-ADD3-47F01DC4F473  
**Nombre del medidor**: SSL SNI  
**Unidad**: Por enlace SSL SNI  
**Notas**: App Service admite dos tipos de conexiones SSL: con Indicación de nombre de servidor (SNI) y con dirección IP. SSL basado en SNI funciona en los exploradores modernos, mientras que SSL basado en IP funciona en todos los exploradores.  
  
**Id. del medidor**: 60B42D72-DC1C-472C-9895-6C516277EDB4  
**Nombre del medidor**: SSL de IP  
**Unidad**: Por enlace SSL basado en IP  
**Notas**: App Service admite dos tipos de conexiones SSL: con Indicación de nombre de servidor (SNI) y con dirección IP. SSL basado en SNI funciona en los exploradores modernos, mientras que SSL basado en IP funciona en todos los exploradores.  
  
**Id. del medidor**: 73215A6C-FA54-4284-B9C1-7E8EC871CC5B  
**Nombre del medidor**: Proceso web  
**Unidad**:  
**Notas**: Se calcula por cada sitio activo por hora.  
  
**Id. del medidor**: 5887D39B-0253-4E12-83C7-03E1A93DFFD9  
**Nombre del medidor**: Ancho de banda de salida externo  
**Unidad**: GB  
**Notas**: Bytes totales de respuesta de solicitud de entrada + bytes totales de solicitud de salida + bytes totales de respuesta de solicitud FTP de entrada + bytes totales de respuesta de solicitud de implementación web de entrada.  
  

## <a name="how-do-the-azure-stack-usage-apis-compare-to-the-azure-usage-apihttpsdocsmicrosoftcomazurebillingbilling-usage-rate-card-overviewazure-resource-usage-api-preview-currently-in-public-preview"></a>¿Cómo se comparan las API de uso de Azure Stack con la [API de uso de Azure](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview#azure-resource-usage-api-preview) (actualmente en versión preliminar pública)?
* La API de uso de inquilinos es coherente con la API de Azure, con una excepción: actualmente, la marca *showDetails* no se admite en Azure Stack.
* La API de uso de proveedor solo se aplica a Azure Stack.
* Actualmente, la [API RateCard](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview#azure-resource-ratecard-api-preview) disponible en Azure no se encuentra disponible en Azure Stack.

## <a name="what-is-the-difference-between-usage-time-and-reported-time"></a>¿Cuál es la diferencia entre el tiempo notificado y el tiempo de uso?
Los informes de datos de uso tienen dos valores de tiempo principales:

* **Hora de notificación**. La hora cuando el evento de uso entró en el sistema de uso
* **Tiempo de uso**. El tiempo durante el que se ha usado el recurso de Azure Stack

Puede que vea una discrepancia en los valores de tiempo de uso y tiempo notificado para un evento de uso específico. El retraso puede ser de hasta varias horas en cualquier entorno.

Actualmente, solo se puede consultar mediante el *tiempo notificado*.

## <a name="what-do-these-usage-api-error-codes-mean"></a>¿Qué significan estos códigos de error de la API de uso?
| **Código de estado HTTP** | **Código de error** | **Descripción** |
| --- | --- | --- |
| 400/Solicitud incorrecta |*NoApiVersion* |No se proporcionó el parámetro de consulta de la *api-version*. |
| 400/Solicitud incorrecta |*InvalidProperty* |Falta una propiedad o hay un valor no válido. El mensaje en el código de error en el cuerpo de respuesta identifica la propiedad que falta. |
| 400/Solicitud incorrecta |*RequestEndTimeIsInFuture* |El valor de *ReportedEndTime* no puede ser un tiempo futuro. En el futuro no se permiten valores para este argumento. |
| 400/Solicitud incorrecta |*SubscriberIdIsNotDirectTenant* |Una llamada de API del proveedor ha usado un identificador de suscripción que no es un inquilino válido del autor de la llamada. |
| 400/Solicitud incorrecta |*SubscriptionIdMissingInRequest* |Falta el identificador de suscripción del autor de la llamada. |
| 400/Solicitud incorrecta |*InvalidAggregationGranularity* |Se solicitó una granularidad de agregación no válida. Los valores válidos son daily y hourly. |
| 503 |*ServiceUnavailable* |Se produjo un error recuperable porque el servicio está ocupado o la llamada se está limitando. |

## <a name="next-steps"></a>Pasos siguientes
[Facturación y contracargo del cliente en Azure Stack](azure-stack-billing-and-chargeback.md)

[API de uso de recursos del proveedor](azure-stack-provider-resource-api.md)

[API de uso de recursos del inquilino](azure-stack-tenant-resource-usage-api.md)
