# VdfWebTools

This is a set of helper classes for working with web requests and web content in DataFlex.

## JSON Examples

### Table of Contents
1. [Context](#context)
2. [Parsing an Object](#parsing-object)
3. [Parsing an Array](#parsing-array)
3. [Creating and Serializing a JSON Object](#serializing-object)

### Context <a name="context"></a>
* The `cJSONParser` class helps you parse JSON strings using the `Parse` procedure.
* The `cJSONDictionary` class helps you access, mutate and serialize JSON objects. It depends on [cHashTable](https://github.com/svan-jansson/cHashTable) and [cRegex](https://github.com/svan-jansson/cRegex).

### Parsing an Object <a name="parsing-object"></a>
```dataflex

Procedure ParsingExample
  Handle hParser hDictionary
  String sJSON sValue
  
  Move '{"stringValue": "this is a string"}' to sJSON
  
  Get Create U_cJSONParser to hParser
  Get Create U_cJSONDictionary to hDictionary
  Send Parse of hParser sJSON hDictionary
  If (Found) Begin
    Get Value of hDictionary "stringValue" to sValue // "this is a string"
  End
  
  Send Destroy of hParser
  Send Destroy of hDictionary
End_Procedure

```

### Parsing an Array <a name="parsing-array"></a>

```dataflex
Procedure ParsingExample
  Handle hParser hDictionary
  String sJSON sValue
  Variant[] vaArray
  
  Move '["first", "second", "third"]' to sJSON

  Get Create U_cJSONParser to hParser
  Get Create U_cJSONDictionary to hDictionary
  Send Parse of hParser sJSON hDictionary

  If (Found) Begin
    Get Value of hDictionary "_array" to vaArray // "_array" is a pseudo element for top-level JSON arrays
    Move vaArray[1] to sValue // "second"
  End
  
  Send Destroy of hParser
  Send Destroy of hDictionary
End_Procedure
```

### Creating and Serializing a JSON Object <a name="serializing-object"></a>

Let's recreate this JSON string using DataFlex:

```json
{
	"type": "Horse",
	"properties": {
		"legs": 4,
		"hasStripes": false
	},
	"relatedAnimals": [
		"Zebra",
		"Mule"
	]
}
```

```dataflex
Procedure SerializingExample
  Handle hAnimal hProperties
  String[] saRelatedAnimals
  String sJSON
  
  Move "Zebra" to saRelatedAnimals[0]
  Move "Mule" to saRelatedAnimals[1]
  
  Get Create U_cJSONDictionary to hAnimal
  Get Create U_cJSONDictionary to hProperties
  
  Set TypedValue of hAnimal "type" _jsond_string to "Horse"
  Set TypedValue of hAnimal "properties" _jsond_object to hProperties
  Set TypedValue of hAnimal "relatedAnimals" _jsond_array to saRelatedAnimals
  
  Set TypedValue of hProperties "legs" _jsond_number to 4
  Set TypedValue of hProperties "hasStripes" _jsond_bool to false
  
  Get Serialize of hAnimal to sJSON // { "type": "Horse", ... }
  
  Send Destroy of hAnimal
  Send Destroy of hProperties
End_Procedure
```
