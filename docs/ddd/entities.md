# Entities

> An object that is not defined by its attributes, but rather by a thread of continuity and its identity.
>
> — https://en.wikipedia.org/wiki/Domain-driven_design#Building_blocks

Domain entities are "[vanilla] PHP objects". To simplify its model definition common _fields_ and _features_ are
provided in the form of [traits].

## Identities

Use identities to simplify the entity its identity definition. Built-in identities are:

- `MsgPhp\Domain\Model\IntIdentity`

## Entity Fields

Use entity fields to provide _read operations_ for common entity fields. Built-in fields are:

- `Msgphp\Domain\Model\CreatedAtField`
- `Msgphp\Domain\Model\LastUpdatedAtField`

## Entity Features

Use entity features to provide _write operations_ for common entity fields. Built-in features are:

- `MsgPhp\Domain\Model\CanBeConfirmed`
- `MsgPhp\Domain\Model\CanBeEnabled`

## Basic Example

```php
<?php

use MsgPhp\Domain\Model\CreatedAtField;
use MsgPhp\Domain\Model\CanBeEnabled;
use MsgPhp\Domain\Model\IntIdentity;

// SETUP

class MyEntity
{
    use IntIdentity;
    use CreatedAtField;
    use CanBeEnabled;

    public function __construct()
    {
        $this->createdAt = new \DateTimeImmutable();
    }
}

// USAGE

$entity = new MyEntity();
$entity->getId()->isNil(); // true

$createdAt = $entity->getCreatedAt();

if (!$entity->isEnabled()) {
    $entity->enable();
}
```

!!! note
    By default `getId()` returns a [`GenericDomainId`](identifiers.md#msgphpdomaingenericdomainid). Since the method is
    derived from a trait [covariance] is possible by default, thus it can be overridden to return any `DomainId` type
    instead.

[vanilla]: https://en.wikipedia.org/wiki/Plain_vanilla
[traits]: https://secure.php.net/traits
[covariance]: https://www.php.net/manual/en/language.oop5.variance.php#language.oop5.variance.covariance
