---
title: Guía de incorporación de Análisis de código de seguridad de Microsoft Azure
description: Este artículo incluye información sobre la instalación de la extensión Análisis de código de seguridad
author: vharindra
manager: sukhans
ms.author: terrylan
ms.date: 07/31/2019
ms.topic: article
ms.service: security
services: azure
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: 8a8eca73205d4f8f33d4d069fda72638af61d355
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68718175"
---
# <a name="how-to-onboarding-and-installing"></a>Instrucciones: Incorporación e instalación

Requisitos previos para empezar a trabajar con Análisis de código de seguridad de Microsoft
  - Oferta válida de servicios de Soporte técnico unificado de Microsoft (detalles a continuación)
  - Una organización de Azure DevOps
  - Permisos para instalar extensiones en la organización de Azure DevOps
  - Código fuente que se puede sincronizar con una canalización de Azure DevOps hospedada en la nube


## <a name="onboarding-microsoft-security-code-analysis-extension"></a>Incorporación de la extensión Análisis de código de seguridad de Microsoft

- Si ya dispone de una de las siguientes ofertas de soporte técnico, simplemente póngase en contacto con el responsable técnico de cuentas y compre o cambie horas existentes para acceder a la extensión.
   - Soporte técnico unificado: nivel Avanzado y Rendimiento, soporte técnico Premier para desarrolladores, soporte técnico Premier para asociados o soporte técnico Premier para empresas.
- Si tiene uno de los siguientes servicios de soporte técnico o no tiene ningún plan de soporte técnico de Microsoft, tendrá que actualizar a una oferta de soporte técnico válida:
   - Soporte técnico de Azure para asociados, soporte técnico Basic de Azure, soporte técnico Developer de Azure, soporte técnico Standard de Azure, soporte técnico Professional Direct de Azure o soporte técnico unificado (nivel básico)
- Para comprar una oferta de soporte técnico válida, visite [nuestra página principal de servicios de soporte técnico.](https://www.microsoft.com/enterprise/services/support)
- Una vez que haya un contrato de soporte técnico, póngase en contacto con su responsable técnico de cuentas que puede ayudarle a comenzar y recopile todos los detalles necesarios.
 
>[!NOTE]
> Solo los asociados registrados en Microsoft Partner Network tienen derecho a adquirir soporte técnico Premier para asociados; de lo contrario, adquiera una de las ofertas de soporte técnico válidas mencionadas anteriormente.

## <a name="installing-microsoft-security-code-analysis-extension"></a>Instalación de la extensión Análisis de código de seguridad de Microsoft

1. Una vez que la extensión se comparte con la organización de Azure DevOps, vaya a la página de la organización de Azure DevOps (por ejemplo, http://dev.azure.com/contoso).
2. Haga clic en el icono de la bolsa de la compra que se encuentra en la esquina superior derecha junto a su nombre y haga clic en Administrar extensiones. 
3. Haga clic en la extensión Análisis de código de seguridad de Microsoft e inicie el asistente para la interfaz de usuario de Azure DevOps para iniciar la instalación.
4. En la lista desplegable, elija la organización de Azure DevOps en la que instalar la extensión.
5. Haga clic en Instalar. Una vez completada, puede continuar utilizando la extensión.

>[!NOTE]
> Aunque no tenga acceso, continúe con los pasos de instalación y podrá solicitar acceso al administrador de la organización de Azure DevOps durante el proceso.
>
Una vez instalada la extensión, las tareas de compilación de desarrollo seguro estarán visibles y disponibles para agregarse a Azure Pipelines.

## <a name="adding-specific-build-tasks-to-your-devops-pipeline"></a>Incorporación de tareas de compilación específicas a la canalización DevOps

1. Abra el proyecto de equipo de la organización de Azure DevOps.
2. Vaya a la pestaña **Compilaciones** en **Canalizaciones**. 
3. Seleccione la canalización a la que desea agregar las tareas de compilación de la extensión. 
   - Nuevo: haga clic en **Nuevo** y siga los pasos detallados para crear una nueva canalización.
   - Editar: seleccione la canalización y haga clic en **Editar** para empezar a editar una canalización existente.
4. Haga clic en + para navegar al panel **Agregar tareas**.
5. Busque la tarea de compilación que desea agregar de la lista o use el cuadro de búsqueda y luego haga clic en **Agregar**. 
6. Ahora puede seguir especificando los parámetros necesarios para la tarea.
>[!NOTE]
>Las rutas de acceso de archivos o directorios son relativas a la raíz del repositorio de origen y los parámetros que especifican la carpeta o los archivos de salida se reemplazarán por la ubicación común que se haya definido en el agente de compilación.

7. Ponga en cola una nueva compilación.
> [!TIP]
>  - Para ejecutar el análisis después de la compilación, coloque las tareas de compilación de Análisis de código de seguridad de Microsoft después del paso Publicar los artefactos de la compilación de la compilación. De este modo, la compilación puede finalizar y publicar los resultados antes de ejecutar las herramientas de análisis estático.
>  - Compruebe siempre la opción **"Continuar en caso de error"** de las tareas de compilación de desarrollo seguro. Incluso si se produce un error en una de las herramientas, las demás podrán ejecutarse. No hay interdependencias.
>  - SOLO se producirá un error en las tareas de compilación de Análisis de código de seguridad de Microsoft si la herramienta no se puede ejecutar correctamente, pero NO se producirán errores si la herramienta identifica problemas en el código. Mediante la tarea de compilación **Posanálisis**, puede configurar la compilación para que se interrumpa cuando una herramienta identifique problemas en el código.
>  - Algunas tareas de compilación de Azure DevOps no se admiten cuando se ejecutan a través de una canalización “Versión”. Se trata de una limitación de Azure DevOps. No admiten tareas que publican artefactos desde una canalización Versión.
>  - Para una lista de variables predefinidas en Azure DevOps Team Build, que puede especificar como parámetros, consulte [Variables de compilación de Azure DevOps](https://docs.microsoft.com/azure/devops/pipelines/build/variables?tabs=batch&view=vsts)

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la configuración de las tareas de compilación, consulte nuestra [guía de configuración](security-code-analysis-customize.md).

Si tiene más preguntas sobre la extensión y las herramientas que se ofrecen, [consulte nuestra página de preguntas más frecuentes](security-code-analysis-faq.md).


