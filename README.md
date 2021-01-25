# Model Design Syntax

A simple syntax I use for fleshing out model designs on the fly.

## Rules

- All names should use PascalCase for naming, e.g. `ExtendedIdentityResource`.
- Names are case-sensitive
- Names which are acronyms may use all uppercase characters, e.g. `API`, `FBI`, `CRISPR`
- Except where order promotes readability, maintainability, and understanding of the context, models and property descriptions should be placed in alphabetical order, since order of importance is subjective across contexts and people, and models may be used across contexts.
- Lines should not exceed 120 characters, where reasonable
- Nested lines between braces, brackets, or any grouping character must be indented
- Any indentation must use spaces instead of tabs and must be four characters in length
- Braces should go on a new line to help with readability

## Syntax

Anything wrapped in arrow brackets `<>` denotes a variable.

Double forward slashes `//` mark comments.

### Model

```
<name>
{
    // property descriptions
}
```

The `<name>` parameter denotes the model name and should semantically describe the model. Abbreviating the name or using acronyms is discouraged, where reasonable. Commonly used named such as API and Spec which can easily be discerned from the context. Another example of common names would be those used in a finance app such as APY, FIDC, etc.

The brakcets `{}` are used to group property descriptions together under a single model.

Example:

```
User
{
    Name: string (r)
    Enabled: boolean (r) [d:true]
    CreatedOn: date-time (r) (ro)
    UpdatedOn: date-time (r)
    // etc.
}
```

#### Inheritance

Models may inherit from other models and share these derived properties. Inheritance is denoted by a tilde `~`. Only one tilde can be used per model.

While an empty `SUV` model is used below for the sake of example, it is recommended to avoid using empty models unless you plan on adding property descriptions later.

Example:

```
Vehicle
{
    HasLicensePlate: boolean (r) [d:true]
    Height: decimal (r)
    LicensePlateNumber: string (r) (ro)
    NumberOfDoors: integer (r)
}

SUV ~ Vehicle
{
    // ...
}

Car ~ Vehicle
{
    HasSpinningRims: boolean (r) [d:true]
}

Truck ~ Vehicle
{
    BedLength: decimal (r)
    HasCanopy: boolean (r) [d:false]
}
```

A model may inherit from multiple models where property names do not clash. Multiple inheritance is denoted by a comma separated list after the tilde `~`.

```
ChevyElCamino ~ Car, Truck
{
    HasPowerFrontDiskBrakes: boolean (o)
}
```

### Property Descriptions

A property description describes one value among several that are grouped together by a model.

Syntax: `<property_name>: <type> [...(<flag>)] [d:<value>] [v:...<value>]`

#### Flag

Syntax: `(<flag>)`

A flag denotes a certain state for a property or model. E.g., is the property required, is the property read-only, etc.

Example:

```
Car
{
    NumberOfDoors: integer (r) (ro)
}
```

Below a current list of flags. It is by no means definitive:

- (d) - deprecated
- (o) - optional
- (r) - required
- (ro) - read-only

#### Default

Syntax: `[d:<value>]`

Denotes a default value for a property description. Any default value given must match the value type of the property description.

i.e. `IsDeleted: boolean [d:false]`

#### Accepted Values

Syntax: `[v:...<value>]`

Denotes a comma separated list of acceptable values for a property.

i.e. `Direction: string [v:Up,Down,Left,Right]`
