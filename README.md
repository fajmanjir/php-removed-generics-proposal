# PHP Generics Proposal - Removed Generics

## Motivation

## Introduction

This document proposes generic types definition with implementation using removed generics. The purpose of removed generics is to enable specification of exact data types, which can be used by static analysis tools. Removed generics, by its definition, must not affect behavior of php interpreter. 

## Main Concepts

The main concepts of this proposal are that:

- all syntax extensions must not change behavior of interpreted code
- all syntax extensions must be easily removed by parser

## TODO

Generics can specify the following types:

- return type of function or method
- parameter type of function or method
- class property type

- array item type specification


## Basic function using generics

This section shows function or method definition with extended input and return types.

### Parameter type matching return type

Since all syntax extensions must be easily removed, the return type declaration can use only _native types_. Otherwise, the parser would need to distinguish between existing type and generic type, which is difficult to implement.

```
// generic return type cannot be used
function getClone<T>(): T
```

Instead, `mixed<T>` type with _generic usage block_ can be used as a return type.

Input parameter using _native type_:

```
function getClone<T>(mixed<T> $source): mixed<T>
{
    return clone $source;
}
```

Input parameter omitting _native type_:

```
function getClone<T>(<T> $source): mixed<T>
{
    return clone $source;
}
```



### Collection example

```
function createCollection<T>(): Collection<T>
```

## Basic class using generics

Example class definition and usage:

```
class Collection<T>
{
    private array<T> $collection = <T>[];
    
    public function get<T> all(): array<T>
    {
        return $this->collection;
    }
    
    public function append(<T> $item): void
    {
        $this->collection[] = $item;
    }
}

$collection = new Collection<int>();
$collection->append(3);

$result = $collection->all();
var_dump($result);
```

## Arrays

### Array item type definitions

The following definitions can be used as parameter type, return type, class generic type variable, trait, interface. (What about enum?)

Array without specified index type using native type:

```
array<int>
```

Array without specified index type using generic type variable:

```
array<T>
```

The array form with a type ending with `[]`, for example `int[]` must not be used since it does not 

Array without specified index type using native type:

### Instancing array using generics

We can instance an array with defined item type using generics block before array instantiation, for example with integer item type:

```
$a = <int>[];
$b = array<int>()
```

This array will match type `array<int>`

TODO: 

## Definitions

- _native type_ - data type which can be used as native type in parameter type, return type or property type definition in PHP 8.2 and later
