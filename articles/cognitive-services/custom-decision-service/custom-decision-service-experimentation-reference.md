---
title: 'Experimentación: Custom Decision Service'
titlesuffix: Azure Cognitive Services
description: Este artículo es una guía para realizar experimentos con Custom Decision Service.
services: cognitive-services
author: marco-rossi29
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-decision-service
ms.topic: conceptual
ms.date: 05/10/2018
ms.author: marossi
ms.openlocfilehash: b5f8c853218a1db53f4dd23e7254b35990a7132b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60829181"
---
# <a name="experimentation"></a>Experimentación

Siguiendo la teoría de los [bandidos contextuales (CB)](https://www.microsoft.com/en-us/research/blog/contextual-bandit-breakthrough-enables-deeper-personalization/), Custom Decision Service observa repetidamente un contexto, realiza una acción y observa una recompensa para la acción elegida. Un ejemplo es la personalización de contenido: el contexto describe a un usuario, las acciones son los casos de los candidatos y la recompensa mide cuánto le gustó al usuario la historia recomendada.

Custom Decision Service puede crear una directiva, ya que asigna elementos que van desde los contextos a las acciones. Si usa una directiva de objetivos específica, puede conocer la recompensa que se espera. Una forma de estimar la recompensa es usar una directiva en línea y dejar que esta elija las acciones (por ejemplo, recomendar historias a los usuarios). Sin embargo, dicha evaluación en línea puede ser costosa por dos razones:

* Expone a los usuarios a una directiva experimental no probada.
* No realiza escalados para evaluar múltiples directivas de destino.

De todos modos, la evaluación fuera de la directiva es un paradigma alternativo. Si tiene registros de un sistema en línea que sigue una directiva de registro, la evaluación que se realiza fuera de la directiva puede estimar las recompensas esperadas de las nuevas directivas de destino.

Al usar el archivo de registro, Experimentación busca la directiva que tenga la recompensa estimada más alta. Las directivas de destino se parametrizan con los argumentos de [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit/wiki). En el modo predeterminado, el script comprueba una serie de argumentos de Vowpal Wabbit mediante la anexión a la `--base_command`. Este script realiza las acciones siguientes:

* Detecta automáticamente los espacios de nombres de las características, a partir de las primeras `--auto_lines` líneas del archivo de entrada.
* Realiza un barrido inicial sobre los hiperparámetros (`learning rate`, `L1 regularization` y `power_t`).
* Prueba la evaluación de la directiva `--cb_type`: puntuación de propensión inversa (`ips`) o doblemente sólida (`dr`). Para obtener más información, consulte [Contextual Bandit example](https://github.com/JohnLangford/vowpal_wabbit/wiki/Contextual-Bandit-Example) (Ejemplo de bandido contextual).
* Pruebas marginales.
* Prueba las funciones de interacción cuadrática:
   * **fase de fuerza bruta**: prueba todas las combinaciones con pares `--q_bruteforce_terms` o menos.
   * **fase expansiva**: agrega el mejor par hasta que no haya ninguna mejora en rondas `--q_greedy_stop`.
* Realiza un segundo barrido sobre los hiperparámetros (`learning rate`, `L1 regularization` y `power_t`).

Los parámetros que controlan estos pasos incluyen algunos argumentos de Vowpal Wabbit:
- Ejemplo de opciones de manipulación:
  - espacios de nombres compartidos
  - espacios de nombres de acción
  - espacios de nombres marginales
  - características cuadráticas
- Opciones de la regla de actualización
  - velocidad de aprendizaje
  - Regularización L1
  - valor de la potencia de t

Para obtener una explicación detallada de los argumentos anteriores, consulte [Vowpal Wabbit command-line arguments](https://github.com/JohnLangford/vowpal_wabbit/wiki/Command-line-arguments) (Argumentos de la línea de comandos de Vowpal Wabbit).

## <a name="prerequisites"></a>Requisitos previos
- Vowpal Wabbit: instalado y en su ruta de acceso.
  - Windows: [use el `.msi` instalador](https://github.com/eisber/vowpal_wabbit/releases).
  - Otras plataformas: [Obtención del código fuente](https://github.com/JohnLangford/vowpal_wabbit/releases).
- Python 3: instalado y en su ruta de acceso.
- NumPy: use el administrador de paquetes de su elección.
- El repositorio *Microsoft/mwt-ds*: [Clonación del repositorio](https://github.com/Microsoft/mwt-ds).
- Archivo de registro JSON de Decision Service: de forma predeterminada, el comando base incluye `--dsjson`, que permite el análisis JSON de Decision Service del archivo de datos de entrada. [Obtenga un ejemplo de este formato](https://github.com/JohnLangford/vowpal_wabbit/blob/master/test/train-sets/decisionservice.json).

## <a name="usage"></a>Uso
Vaya a `mwt-ds/DataScience` y ejecute `Experimentation.py` con los argumentos necesarios, tal como se detalla en el siguiente código:

```cmd
python Experimentation.py [-h] -f FILE_PATH [-b BASE_COMMAND] [-p N_PROC]
                          [-s SHARED_NAMESPACES] [-a ACTION_NAMESPACES]
                          [-m MARGINAL_NAMESPACES] [--auto_lines AUTO_LINES]
                          [--only_hp] [-l LR_MIN_MAX_STEPS]
                          [-r REG_MIN_MAX_STEPS] [-t PT_MIN_MAX_STEPS]
                          [--q_bruteforce_terms Q_BRUTEFORCE_TERMS]
                          [--q_greedy_stop Q_GREEDY_STOP]
```

Encontrará adjunto un registro de los resultados en el archivo *mwt-ds/DataScience/experiments.csv*.

### <a name="parameters"></a>Parámetros
| Entrada | DESCRIPCIÓN | Valor predeterminado |
| --- | --- | --- |
| `-h`, `--help` | Muestra este mensaje de ayuda y sale. | |
| `-f FILE_PATH`, `--file_path FILE_PATH` | Ruta de acceso del archivo de datos (formato `.json` o `.json.gz`: cada línea es un elemento `dsjson`). | Obligatorio |  
| `-b BASE_COMMAND`, `--base_command BASE_COMMAND` | Comando base de Vowpal Wabbit.  | `vw --cb_adf --dsjson -c` |  
| `-p N_PROC`, `--n_proc N_PROC` | Número de procesos paralelos que se van a usar. | Procesadores lógicos |  
| `-s SHARED_NAMESPACES, --shared_namespaces SHARED_NAMESPACES` | Espacios de nombres de características compartidas (por ejemplo, `abc` indica los espacios de nombres `a`, `b` y `c`).  | Detección automática del archivo de datos. |  
| `-a ACTION_NAMESPACES, --action_namespaces ACTION_NAMESPACES` | Espacios de nombres de características de la acción. | Detección automática del archivo de datos. |  
| `-m MARGINAL_NAMESPACES, --marginal_namespaces MARGINAL_NAMESPACES` | Espacios de nombres de características marginales. | Detección automática del archivo de datos. |  
| `--auto_lines AUTO_LINES` | Número de líneas del archivo de datos que hay que escanear para detectar automáticamente los espacios de nombres de las características. | `100` |  
| `--only_hp` | Hacer un barrido solo sobre los hiperparámetros (`learning rate`, `L1 regularization` y `power_t`). | `False` |  
| `-l LR_MIN_MAX_STEPS`, `--lr_min_max_steps LR_MIN_MAX_STEPS` | Rango de la tasa de aprendizaje como los valores positivos `min,max,steps`. | `1e-5,0.5,4` |  
| `-r REG_MIN_MAX_STEPS`, `--reg_min_max_steps REG_MIN_MAX_STEPS` | Rango de la regularización L1 como los valores positivos `min,max,steps`. | `1e-9,0.1,5` |  
| `-t PT_MIN_MAX_STEPS`, `--pt_min_max_steps PT_MIN_MAX_STEPS` | Rango de Power_t como los valores positivos `min,max,step`. | `1e-9,0.5,5` |  
| `--q_bruteforce_terms Q_BRUTEFORCE_TERMS` | Número de pares cuadráticos que se probarán en la fase de fuerza bruta. | `2` |  
| `--q_greedy_stop Q_GREEDY_STOP` | Rondas sin mejoras, después de las cuales se detiene la fase de búsqueda expansiva cuadrática. | `3` |  

### <a name="examples"></a>Ejemplos
Para usar los valores predeterminados preestablecidos:
```cmd
python Experimentation.py -f D:\multiworld\data.json
```

De igual modo, Vowpal Wabbit también puede ingerir archivos `.json.gz`:
```cmd
python Experimentation.py -f D:\multiworld\data.json.gz
```

Debe realizar el barrido solamente en hiperparámetros (`learning rate`, `L1 regularization` y `power_t`; debe detenerse después del paso 2):
```cmd
python Experimentation.py -f D:\multiworld\data.json --only_hp
```
