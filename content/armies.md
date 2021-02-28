---
weight: 11 
title: Armies
---

# Armies

The `armies` endpoint is the primary endpoint of the REST API. It allows you to list out all armies, and their
associated units from the Warhammer Ago of Sigmar universe.

## List Armies

```go
package main

import (
	"fmt"

	"github.com/brittonhayes/go-warhammer/api"
)

func main() {
	a, err := api.GetArmies()
	if err != nil {
		panic(err)
	}

	fmt.Println(a)
}
```

```shell
curl "http://aos-api.com/armies"
```

> The above command returns JSON structured like this:

```json
{
  "count": 1,
  "data": [
    {
      "army": "Daughters of Khaine",
      "units": [
        {
          "name": "Avatar of Khaine",
          "size": "40 mm",
          "move": "",
          "save": "",
          "bravery": "",
          "wounds": "",
          "missile_weapon": [
            {
              "name": "",
              "range": "",
              "attacks": "",
              "to_hit": "",
              "to_wound": "",
              "rend": "",
              "damage": ""
            }
          ],
          "melee_weapon": [
            {
              "name": "",
              "range": "",
              "attacks": "",
              "to_hit": "",
              "to_wound": "",
              "rend": "",
              "damage": ""
            }
          ],
          "abilities": [
            {
              "name": "",
              "desc": ""
            }
          ],
          "keywords": [
            ""
          ]
        }
      ]
    }
  ]
}
```

This endpoint retrieves all armies.

### HTTP Request

`GET http://aos-api.com/armies`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
limit     | `100`    | Limit the number of results returned

## Find Army

This endpoint allows you find a singular army from the API. An army includes the list of associated units, and the count
of total results.

<aside class="warning">
    Army names must contain lower-case characters and space-separated. 
    Otherwise, your request will be considered invalid.
</aside>

```go
package main

import (
	"fmt"

	"github.com/brittonhayes/go-warhammer/api"
)

func main() {
	a, err := api.FindArmy("daughters-of-khaine")
	if err != nil {
		panic(err)
	}

	fmt.Println(a)
}
```

```shell
curl "http://aos-api.com/armies/daughters-of-khaine"
```

> The above command returns JSON structured like this:

```json
{
  "count": 1,
  "data": {
    "army": "Daughters of Khaine",
    "units": [
      {
        "name": "Avatar of Khaine",
        "size": "40 mm",
        "move": "",
        "save": "",
        "bravery": "",
        "wounds": "",
        "missile_weapon": [
          {
            "name": "",
            "range": "",
            "attacks": "",
            "to_hit": "",
            "to_wound": "",
            "rend": "",
            "damage": ""
          }
        ],
        "melee_weapon": [
          {
            "name": "",
            "range": "",
            "attacks": "",
            "to_hit": "",
            "to_wound": "",
            "rend": "",
            "damage": ""
          }
        ],
        "abilities": null,
        "keywords": [
          ""
        ]
      }
    ]
  }
}
```

### HTTP Request

`GET http://aos-api.com/armies/{name}`

### Path Parameters

Parameter | Default | Description
--------- | ------- | -----------
name      | `""`    | The specific army to be returned
