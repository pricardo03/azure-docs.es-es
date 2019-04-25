---
title: Configuración y uso de entornos públicos en Azure DevTest Labs | Microsoft Docs
description: Aprenda a configurar y usar entornos públicos en Azure DevTest Labs.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/13/2018
ms.author: spelluru
ms.openlocfilehash: 2cd6998c7ac11638ead67fde384bdf4599692781
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60201794"
---
# <a name="configure-and-use-public-environments-in-azure-devtest-labs"></a>Configuración y uso de entornos públicos en Azure DevTest Labs
Azure DevTest Labs tiene un [repositorio público de plantillas de Azure Resource Manager](https://github.com/Azure/azure-devtestlab/tree/master/Environments) que puede usar para crear entornos sin tener que conectarse a un origen externo de GitHub por sí mismo. Este repositorio incluye plantillas usadas con frecuencia, como Azure Web Apps, Clúster de Service Fabric y entorno de desarrollo de Granja de SharePoint. Esta característica es similar al repositorio público de artefactos que se incluye para todos los laboratorios que cree. El repositorio de entorno le permite empezar a trabajar rápidamente con las plantillas de entorno ya creadas, con el mínimo de parámetros de entrada, para proporcionarle una experiencia de introducción sin problemas a los recursos de PaaS en laboratorios. 

## <a name="configuring-public-environments"></a>Configuración de entornos públicos
Como propietario de un laboratorio, puede habilitar el repositorio público del entorno para el laboratorio durante la creación del laboratorio. Para habilitar los entornos públicos para el laboratorio, seleccione **Activado** en el campo **Entornos públicos** al crear un laboratorio. 

![Habilitación de un entorno público para un nuevo laboratorio](media/devtest-lab-configure-use-public-environments/enable-public-environment-new-lab.png)


Para ver laboratorios existentes, el repositorio del entorno público no está habilitado. Habilítelo manualmente para utilizar plantillas en el repositorio. Para los laboratorios creados mediante plantillas de Resource Manager, el repositorio también está deshabilitado de forma predeterminada.

Puede habilitar o deshabilitar entornos públicos para el laboratorio y también poner a disposición solo entornos específicos para los usuarios del laboratorio mediante los pasos siguientes: 

1. Seleccione **Configuración y directivas** para el laboratorio. 
2. En la sección **BASES PARA MÁQUINAS VIRTUALES**, seleccione **Entornos públicos**.
3. Para habilitar entornos públicos para el laboratorio, seleccione **Sí**. De lo contrario, seleccione **No**. 
4. Si habilitó los entornos públicos, todos los entornos en el repositorio se habilitan de manera predeterminada. Puede anular la selección de un entorno para que no esté disponible para los usuarios del laboratorio. 

![Página Entornos públicos](media/devtest-lab-configure-use-public-environments/public-environments-page.png)

## <a name="use-environment-templates-as-a-lab-user"></a>Uso de plantillas de entorno como usuario del laboratorio
Como usuario del laboratorio, puede crear un entorno a partir de la lista de plantillas de entorno habilitada simplemente seleccionando **+Agregar** desde la barra de herramientas en la página del laboratorio. La lista de bases incluye las plantillas de entornos públicos habilitadas por el administrador del laboratorio en la parte superior de la lista.

![Plantillas de entornos públicos](media/devtest-lab-configure-use-public-environments/public-environment-templates.png)

## <a name="next-steps"></a>Pasos siguientes
Este es un repositorio de código abierto al que puede contribuir con sus propias plantillas de Resource Manager usadas con frecuencia y útiles. Para contribuir, basta con enviar una solicitud de incorporación de cambios en el repositorio.  
