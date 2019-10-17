---
title: Conexión de datos de inteligencia sobre amenazas con Azure Sentinel | Microsoft Docs
description: Obtenga información sobre cómo conectar datos de inteligencia sobre amenazas a Azure Sentinel.
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/24/2019
ms.author: rkarlin
ms.openlocfilehash: 44b3830465bf2b5aa06612aa868b086b120f1ece
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72372275"
---
# <a name="connect-data-from-threat-intelligence-providers"></a>Conectar datos de proveedores de inteligencia sobre amenazas

> [!IMPORTANT]
> El conector de datos de las plataformas de inteligencia sobre amenazas en Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta característica se ofrece sin contrato de nivel de servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Sentinel le permite importar los indicadores de amenazas que usa su organización, lo cual puede mejorar la capacidad de los analistas de seguridad para detectar y priorizar las amenazas conocidas. Varias características de Azure Sentinel están disponibles o se han mejorado:

- **Analytics** incluye un conjunto de plantillas de reglas programadas que puede habilitar para generar alertas e incidentes que se basan en coincidencias con eventos de registro de los indicadores de amenazas.

- **Libros** proporciona información resumida sobre los indicadores de amenazas que se importan en Azure Sentinel y las alertas generadas a partir de las reglas de análisis que coinciden con los indicadores de amenazas.

- **Búsqueda** de consultas permite a los investigadores de seguridad usar indicadores de amenazas en el contexto de los escenarios de búsqueda habituales.

- **Notebooks** puede usar indicadores de amenazas al investigar anomalías y buscar comportamientos malintencionados.

Puede transmitir los indicadores de amenazas a Azure Sentinel mediante uno de los productos integrados de la plataforma de inteligencia sobre amenazas (TIP) que se enumeran en la sección siguiente, o mediante la integración directa con [Microsoft Graph Security tiIndicators API](https://aka.ms/graphsecuritytiindicators).

## <a name="integrated-threat-intelligence-platform-products"></a>Productos de la plataforma de inteligencia sobre amenazas integrada

- [Plataforma de inteligencia sobre amenazas de código abierto de MISP](https://www.misp-project.org/)
    
    Para ver un script de ejemplo que proporciona a los clientes instancias de MISP para migrar indicadores de amenazas a Microsoft Graph Security API, consulte el artículo [MISP to Microsoft Graph Security Script](https://github.com/microsoftgraph/security-api-solutions/tree/master/Samples/MISP) (MISP para un script de Microsoft Graph Security).

- [Palo Alto Networks MineMeld](https://www.paloaltonetworks.com/products/secure-the-network/subscriptions/minemeld)
    
    Para obtener instrucciones detalladas, consulte [Sending IOCs to the Microsoft Graph Security API using MineMeld](https://live.paloaltonetworks.com/t5/MineMeld-Articles/Sending-IOCs-to-the-Microsoft-Graph-Security-API-using-MineMeld/ta-p/258540) (Envío de IOC a Microsoft Graph Security API mediante MineMeld).

- [Plataforma ThreatConnect](https://threatconnect.com/solution/)


## <a name="prerequisites"></a>Requisitos previos  

- Rol de Azure AD de administrador global o de administrador de seguridad para conceder permisos al producto TIP o a la aplicación personalizada que usa integración directa con Microsoft Graph Security tiIndicators API.

- Permisos de lectura y escritura en el área de trabajo de Azure Sentinel para almacenar los indicadores de amenazas.

## <a name="connect-azure-sentinel-to-your-threat-intelligence-provider"></a>Conexión de Azure Sentinel a su proveedor de inteligencia sobre amenazas

1. [Registre una aplicación](/graph/auth-v2-service#1-register-your-app) en Azure Active Directory para obtener un identificador de aplicación, un secreto de aplicación y un identificador de inquilino de Azure Active Directory. Necesitará estos valores para cuando configure el producto TIP o la aplicación integrada que usa la integración directa con Microsoft Graph Security tiIndicators API.

2. [Configure los permisos de la API](/graph/auth-v2-service#2-configure-permissions-for-microsoft-graph) para la aplicación registrada: Agregue el permiso de aplicación de Microsoft Graph **ThreatIndicators.ReadWrite.OwnedBy** a la aplicación registrada.

3. Pida a su administrador de inquilinos de Azure Active Directory que conceda consentimiento de administrador a la aplicación registrada de su organización. En el Portal de Azure: **Azure Active Directory** > **Registros de aplicaciones** >  **\<_nombre de aplicación_>**  > **Ver permisos de API** > **Grant admin consent for (Conceder consentimiento del administrador para) \<_nombre del inquilino_>** .

4. Configure el producto TIP o la aplicación que usa la integración directa con Microsoft Graph Security tiIndicators API para enviar indicadores a Azure Sentinel especificando lo siguiente:
    
    a. Los valores del identificador, el secreto y el identificador de inquilino de la aplicación registrada.
    
    b. Para el producto de destino, especifique Azure Sentinel.
    
    c. Para la acción, especifique alerta.

5. En Azure Portal, vaya a **Azure Sentinel** > **Data connectors** (Conectores de datos) y, a continuación, seleccione el conector **Plataformas de inteligencia sobre amenazas (versión preliminar)** .

6. Seleccione **Open connector page** (Abrir página del conductor) y, a continuación, **Conectar**.

7. Para ver los indicadores de amenazas que se importan en Azure Sentinel, vaya a **Azure Sentinel: Registros** > **SecurityInsights** y amplíe **ThreatIntelligenceIndicator**.

## <a name="next-steps"></a>Pasos siguientes

En este documento, ha aprendido a conectar su proveedor de inteligencia sobre amenazas a Azure Sentinel. Para obtener más información sobre Azure Sentinel, consulte los siguientes artículos.

- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats.md).
