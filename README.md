# Unity SerializeReference Property Drawer

![color:ff69b4](https://img.shields.io/badge/licence-MIT-blue)
![color:ff69b4](https://img.shields.io/badge/Unity-2019.3.x-red)

This package provides the ability to edit the type of fields marked with the SerializeReference attribute in Unity editor.

## Installation

Installation as a unity module via a git link in PackageManager or direct editing of `Packages/manifest' is supported.json:
```
"com.elmortem.serializereferenceeditor": "https://github.com/elmortem/serializereferenceeditor.git",
```

## Main types
### SRAttribute
Use it to mark the fields that you want to edit. Important note - they should also be marked with the Serializedreference attribute. You can mark the base type (including the interface) with it.
Allows you to change an array, a list, and a single element. Displays the current field type.

#### Example:
```
[SerializeReference]
[SR]
public List<AbstractData> DataList = new List<AbstractData>();
```
#### Result:
<img src="Images/Demo.gif" width="400">

#### Additional features
You can override SRAttribute and implement a rule for processing instantiated objects.

You can see an example in [SRDemoAttribute.cs](serializereferenceeditor%2FDemo%2FSRDemoAttribute.cs), где был переопределён метод `OnCreate`:
```
public override void OnCreate(object instance)
{
    if(instance is AbstractData)
    {
        ((AbstractData)instance).DataName = instance.GetType().Name;
    }
}
```

### SRNameAttribute
Mark classes with them if you want to customize the display name and nesting hierarchy in the search tree for a specific type.

Example [FloatData.cs](serializereferenceeditor%2FDemo%2FFloatData.cs):
```
[SRName("Data/Simple types/Float")]  
public class FloatData : AbstractData
{
    [Range(0f, 1f)]
    public float Float;
}
```

## Tools
### MissingTypesValidator
You can use the built-in tool to search for lost types.
(this error occurs if the serialized data stores information about a type that no longer exists and could have been deleted during development). To start, you can run
`Tools -> SREditor -> Check MissingTypes`

> At least one `SRMissingTypesValidatorConfig` is required to work.
If necessary, you can implement your own `IAssetMissingTypeReport` for error
reporting if you use it in CI/CD systems.
You can also implement your own `IAssetsLoader` if the default `LoadAllScriptableObjects` is not suitable for you.

Support Unity 2019.3 or later.

Use for free.

Enjoy!
