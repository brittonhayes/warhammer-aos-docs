---
weight: 12 
title: Resources
---

# Resources

Resources are the types of objects returned by the REST API. This includes the overall JSON structure and data types in
each of the fields (e.g. string, array, object etc).

Each of the resources has an associated JSON Schema definition available in the API's Github repository in the `/schema`
folder.

## Army

An army is the foundational resource for all battles in Warhammer. Each army contains a list of relevant units that fall
under this army.

```go
package main

// Army is the parent structure that all nested
// units belong to
type Army struct {
	Army  string `json:"army"`
	Units []Unit `json:"units"`
}
```

> JSON Schema

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "$id": "armies_schema.json",
  "type": "object",
  "properties": {
    "army": {
      "$id": "#/properties/army",
      "type": "string"
    },
    "units": {
      "$id": "#/properties/units",
      "type": "array",
      "items": {
        "$ref": "unit_schema.json#/properties/unit"
      }
    }
  }
}
```

### Response

Field    | Type          | Description
-------- | -----         | -----------
count    | `int`           | The total number of results in the request
army     | `string`        | The name of the army
units    | `array of Unit` | The list of units associated with this army

## Unit

A unit is the individual fighting group of the army and contains a more detailed set of fields representing their
characteristics. Characteristics including abilities, weapons, size, name and more.

```go
package main

// Unit is an individual unit in Warhammer
type Unit struct {
	Name          string          `json:"name"`
	Size          string          `json:"size"`
	Move          string          `json:"move"`
	Save          string          `json:"save"`
	Bravery       string          `json:"bravery"`
	Wounds        string          `json:"wounds"`
	MissileWeapon []MissileWeapon `json:"missile_weapon,omitempty"`
	MeleeWeapon   []MissileWeapon `json:"melee_weapon,omitempty"`
	Abilities     []Ability       `json:"abilities"`
	Keywords      []string        `json:"keywords"`
}

```

> JSON Structure

```json
{
  "$id": "#/properties/unit",
  "type": "object",
  "required": [
    "name",
    "size",
    "move",
    "save",
    "bravery",
    "wounds",
    "abilites",
    "keywords"
  ],
  "properties": {
    "name": {
      "type": "string",
      "pattern": "^[A-Za-z'].*"
    },
    "size": {
      "type": "string",
      "pattern": "^([0-9xm]|Use model mm).*"
    },
    "move": {
      "type": "string",
      "pattern": "^([0-9+-]|See Damage Table)"
    },
    "save": {
      "type": "string",
      "pattern": "^[0-9+-]"
    },
    "bravery": {
      "type": "string",
      "pattern": "^[0-9+-]"
    },
    "wounds": {
      "type": "string",
      "pattern": "^[0-9+-]"
    },
    "missile_weapon": {
      "type": "array",
      "items": {
        "$ref": "#/definitions/missile"
      }
    },
    "melee_weapon": {
      "type": "array",
      "items": {
        "$ref": "#/definitions/melee"
      }
    },
    "abilites": {
      "type": "array",
      "items": {
        "$ref": "#/definitions/ability"
      }
    },
    "keywords": {
      "type": "array",
      "items": {
        "$ref": "#/definitions/keyword"
      }
    },
    "command_abilities": {
      "type": "array",
      "items": {
        "$ref": "#/definitions/attribute"
      }
    },
    "magic": {
      "type": "array",
      "items": {
        "$ref": "#/definitions/attribute"
      }
    },
    "damage_table": {
      "type": "array",
      "items": {
        "$ref": "#/definitions/dmg_table"
      }
    }
  }
}
```

### Type

Field           | Type                        | Description
--------        | -----                       | -----------
name            | `string`                    | The name of the unit
size            | `string`                    | The name of the unit
move            | `string`                    | The move of the unit
save            | `string`                    | The save of the unit
bravery         | `string`                    | The bravery of the unit
missile_weapon  | `array of Missile Weapon`   | The list of ranged weapons for this unit
melee_weapon    | `array of Melee Weapon`     | The list of close quarters weapons for this unit
keywords        | `array`                     | The list of relevant keywords for this unit
abilities       | `array of Ability`          | The list of abilities for this unit

## Missile Weapon

A unit's ranged weapon.

```go
package main

// MissileWeapon is a weapon that is used
// for ranged attacks
type MissileWeapon struct {
	Name    string `json:"name"`
	Range   string `json:"range"`
	Attacks string `json:"attacks"`
	ToHit   string `json:"to_hit"`
	ToWound string `json:"to_wound"`
	Rend    string `json:"rend"`
	Damage  string `json:"damage"`
}
```

> JSON Schema

```json
{
  "$id": "#/definitions/missile",
  "type": "object",
  "required": [
    "name",
    "range",
    "attacks",
    "to_hit",
    "to_wound",
    "rend",
    "damage"
  ],
  "properties": {
    "name": {
      "type": "string"
    },
    "range": {
      "type": "string"
    },
    "attacks": {
      "type": "string"
    },
    "to_hit": {
      "type": "string"
    },
    "to_wound": {
      "type": "string"
    },
    "rend": {
      "type": "string"
    },
    "damage": {
      "type": "string"
    }
  }
}
```

### Type

Field     | Type        | Description
--------  | -----       | -----------
name      | `string`    | The name of the weapon
range     | `string`    | The range of the weapon
attacks   | `string`    | The attacks of the weapon
to_hit    | `string`    | The roll requirements to hit with this weapon
to_wound  | `string`    | The roll requirements to wound with this weapon
rend      | `string`    | The list of ranged weapons for this weapon
damage    | `string`    | The damage of this weapon

## Melee Weapon

A unit's close quarters weapon.

```go
package main

// MeleeWeapon is a weapon that is used
// for close quarters combat
type MeleeWeapon struct {
	Name    string `json:"name"`
	Range   string `json:"range"`
	Attacks string `json:"attacks"`
	ToHit   string `json:"to_hit"`
	ToWound string `json:"to_wound"`
	Rend    string `json:"rend"`
	Damage  string `json:"damage"`
}
```

> JSON Schema

```json
{
  "$id": "#/definitions/melee",
  "type": "object",
  "required": [
    "name",
    "range",
    "attacks",
    "to_hit",
    "to_wound",
    "rend",
    "damage"
  ],
  "properties": {
    "name": {
      "type": "string"
    },
    "range": {
      "type": "string"
    },
    "attacks": {
      "type": "string"
    },
    "to_hit": {
      "type": "string"
    },
    "to_wound": {
      "type": "string"
    },
    "rend": {
      "type": "string"
    },
    "damage": {
      "type": "string"
    }
  }
}
```

### Type

Field     | Type        | Description
--------  | -----       | -----------
name      | `string`    | The name of the weapon
range     | `string`    | The range of the weapon
attacks   | `string`    | The attacks of the weapon
to_hit    | `string`    | The roll requirements to hit with this weapon
to_wound  | `string`    | The roll requirements to wound with this weapon
rend      | `string`    | The list of ranged weapons for this weapon
damage    | `string`    | The damage of this weapon
