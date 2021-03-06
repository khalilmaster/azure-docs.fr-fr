---
title: Documentation de référence pour l’entité prédéfinie datetimev2 de LUIS - Azure | Microsoft Docs
titleSuffix: Azure
description: Cet article contient des informations sur l’entité prédéfinie datetimev2 dans Language Understanding (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 261f6f27c39c280efdcd070888d735374a473c85
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321571"
---
# <a name="datetimev2-entity"></a>Entité Datetimev2

L’entité prédéfinie **datetimeV2** extrait des valeurs de date et d’heure. Ces valeurs se résolvent dans un format standardisé que les programmes clients peuvent exploiter. Quand un énoncé a une date ou une heure incomplètes, LUIS inclut les _valeurs passées et futures_ dans la réponse du point de terminaison. Étant donné que cette entité est déjà formée, vous n’avez pas besoin d’ajouter d’exemples d’énoncés contenant datetimev2 aux intentions de l’application. 

## <a name="types-of-datetimev2"></a>Types de datetimev2
Datetimev2 est géré à partir du dépôt GitHub [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-DateTime.yaml)

## <a name="example-json"></a>Exemple de JSON 
L’exemple suivant de réponse JSON a une entité `datetimeV2` avec un sous-type `datetime`. Pour obtenir des exemples d’autres types d’entités datetimev2, voir [Sous-types de datetimev2](#subtypes-of-datetimev2)</a>.

```JSON
"entities": [
  {
    "entity": "8am on may 2nd 2017",
    "type": "builtin.datetimeV2.datetime",
    "startIndex": 15,
    "endIndex": 30,
    "resolution": {
      "values": [
        {
          "timex": "2017-05-02T08",
          "type": "datetime",
          "value": "2017-05-02 08:00:00"
        }
      ]
    }
  }
]
  ```

## <a name="json-property-descriptions"></a>Description des propriétés JSON

|Nom de la propriété |Type et description de la propriété|
|---|---|
|Entity|**string** : texte extrait de l’énoncé avec le type de date, d’heure, de plage de dates ou de plage d’heures.|
|type|**string** : l’un des [sous-types de datetimev2](#subtypes-of-datetimev2).
|startIndex|**int** : index dans l’énoncé auquel l’entité commence.|
|endIndex|**int** : index dans l’énoncé auquel l’entité finit.|
|resolution|Tableau `values` contenant une, deux ou quatre [valeurs de résolution](#values-of-resolution).|
|end|Valeur de fin d’une plage d’heures ou de dates, au même format que `value`. Utilisée uniquement si `type` est `daterange`, `timerange` ou `datetimerange`.|

## <a name="subtypes-of-datetimev2"></a>Sous-types de datetimev2

L’entité prédéfinie **datetimev2** a les sous-types ci-dessous, pour lesquels le tableau suivant fournit des exemples :
* `date`
* `time`
* `daterange`
* `timerange`
* `datetimerange`
* `duration`
* `set`

## <a name="values-of-resolution"></a>Valeurs de résolution
* Le tableau contient un seul élément si la date ou l’heure dans l’énoncé sont complètement spécifiées et non équivoques.
* Le tableau contient deux éléments si la valeur datetimev2 est ambiguë. L’ambiguïté résulte de l’absence d’année, d’heure ou d’intervalle de temps spécifiques. Pour obtenir des exemples, voir [Dates ambiguës](#ambiguous-dates). Quand l’heure est ambiguë au sens où elle ne permet pas de déterminer s’il s’agit de l’avant-midi (AM) ou de l’après-midi (PM), les deux valeurs sont incluses.
* Le tableau contient quatre éléments si l’énoncé comporte deux éléments ambigus. Cette ambiguïté a trait aux éléments qui incluent :
  * Une date ou une plage de dates ambiguës pour l’année
  * Une heure ou une plage d’heures ambiguës parce qu’elles ne permettent pas de déterminer s’il s’agit de l’avant-midi (AM) ou de l’après-midi (PM). Par exemple, le 3 avril à 3 heures.

Chaque élément du tableau `values` peut comprendre les champs suivants : 

|Nom de la propriété|Description de la propriété|
|--|--|
|timex|Heure, date ou plage de dates exprimées au format TIMEX qui suit la [norme ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) et les attributs TIMEX3 pour l’annotation à l’aide du langage TimeML. Cette annotation est décrite dans les [recommandations TIMEX](http://www.timeml.org/tempeval2/tempeval2-trial/guidelines/timex3guidelines-072009.pdf).|
|type|Sous-type qui peut être l’un des éléments suivants : datetime, date, time, daterange, timerange, datetimerange, duration, set.|
|value|**Facultatif.** Objet datetime au format aaaa:MM:jj (date), HH:mm:ss (heure), aaaa:MM:dd HH:mm:ss (dateheure). Si `type` est `duration`, la valeur est le nombre de secondes (durée). <br/> Utilisé uniquement si `type` est `datetime`, `date`, `time` ou une durée.|

## <a name="valid-date-values"></a>Valeurs de date valides

L’entité **datetimev2** prend en charge les dates comprises dans les plages suivantes :

| Min | max |
|----------|-------------|
| 1er janvier 1900   | 31 décembre 2099 |

## <a name="ambiguous-dates"></a>Dates ambiguës

Si la date peut être dans le passé ou dans le futur, LUIS fournit les deux valeurs. Il s’agit, par exemple, d’un énoncé incluant le mois et la date sans spécifier l’année.  

Par exemple, prenons l’énoncé "May 2nd" (2 mai) :
* Si la date du jour est le 3 mai 2017, LUIS fournit les valeurs "2017-05-02" (02-05-2017) et "2018-05-02" (02-05-2018). 
* Quand la date du jour est le 1er mai 2017, LUIS fournit les valeurs "2016-05-02" (02-05-2016) et "201è-05-02" (02-05-2017).

L’exemple suivant montre la résolution de l’entité "may 2nd" (2 mai). Cette solution suppose que la date du jour est comprise entre le 2 mai 2017 et le 1er mai 2018.
Les `X` dans le champ `timex` correspondent à des parties de la date qui ne sont pas explicitement spécifiées dans l’énoncé.

```JSON
  "entities": [
    {
      "entity": "may 2nd",
      "type": "builtin.datetimeV2.date",
      "startIndex": 0,
      "endIndex": 6,
      "resolution": {
        "values": [
          {
            "timex": "XXXX-05-02",
            "type": "date",
            "value": "2017-05-02"
          },
          {
            "timex": "XXXX-05-02",
            "type": "date",
            "value": "2018-05-02"
          }
        ]
      }
    }
  ]
```

## <a name="date-range-resolution-examples-for-numeric-date"></a>Exemples de résolution de plage de dates pour la date numérique

L’entité `datetimeV2` extrait les plages de dates et d’heures. Les champs `start` et `end` indiquent respectivement le début et la fin de la plage. Pour l’énoncé "may 2nd to may 5th" (du 2 mai au 5 mai), LUIS fournit les valeurs **daterange** (plage de dates) valeurs pour l’année en cours et l’année suivante. Dans le champ `timex`, les valeurs `XXXX` indiquent l’ambiguïté de l’année. `P3D` indique que la période est de trois jours.

```JSON
"entities": [
    {
      "entity": "may 2nd to may 5th",
      "type": "builtin.datetimeV2.daterange",
      "startIndex": 0,
      "endIndex": 17,
      "resolution": {
        "values": [
          {
            "timex": "(XXXX-05-02,XXXX-05-05,P3D)",
            "type": "daterange",
            "start": "2017-05-02",
            "end": "2017-05-05"
          },
          {
            "timex": "(XXXX-05-02,XXXX-05-05,P3D)",
            "type": "daterange",
            "start": "2018-05-02",
            "end": "2018-05-05"
          }
        ]
      }
    }
  ]
```

## <a name="date-range-resolution-examples-for-day-of-week"></a>Exemples de résolution de plage de dates pour le jour de la semaine

L’exemple suivant montre comment LUIS utilise **datetimev2** pour résoudre l’énoncé "Tuesday to Thursday" (Mardi à jeudi). Dans cet exemple, la date du jour est le 19 juin. LUIS inclut les valeurs **daterange** (plage de dates) pour les plages de dates qui précèdent et qui suivent la date du jour.

```JSON
  "entities": [
    {
      "entity": "tuesday to thursday",
      "type": "builtin.datetimeV2.daterange",
      "startIndex": 0,
      "endIndex": 19,
      "resolution": {
        "values": [
          {
            "timex": "(XXXX-WXX-2,XXXX-WXX-4,P2D)",
            "type": "daterange",
            "start": "2017-06-13",
            "end": "2017-06-15"
          },
          {
            "timex": "(XXXX-WXX-2,XXXX-WXX-4,P2D)",
            "type": "daterange",
            "start": "2017-06-20",
            "end": "2017-06-22"
          }
        ]
      }
    }
  ]
```
## <a name="ambiguous-time"></a>Heure ambiguë
Le tableau des valeurs contient deux éléments si l’heure ou la plage d’heures sont ambiguës. Quand une heure est ambiguë, les valeurs comprennent les deux possibilités (AM et PM) .

## <a name="time-range-resolution-example"></a>Exemple de résolution de plage d’heures

L’exemple suivant montre comment LUIS utilise **datetimev2** pour résoudre l’énoncé contenant une plage d’heures.

```
  "entities": [
    {
      "entity": "6pm to 7pm",
      "type": "builtin.datetimeV2.timerange",
      "startIndex": 0,
      "endIndex": 9,
      "resolution": {
        "values": [
          {
            "timex": "(T18,T19,PT1H)",
            "type": "timerange",
            "start": "18:00:00",
            "end": "19:00:00"
          }
        ]
      }
    }
  ]
```

## <a name="deprecated-prebuilt-datetime"></a>Désapprobation de l’entité prédéfinie datetime

L’entité prédéfinie `datetime` est désapprouvée et remplacée par [`datetimeV2`](#builtindatetimev2). 

Pour remplacer `datetime` par `datetimeV2` dans votre application LUIS, procédez comme suit :

1. Ouvrez le volet **Entities** (Entités) l’interface web de LUIS. 
2. Supprimez l’entité prédéfinie **datetime**.
3. Cliquez sur **Add prebuilt entity** (Ajouter une entité prédéfinie).
4. Sélectionnez **datetimev2**, puis cliquez sur **Save** (Enregistrer).

## <a name="next-steps"></a>Étapes suivantes

En savoir plus sur les entités [dimension](luis-reference-prebuilt-dimension.md), [email](luis-reference-prebuilt-email.md) et [number](luis-reference-prebuilt-number.md). 

