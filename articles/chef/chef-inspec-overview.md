---
title: Uso de InSpec para automatizar el cumplimiento de normativas en una infraestructura de Azure
description: Información acerca de cómo usar InSpec para detectar problemas en las implementaciones de Azure
keywords: azure, chef, devops, máquinas virtuales, información general, automatizar, inspec
ms.service: virtual-machines-linux
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 05/15/2018
ms.topic: article
ms.openlocfilehash: a5de2ca04a193f97a2a65a043f744abb8e0ea758
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2019
ms.locfileid: "54359228"
---
# <a name="use-inspec-for-compliance-automation-of-your-azure-infrastructure"></a>Uso de InSpec para automatizar el cumplimiento de normativas en una infraestructura de Azure
[InSpec](https://www.chef.io/inspec/) es el lenguaje de código abierto de Chef para describir reglas de seguridad y cumplimiento que se pueden compartir entre los ingenieros de software, las operaciones y los ingenieros de seguridad. InSpec compara el estado real de la infraestructura con el estado deseado que usted expresa en código de InSpec que puede leer y escribir fácilmente. InSpec detecta infracciones y muestra los resultados en informes, pero le permite decidir cómo corregirlas.

Puede usar InSpec para validar el estado de los recursos y grupos de recursos dentro de una suscripción, incluidas las máquinas virtuales, las configuraciones de red, la configuración de Azure Active Directory y mucho más.

Este artículo describe las ventajas de usar InSpec para facilitar la seguridad y cumplimiento de normas en Azure.

## <a name="make-compliance-easy-to-understand-and-assess"></a>Cumplimiento fácil de entender y evaluar
La documentación de cumplimiento escrito en hojas de cálculo o los documentos de Word dejan los requisitos abiertos a la interpretación. Con InSpec, transformará sus requisitos en código de lenguaje natural, ejecutable y con control de versiones. El código acaba con las especulaciones sobre qué debería evaluarse reemplazándolas por pruebas tangibles con una intención bien definida.

## <a name="detect-fleet-wide-issues-and-prioritize-their-remediation"></a>Detección de problemas globales y priorización de su corrección
El modo de detección sin agente de InSpec le permite evaluar rápidamente (a escala) el nivel de riesgo. Los metadatos integrados para la puntuación de la gravedad ayudan a determinar en qué áreas centrar las estrategias de corrección. También puede escribir reglas rápidamente en respuesta a las nuevas vulnerabilidades o reglamentos e implementarlas inmediatamente.

## <a name="satisfy-audits"></a>Cumplimiento de auditorías
Con InSpec, puede responder a las cuestiones que se formulan en una auditoría en cualquier momento, no solo en intervalos predeterminados, como trimestral o anualmente. Al ejecutar pruebas de InSpec de forma continua, podrá saber exactamente su historial y nivel de cumplimiento, con lo que las conclusiones de los auditores no le pillarán por sorpresa.

## <a name="next-steps"></a>Pasos siguientes

* Probar InSpec en Azure Cloud Shell [![Iniciar Cloud Shell](https://shell.azure.com/images/launchcloudshell.png "Iniciar Cloud Shell")](https://shell.azure.com)
