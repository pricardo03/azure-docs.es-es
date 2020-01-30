---
title: Configuración de Advanced Threat Protection para Azure Key Vault | Microsoft Docs
description: En este artículo se explica cómo configurar Advanced Threat Protection para Azure Key Vault en Azure Security Center
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: f7f716734875d652de575991a4dc1b7bfe948ae3
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2020
ms.locfileid: "76773534"
---
# <a name="set-up-advanced-threat-protection-for-azure-key-vault-preview"></a>Configuración de Advanced Threat Protection para Azure Key Vault (versión preliminar)

Advanced Threat Protection para Azure Key Vault proporciona una capa adicional de inteligencia de seguridad. Esta herramienta permite detectar intentos potencialmente perjudiciales de aprovechar las vulnerabilidades de sus cuentas de CosmosDB o de acceder a ellas. Gracias a la instancia nativa de Advanced Threat Protection de Azure Security Center, puede abordar amenazas sin ser un experto en seguridad y sin tener que aprender a manejar más sistemas de supervisión de seguridad.

Cuando Security Center detecta una actividad anómala, muestra alertas. También envía por correo electrónico al administrador de la suscripción los detalles de la actividad sospechosa y recomendaciones sobre cómo investigar y corregir las amenazas identificadas.

## <a name="set-up-advanced-threat-protection-from-azure-security-center"></a>Configuración de Advanced Threat Protection en Azure Security Center

De forma predeterminada, Advanced Threat Protection está habilitado en todas las cuentas de Key Vault cuando se suscribe al nivel estándar de Security Center. Para obtener más información, consulte el apartado [Precios](security-center-pricing.md).

Para habilitar o deshabilitar la protección de una suscripción específica, siga estos pasos.

1. En el panel izquierdo de Security Center, seleccione **Precios y configuración**.
1. Seleccione la suscripción con las cuentas de almacenamiento para las que quiera habilitar o deshabilitar la protección contra amenazas.
1. Seleccione **Plan de tarifa**.
1. En el grupo **Seleccionar el plan de tarifa por tipo de recurso**, busque la fila de **Key Vault** y haga clic en **Habilitado** o **Deshabilitado**.

    [![Habilitar o deshabilitar Advanced Threat Protection de Key Vault en Azure Security Center](media/advanced-threat-protection-key-vault/atp-for-akv-enable-atp-for-akv.png)](media/advanced-threat-protection-key-vault/atp-for-akv-enable-atp-for-akv.png#lightbox)
1. Seleccione **Guardar**.


## <a name="next-steps"></a>Pasos siguientes

En este artículo aprendió a habilitar y deshabilitar Advanced Threat Protection de Azure Key Vault. 

Para obtener material relacionado, consulte el siguiente artículo:

- [Detección de amenazas para la capa de servicios de Azure en Security Center](security-center-alerts-service-layer.md): en este artículo se describen las alertas relacionadas con Advanced Threat Protection para Azure Key Vault.
