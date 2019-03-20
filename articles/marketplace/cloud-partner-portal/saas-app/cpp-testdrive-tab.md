---
title: Configuración de la versión de prueba para una oferta de aplicación SaaS de Azure | Microsoft Docs
description: Configure la versión de prueba para la oferta de aplicación SaaS en Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 12/04/2018
ms.author: pbutlerm
ms.openlocfilehash: e97e79ef3077431d8fc627cd634a67b0a01b6f8e
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57993108"
---
# <a name="saas-application-test-drive-tab"></a>Pestaña Versión de prueba de una aplicación SaaS

Use la pestaña Versión de prueba para proporcionar una experiencia de evaluación para sus clientes.

## <a name="test-drive-benefits"></a>Ventajas de la versión de prueba

La creación de una experiencia de evaluación para los clientes es un procedimiento recomendado para garantizar que puedan comprar con confianza. De las opciones de prueba disponibles, Versión de prueba es la más eficaz a la hora de generar clientes potenciales de alta calidad y lograr un aumento en la conversión de estos.

La versión de prueba proporciona a los clientes una versión de prueba práctica y autodidacta con las principales características y ventajas del producto, y una demostración en un escenario de implementación del mundo real.

## <a name="how-a-test-drive-works"></a>Funcionamiento de una versión de prueba

Un cliente potencial busca y encuentra la aplicación en Marketplace. El cliente inicia sesión y acepta los términos de uso. En ese momento, el cliente recibe su entorno preconfigurado de prueba durante un número determinado de horas, mientras que usted recibe una notificación de cliente potencial altamente cualificado que le permite realizar un seguimiento del mismo. Para más información, consulte [¿Qué es la versión de prueba?](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive)

## <a name="publishing-steps"></a>Pasos para la publicación

Los pasos de publicación principales para agregar una versión de prueba son:

1. Definición del escenario de la versión de prueba
2. Compilación o modificación de la plantilla de Azure Resource Manager
3. Creación del manual detallado de la versión de prueba
4. Nueva publicación de la oferta

## <a name="setting-up-a-test-drive"></a>Configuración de una versión de prueba

Hay cuatro tipos diferentes de versiones de prueba disponibles, cada uno basado en el tipo de producto, el escenario y el Marketplace en el que se encuentra.

|  **Tipo**          |  **Descripción**  |  **Instrucciones de configuración**  |
|  ---------------   |  ---------------  |  ---------------  |
|     Azure Resource Manager               |    Una versión de prueba de Azure Resource Manager es una plantilla de implementación que contiene todos los recursos de Azure que componen una solución que está compilando el anunciante. Los productos que pertenecen a este tipo de versión de prueba son aquellos que solo usan recursos de Azure.               |       [Versión de prueba de Azure Resource Manager](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive)            |
|       Hospedada             |       Una versión de prueba hospedada elimina la complejidad de la configuración haciendo que Microsoft hospede y mantenga el servicio que realiza el aprovisionamiento y desaprovisionamiento de usuarios de la versión de prueba.             |         [Versión de prueba hospedada](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/hosted-test-drive)          |
|      Aplicación lógica              |       Una versión de prueba de aplicación lógica es una plantilla de implementación que está diseñada para incluir todas las arquitecturas de soluciones complejas. Todas las aplicaciones o productos personalizados de Dynamics deberían usar este tipo de versión de prueba.            |      [Versión de prueba de aplicación lógica](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/logic-app-test-drive)             |
|       Power BI             |         Una versión de prueba de Power BI consta de un vínculo insertado a un panel con una compilación personalizada. Aquellos productos que deseen realizar una demostración de un objeto visual interactivo de Power BI deben usar este tipo de versión de prueba. Todo lo que necesita para la carga es la dirección URL insertada de Power BI.          |        [Versión de prueba de Power BI](#Power-BI-test-drive)           |

### <a name="power-bi-test-drive"></a>Versión de prueba de Power BI

Para configurar una versión de prueba, siga los pasos a continuación:

1. En Nueva oferta, seleccione **Versión de prueba**.
2. En Versión de prueba, seleccione **Sí**.

   ![Habilitación de la versión de prueba](./media/saas-enable-test-drive.png)

   Cuando habilite una versión de prueba, podrá ver los formularios Detalles y Configuración técnica, que se muestran en la siguiente captura de pantalla.

   ![Formulario de configuración de la versión de prueba](./media/saas-test-drive-yes.png)

3. En **Detalles**, proporcione información para los campos siguientes:
  
   - Descripción: Describa la versión de prueba y qué pueden hacer con ella los usuarios. Puede usar etiquetas HTML básicas para dar formato a esta descripción.
   - Manual del usuario: Cargue un documento Manual del usuario que los clientes puedan usar con la versión de prueba. Este manual debe ser un archivo .pdf.
   - Test Drive Demo Video (Vídeo de demostración de la versión de prueba) (opcional): Puede proporcionar un vídeo (YouTube o Vimeo) para que los clientes vean antes de que entren en la versión de prueba. Proporcione una dirección URL para el vídeo.

4. En **Configuración técnica**, proporcione información para los campos siguientes:

   - Type of Test Drive (Tipo de versión de prueba): Seleccione **Power BI** de la lista desplegable.
   - Link to Power BI Dashboard (Vínculo al panel de Power BI): Proporcione un vínculo al panel.

5. Cuando termine de configurar la versión de prueba, seleccione **Guardar**.


## <a name="next-steps"></a>Pasos siguientes

[Pestaña Detalles del escaparate](./cpp-storefront-tab.md)