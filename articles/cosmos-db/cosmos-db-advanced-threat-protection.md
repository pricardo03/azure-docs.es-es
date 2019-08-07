---
title: Advanced Threat Protection para Azure Cosmos DB
description: Obtenga información acerca de cómo Azure Cosmos DB proporciona cifrado de datos en reposo y cómo se implementa.
author: monhaber
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/24/2019
ms.author: v-mohabe
ms.custom: seodec18
ms.openlocfilehash: 02281a1cad9c7e6f9680441a699fa5d34558b890
ms.sourcegitcommit: a0b37e18b8823025e64427c26fae9fb7a3fe355a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/25/2019
ms.locfileid: "68501590"
---
# <a name="advanced-threat-protection-for-azure-cosmos-db"></a>Advanced Threat Protection para Azure Cosmos DB

Advanced Threat Protection para Azure Cosmos DB proporciona un nivel adicional de inteligencia de seguridad que detecta intentos poco habituales y potencialmente peligrosos de acceder a las cuentas de Azure Cosmos DB o vulnerarlas. Esta capa de protección le permite afrontar las amenazas, incluso sin necesidad de ser un experto en seguridad, e integrarlas con sistemas centrales de supervisión de seguridad.

Las alertas de seguridad se desencadenan cuando se producen anomalías en una actividad. Estas alertas de seguridad se integran en  [Azure Security Center](https://azure.microsoft.com/services/security-center/) y también se envían por correo electrónico a los administradores de las suscripciones, con detalles de la actividad sospechosa y recomendaciones sobre cómo investigar y solucionar las amenazas.

> [!NOTE]
>
> * Advanced Threat Protection para Azure Cosmos DB está actualmente disponible solo para SQL API.
> * Advanced Threat Protection para Azure Cosmos DB está disponible actualmente en Azure Government y en regiones de nube soberana.

Para una experiencia de investigación completa de las alertas de seguridad, se recomienda habilitar el [registro de diagnóstico en Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/logging), que registra las operaciones en la base de datos, incluidas las operaciones CRUD en todos los documentos, contenedores y bases de datos.

## <a name="set-up-advanced-threat-protection"></a>Configuración de Advanced Threat Protection

### <a name="set-up-atp-using-the-portal"></a>Configuración de ATP con el portal

1. Inicie Azure Portal en   [https://portal.azure.com](https://portal.azure.com/).

2. En la cuenta de Azure Cosmos DB, en el menú **Configuración**, seleccione **Seguridad avanzada**.

    ![Configuración de ATP](./media/cosmos-db-advanced-threat-protection/cosmos-db-atp.png)

3. En la hoja de configuración de  **Seguridad avanzada**:

    * Haga clic en la opción **Advanced Threat Protection** para configurarla en **ACTIVADA**.
    * Haga clic en  **Guardar**  para guardar la directiva de Advanced Threat Protection nueva o actualizada.   

### <a name="set-up-atp-using-rest-api"></a>Configuración de ATP mediante la API de REST

Use comandos de la API de REST para crear, actualizar u obtener la configuración de Advanced Threat Protection para una cuenta de Azure Cosmos DB específica.

* [Creación de Advanced Threat Protection](https://go.microsoft.com/fwlink/?linkid=2099745)
* [Obtención de Advanced Threat Protection](https://go.microsoft.com/fwlink/?linkid=2099643)

### <a name="set-up-atp-using-azure-powershell"></a>Configuración de ATP con Azure PowerShell

Use los siguientes cmdlets de PowerShell:

* [Habilitación de Advanced Threat Protection](https://go.microsoft.com/fwlink/?linkid=2099607&clcid=0x409)
* [Obtención de Advanced Threat Protection](https://go.microsoft.com/fwlink/?linkid=2099608&clcid=0x409)
* [Deshabilitación de Advanced Threat Protection](https://go.microsoft.com/fwlink/?linkid=2099709&clcid=0x409)

## <a name="manage-atp-security-alerts"></a>Administración de alertas de seguridad de ATP

Cuando se producen anomalías en la actividad de Azure Cosmos DB, se desencadena una alerta de seguridad con información sobre el evento de seguridad sospechoso. 

 Desde Azure Security Center, se pueden revisar y administrar las [alertas de seguridad](../security-center/security-center-alerts-overview.md) actuales.  Haga clic en una alerta específica en [Security Center](https://ms.portal.azure.com/#blade/Microsoft_Azure_Security/SecurityMenuBlade/0) para ver las posibles causas y las medidas recomendadas para investigar y mitigar la potencial amenaza. En la imagen siguiente se muestra un ejemplo de los detalles de la alerta proporcionados en Security Center.

 ![Detalles de la amenaza](./media/cosmos-db-advanced-threat-protection/cosmos-db-alert-details.png)

También se envía una notificación por correo electrónico con los detalles de la alerta y las medidas recomendadas. En la imagen siguiente se muestra un ejemplo de un correo electrónico de alerta.

 ![Detalles de alertas](./media/cosmos-db-advanced-threat-protection/cosmos-db-alert.png)

## <a name="cosmos-db-atp-alerts"></a>Alertas de ATP de Cosmos DB

 Para ver una lista de las alertas generadas al supervisar cuentas de Azure Cosmos DB, consulte la sección [Alertas de Cosmos DB](../security-center/security-center-alerts-data-services.md#cosmos-db) en la documentación de Security Center.

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre el  [Registro de diagnóstico de Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/logging#turn-on-logging-in-the-azure-portal).
* Más información sobre  [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro).