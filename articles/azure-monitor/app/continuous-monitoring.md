---
title: Supervisión continua de la canalización de versión de DevOps con Azure DevOps y Azure Application Insights | Microsoft Docs
description: Proporciona instrucciones para configurar rápidamente una supervisión continua con Application Insights
services: application-insights
keywords: ''
author: mrbullwinkle
ms.author: mbullwin
ms.date: 11/13/2017
ms.service: application-insights
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 41999defb01e024773b6364f169a1ce3b1377237
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54056735"
---
# <a name="add-continuous-monitoring-to-your-release-pipeline"></a>Incorporación de supervisión continua a la canalización de versión

Azure DevOps Services se integra con Azure Application Insights para permitir la supervisión continua de la canalización de versión de DevOps a lo largo del ciclo de vida de desarrollo del software. 

Azure DevOps Services admite ahora la supervisión continua mediante la cual las canalizaciones de versión pueden incorporar datos de supervisión de Application Insights y otros recursos de Azure. Cuando se detecta una alerta de Application Insights, la implementación puede permanecer controlada o se puede revertir hasta que se resuelva la alerta. Si todas las comprobaciones son correctas, las implementaciones pueden continuar automáticamente de prueba a producción sin necesidad de intervención manual. 

## <a name="configure-continuous-monitoring"></a>Configuración de la supervisión continua

1. Seleccione un proyecto existente de Azure DevOps Services.

2. Mantenga el mouse sobre **Compilación y lanzamiento** > Seleccione **Versiones** > haga clic en el **signo más** > **Crear definición de versión**  > Busque **Supervisión** > **Azure App Service Deployment with Continuous Monitoring** (implementación de Azure App Service Deployment con supervisión continua)

   ![Nueva canalización de versión de Azure DevOps Services](media/continuous-monitoring/001.png)

3. Haga clic en **Aplicar.**

4. Junto al punto rojo con la exclamación, seleccione el texto en azul para **Ver tareas de entorno.**

   ![Ver tareas de entorno](media/continuous-monitoring/002.png)

   Aparecerá un cuadro de configuración, use la tabla siguiente para rellenar los campos de entrada.

    | Parámetro        | Valor |
   | ------------- |:-----|
   | **Nombre del entorno**      | Nombre que describe el entorno de la canalización de versión |
   | **Suscripción de Azure** | La lista desplegable se rellena con cualquier suscripción a Azure vinculada a la organización de Azure DevOps Services|
   | **Nombre de App Service** | Dependiendo de otras selecciones puede ser necesaria la introducción manual de un nuevo valor para este campo |
   | **Grupo de recursos**    | La lista desplegable se rellena con los grupos de recursos disponibles |
   | **Nombre de recurso de Application Insights** | La lista desplegable se rellena con todos los recursos de Application Insights que se corresponden con el grupo de recursos seleccionado anteriormente.

5. Seleccione **Configure Application Insights alerts** (Configurar alertas de Application Insights)

6. Para las reglas de alertas predeterminadas, seleccione **Guardar** > Escriba un comentario descriptivo > Haga clic en **Aceptar**

## <a name="modify-alert-rules"></a>Modificación de las reglas de alertas

1. Para modificar la configuración de alertas predefinida, haga clic en el cuadro con **puntos suspensivos...**  a la derecha de **Reglas de alertas.**

   (Hay cuatro reglas de alerta listas para su uso presentes: Disponibilidad, Solicitudes con errores, Tiempo de respuesta del servidor, Excepciones de servidor).

2. Haga clic en el símbolo de la lista desplegable junto a **Disponibilidad.**

3. Modifique el **umbral** de disponibilidad para satisfacer sus necesidades de nivel de servicio.

   ![Modificación de alerta](media/continuous-monitoring/003.png)

4. Seleccione **Aceptar** > **Guardar** > escriba un comentario descriptivo > haga clic en **Aceptar.**

## <a name="add-deployment-conditions"></a>Incorporación de condiciones de implementación

1. Haga clic en **Canalización** > seleccione el símbolo de las **Condiciones anteriores** o **posteriores** a la implementación según el escenario que requiera una puerta de supervisión continua.

   ![Condiciones anteriores a la implementación](media/continuous-monitoring/004.png)

2. Establezca **Gates** (Puertas) en **Habilitado** > **Approval gates** (Puertas de aprobación) > haga clic en **Agregar.**

3. Seleccione **Azure Monitor** (esta opción le permite el acceso a las alertas tanto de Azure Monitor como de Application Insights)

    ![Azure Monitor](media/continuous-monitoring/005.png)

4. Escriba un valor de **tiempo de expiración de la puerta**.

5. Escriba un **Intervalo de muestreo.**

## <a name="deployment-gate-status-logs"></a>Registros de estado de la puerta de implementación

Una vez que haya agregado puertas de implementación, una alerta en Application Insights que supera el umbral definido previamente, protege la implementación de la promoción de versión no deseada. Una vez que se resuelva la alerta, la implementación puede continuar automáticamente.

Para observar este comportamiento, seleccione **Versiones** > haga clic con el botón derecho en el nombre de la versión **abrir** > **registros.**

![Registros](media/continuous-monitoring/006.png)

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure Pipelines, pruebe estos [inicios rápidos](https://docs.microsoft.com/azure/devops/pipelines).
