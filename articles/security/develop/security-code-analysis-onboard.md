---
title: Guía de incorporación de Análisis de código de seguridad de Microsoft
description: En este artículo se describe la instalación de la extensión Análisis de código de seguridad de Microsoft
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
ms.openlocfilehash: aaf59de2425e0725958847477f1043a5b7fac68a
ms.sourcegitcommit: 35715a7df8e476286e3fee954818ae1278cef1fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2019
ms.locfileid: "73839360"
---
# <a name="onboarding-and-installing"></a>Incorporación e instalación

Requisitos previos para empezar a trabajar con Análisis de código de seguridad de Microsoft:

- Una oferta válida de Soporte técnico unificado de Microsoft, como se detalla en la sección siguiente.
- Una organización de Azure DevOps.
- Permisos para instalar extensiones en la organización de Azure DevOps.
- Código fuente que se puede sincronizar con una canalización de Azure DevOps hospedada en la nube.

## <a name="onboarding-the-microsoft-security-code-analysis-extension"></a>Incorporación de la extensión Análisis de código de seguridad de Microsoft

- Si ya dispone de una de las siguientes ofertas de soporte técnico, póngase en contacto con el responsable técnico de cuentas y compre o cambie horas existentes para acceder a la extensión:
   - Nivel avanzado de soporte unificado
   - Nivel de rendimiento de soporte unificado
   - Soporte técnico Premier para desarrolladores
   - Soporte técnico Premier para asociados
   - Soporte técnico Premier para empresas
- Si tiene uno de los siguientes servicios de soporte técnico o no tiene ningún plan de soporte técnico de Microsoft, debe actualizarse a una oferta de soporte técnico válida:
   - Soporte técnico para asociados de Azure
   - Soporte técnico Basic de Azure
   - Soporte técnico Developer de Azure
   - Soporte técnico Standard de Azure
   - Professional Direct de Azure
   - Nivel básico de soporte unificado
- Para comprar una oferta de soporte técnico válida, visite nuestra [página principal de servicios de soporte técnico](https://www.microsoft.com/enterprise/services/support).
- Cuando tenga un contrato de soporte técnico, póngase en contacto con su responsable técnico de cuentas para que le ayude a comenzar y a recopilar todos los detalles necesarios.

>[!NOTE]
> Si es un asociado registrado en Microsoft Partner Network, puede comprar soporte técnico Premier para asociados. De lo contrario, debe adquirir una de las ofertas de soporte técnico válidas que se han mencionado anteriormente.

## <a name="installing-the-microsoft-security-code-analysis-extension"></a>Instalación de la extensión Análisis de código de seguridad de Microsoft

1. Después de compartir la extensión con la organización de Azure DevOps, vaya a la página de la organización de Azure DevOps. Una dirección URL de ejemplo para esta página es `https://dev.azure.com/contoso`.
1. Seleccione el icono de la bolsa de la compra que se encuentra en la esquina superior derecha junto a su nombre y haga clic en **Administrar extensiones**.
1. Seleccione la extensión Análisis de código de seguridad de Microsoft y abra el asistente de la interfaz de usuario de Azure DevOps para comenzar la instalación.
1. En la lista desplegable, elija la organización de Azure DevOps en la que va a instalar la extensión.
1. Seleccione **Instalar**. Una vez finalizada la instalación, puede empezar a usar la extensión.

>[!NOTE]
> Aunque no tenga acceso para instalar la extensión, continúe con los pasos de instalación. Puede solicitar acceso al administrador de la organización de Azure DevOps durante el proceso de instalación.

Una vez instalada la extensión, las tareas de compilación de desarrollo seguro estarán visibles y disponibles para agregarse a Azure Pipelines.

## <a name="adding-specific-build-tasks-to-your-azure-devops-pipeline"></a>Incorporación de tareas de compilación específicas a la canalización Azure DevOps

1. Desde la organización de Azure DevOps, abra el proyecto de equipo.
1. Seleccione **Canalizaciones** > **Compilaciones**.
1. Seleccione la canalización a la que quiere agregar las tareas de compilación de la extensión:
   - Nueva canalización: Seleccione **Nuevo** y siga los pasos detallados para crear una canalización.
   - Editar canalización: Seleccione una canalización existente y haga clic en **Editar** para empezar a editarla.
1. Seleccione **+** y vaya al panel **Agregar tareas**.
1. En la lista o mediante el cuadro de búsqueda, busque la tarea de compilación que quiere agregar. Seleccione **Agregar**.
1. Especifique los parámetros necesarios para la tarea.
1. Ponga en cola una nueva compilación.
   >[!NOTE]
   >Las rutas de acceso de archivo y carpeta son relativas a la raíz del repositorio de origen. Si especifica los archivos y las carpetas de salida como parámetros, se reemplazan por la ubicación común que hemos definido en el agente de compilación.

> [!TIP]
>
> - Para ejecutar el análisis después de la compilación, coloque las tareas de compilación de Análisis de código de seguridad de Microsoft después del paso Publicar los artefactos de la compilación de la compilación. De este modo, la compilación puede finalizar y publicar los resultados antes de ejecutar las herramientas de análisis estático.
> - Seleccione siempre **Continuar en caso de error** para las tareas de compilación de desarrollo seguro. Incluso si se produce un error en una de las herramientas, las demás podrán ejecutarse. No hay interdependencias entre las herramientas.
> - Las tareas de compilación de Análisis de código de seguridad de Microsoft solo producen un error si una herramienta no se ejecuta correctamente. Sin embargo, se ejecutan correctamente aunque una herramienta identifique problemas en el código. Mediante la tarea de compilación Posanálisis, puede configurar la compilación para que se interrumpa cuando una herramienta identifique problemas en el código.
> - Algunas tareas de compilación de Azure DevOps no se admiten cuando se ejecutan a través de una canalización de versión. Más concretamente, Azure DevOps no admite tareas que publican artefactos desde una canalización de versión.
> - Para ver una lista de variables predefinidas en Azure DevOps Team Build, que puede especificar como parámetros, consulte las [variables de compilación de Azure DevOps](https://docs.microsoft.com/azure/devops/pipelines/build/variables?tabs=batch&view=vsts).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre la configuración de las tareas de compilación, consulte nuestra [guía de configuración](security-code-analysis-customize.md).

Si tiene más preguntas sobre la extensión y las herramientas que se ofrecen, consulte nuestra página de [preguntas más frecuentes.](security-code-analysis-faq.md)
