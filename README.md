# Model Design Syntax

A simple syntax I use for fleshing out model designs on the fly.

# Rules

- All names should use PascalCase for naming, e.g. `ExtendedIdentityResource`.
- Names are case-sensitive
- Names which are acronyms may use all uppercase characters, e.g. `API`, `FBI`, `CRISPR`
- Except where order promotes readability, maintainability, and understanding of the context, models and property descriptions should be placed in alphabetical order, since order of importance is subjective across contexts and people, and models may be used across contexts.
- Lines should not exceed 120 characters, where reasonable
- Nested lines between braces, brackets, or any grouping character must be indented
- Any indentation must use spaces instead of tabs and must be four characters in length
- Braces should go on a new line to help with readability

# Syntax

Anything wrapped in arrow brackets `<>` denotes a variable.

Double forward slashes `//` mark comments.

## Model

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

### Inheritance

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

## Property Descriptions

A property description describes one value among several that are grouped together by a model.

Syntax: `<property_name>: <type> ...(<flag>) [<rule>:<value>]`

The `...` denotes multiple items, i.e. an array.

Flags must be surrounded only by parenthesis.

Any rule with a value must be surrounded by square braces and the name of the rule and the rule value separated by a colon.

Flags must always appear before rules with values.

There is no particular order in which flags or rules must be set in, other than what is mentioned above, but consistency is strongly encouraged. We recommend either alphabetical order, or, the following:

- `(r) (ro) (d)` for flags
- `[d:<value>] [min:<number>] [max:<number>] [rel:<Model>(<Property>)] [v:...<value>]` for rules

### Flag

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
- (r) - required
- (ro) - read-only

### Default

Syntax: `[d:<value>]`

Denotes a default value for a property description. Any default value given must match the value type of the property description.

i.e. `IsDeleted: boolean [d:false]`

### Minimum Limit

Syntax: `[min:<number>]`

Denotes the minimum amount required for a value to be acceptable on a property.

- For strings, this is the string length
- For numbers, this is the lowest numeric value
- For arrays, this is the minimum amount of items

i.e. `Name: string [min:1]`

### Maximum Limit

Syntax: `[max:<number>]`

Denotes the maximum amount allowed for a value to be acceptable on a property.

- For strings, this is the string length
- For numbers, this is the highest numeric value
- For arrays, this is the maximum amount of items

i.e. `Name: string [max:50]`

### Relationship

Syntax: `[rel:<Model>(<Property>)]`

Denotes a relationship that ties the property of one model to the property of another model. The property types must match.

i.e.

```
Library
{
    Id: integer (r)
    Name: string (r)
    OpeningTime: time (r) [d:07:00]
    ClosingTime: time (r) [d:19:00]
}

LibraryBook
{
    Id: integer (r)
    LibraryId: integer (r) [rel:Library(Id)]
    Title: string (r)
    PublishingDate: date (r)
}
```

### Accepted Values

Syntax: `[v:...<value>]`

Denotes a comma separated list of acceptable values for a property.

i.e. `Direction: string [v:Up,Down,Left,Right]`
