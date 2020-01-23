---
title: Solución de problemas de Azure Internet Analyzer | Microsoft Docs
description: La referencia de solución de problemas de Azure Internet Analyzer.
services: internet-analyzer
author: diego-perez-botero
ms.service: internet-analyzer
ms.topic: guide
ms.date: 12/04/2019
ms.author: dibotero
ms.openlocfilehash: 74bf0422bbe6c2c1c84365c1e8f9329a01ff9fdd
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2020
ms.locfileid: "75909037"
---
# <a name="azure-internet-analyzer-troubleshooting"></a>Solución de problemas de Azure Internet Analyzer

Este artículo contiene los pasos para la solución de problemas comunes de Internet Analyzer.

## <a name="azure-portal"></a>Azure Portal
**"No se recopilaron suficientes medidas para este cuadro de mandos" en la sección Cuadros de mandos**

Observe lo siguiente:
- El script de cliente se debe insertar en un sitio web **HTTPS**. No se recopilarán datos de medición si el script se ejecuta en un sitio web de texto no cifrado (**http://** ) o un sitio local (**file://** ).
- Los datos de medición solo se recopilarán si el script de cliente del perfil de Internet Analyzer se ha incrustado en una aplicación que recibe tráfico de un usuario real. En general, el tráfico sintético (por ejemplo, las pruebas de rendimiento de Azure WebApp) no ejecuta código JavaScript incrustado, por lo que ese tipo de tráfico no generará ninguna medida.
- La serie temporal se genera cada hora, por lo que tendrá que esperar al menos esa cantidad de tiempo para que se muestren los nuevos datos de medición.
- Los cuadros de mandos se generan a diario (al final de cada día, hora UTC).
- Los cuadros de mandos solo se generan si se han recopilado más de 100 medidas para la combinación de filtros seleccionada (prueba, período de tiempo, país, etc.).

## <a name="next-steps"></a>Pasos siguientes
Consulte [Preguntas frecuentes sobre Internet Analyzer](internet-analyzer-faq.md)
