---
title: Control de datos y registro de Azure Stack | Microsoft Docs
description: Obtenga información acerca de cómo Azure Stack recopila información.
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2019
ms.author: PatAltimore
ms.reviewer: chengwei
ms.lastreviewed: 02/14/2019
ms.openlocfilehash: 7beb9dc31e8987d69c2cf9cf64be36caf0e0e308
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/15/2019
ms.locfileid: "56319023"
---
# <a name="azure-stack-log-and-customer-data-handling"></a>Control de datos de clientes y registro de Azure Stack 
*Se aplica a: Sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*  

En la medida en que Microsoft es un procesador o subprocesador de datos personales en relación con Azure Stack, Microsoft cumple para todos los clientes, a partir del 25 de mayo de 2018, los compromisos reflejados en a) la disposición "Procesamiento de los Datos Personales; RGPD"de la sección [Términos de protección de los datos](https://nam06.safelinks.protection.outlook.com/?url=http%3A%2F%2Fwww.microsoftvolumelicensing.com%2FDocumentSearch.aspx%3FMode%3D3%26DocumentTypeId%3D31&data=02%7C01%7Ccomartin%40microsoft.com%7Ce2ce478261764c79c3f308d68df01136%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636852459551078818&sdata=cpWsfZTBHpqEFr50DWQOryq342U8shgeFgMXVPQz5ug%3D&reserved=0) y b) los términos del reglamento general de protección de datos de la Unión Europea en el anexo 4 de los [Términos de los servicios online](https://nam06.safelinks.protection.outlook.com/?url=http%3A%2F%2Fwww.microsoftvolumelicensing.com%2FDocumentSearch.aspx%3FMode%3D3%26DocumentTypeId%3D31&data=02%7C01%7Ccomartin%40microsoft.com%7Ce2ce478261764c79c3f308d68df01136%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636852459551088813&sdata=bv1CBiaCnYmjiv6S0dFCbWEd4fNCkPBjBwgylNa%2FNt0%3D&reserved=0). 

Como Azure Stack reside en los centros de datos del cliente, Microsoft es controlador únicamente de los datos que se comparten con Microsoft a través de [diagnósticos](azure-stack-diagnostics.md), [telemetría](azure-stack-telemetry.md) y [facturación](azure-stack-usage-reporting.md).  

## <a name="data-access-controls"></a>Controles de acceso de datos 
Los empleados de Microsoft, que se asignan para investigar un caso de soporte técnico específico, recibirán acceso de solo lectura a los datos cifrados. Los empleados de Microsoft también tienen acceso a las herramientas que pueden usarse para eliminar los datos si es necesario. Todo el acceso a los datos de los clientes se audita y se registra.  

Controles de acceso de datos:
1.  Los datos solo se conservan durante un máximo de 90 días después de cerrar el caso.
2.  El cliente siempre tiene la opción de eliminar los datos en cualquier momento durante ese período de 90 días.
3.  Los empleados de Microsoft tienen acceso a los datos de cada caso individual y solo cuando es necesario para ayudar a resolver el problema de soporte técnico. 
4.  En el caso de que Microsoft deba compartir los datos del cliente con los socios OEM, el consentimiento del cliente es obligatorio.  

### <a name="what-data-subject-requests-dsr-controls-do-customers-have"></a>¿Qué controles de solicitudes del titular de los datos tienen los clientes?
Como se mencionó anteriormente, Microsoft admite la eliminación de datos bajo demanda del cliente. Los clientes pueden solicitar que nuestro ingeniero de soporte elimine todos sus registros para un caso determinado en cualquier momento que elijan, antes de que los datos se borren permanentemente.  

### <a name="does-microsoft-notify-customers-when-the-data-is-deleted"></a>¿Microsoft notifica a los clientes la eliminación de los datos?
En el caso de la acción de eliminación automática de datos (90 días después del cierre del caso), no nos ponemos en contacto con los clientes de manera proactiva para informarles sobre la eliminación. 

En el caso de la acción de eliminación de datos bajo demanda, el ingeniero de soporte técnico de Microsoft tiene acceso a la herramienta que puede iniciar la eliminación de datos a petición y puede proporcionar confirmación por teléfono al cliente cuando haya terminado.

## <a name="diagnostic-data"></a>Datos de diagnóstico
Como parte del proceso de soporte técnico, los operadores de Azure Stack pueden [compartir los registros de diagnóstico](azure-stack-diagnostics.md) con los equipos de soporte técnico e ingeniería de Azure Stack para facilitar la solución de problemas.

Microsoft proporciona una herramienta y un script para que los clientes recopilen y carguen los archivos de registro de diagnóstico solicitados. Una vez recopilados, los archivos de registro se transfieren a través de una conexión cifrada protegida por HTTPS a Microsoft. Como HTTPS proporciona el cifrado a través de la conexión, no se necesita ninguna contraseña para el cifrado en tránsito. Una vez que se reciben, los registros se cifran y se almacenan hasta que se eliminen automáticamente 90 días después de cerrar el caso de soporte técnico.

## <a name="telemetry-data"></a>Datos de telemetría
La [telemetría de Azure Stack](azure-stack-telemetry.md) carga automáticamente datos del sistema en Microsoft mediante la Experiencia del usuario asociada. Los operadores de Azure Stack disponen de controles para personalizar características y configuraciones de privacidad en cualquier momento.

La intención de Microsoft no es recopilar datos confidenciales, como números de tarjeta de crédito, nombres de usuario y contraseñas, direcciones de correo electrónico u otra información igualmente confidencial. Si determinamos que, por accidente, se ha recibido información confidencial, la eliminamos. 

## <a name="billing-data"></a>Datos de facturación
La [facturación de Azure Stack](azure-stack-usage-reporting.md) hace uso del sistema global de facturación y uso de Azure y, por lo tanto, está en consonancia con las pautas de cumplimiento de Microsoft.

Los operadores de Azure Stack pueden configurar Azure Stack para reenviar la información de uso a Azure con fines de facturación. Esta información es necesaria para los clientes de Azure Stack de varios nodos que eligen el modelo de facturación de pago por uso. Los informes de uso se controlan de forma independiente de la telemetría y no son necesarios para los clientes de varios nodos que eligen el modelo de capacidad o para los usuarios del Kit de desarrollo de Azure Stack. En estos escenarios, los informes de uso se pueden desactivar [mediante el script de registro](azure-stack-usage-reporting.md).


## <a name="next-steps"></a>Pasos siguientes 
[Más información acerca de la seguridad de Azure Stack](azure-stack-security-foundations.md) 
