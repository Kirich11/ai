# PHP

- Always use strict typing at the head of a `.php` file: `declare(strict_types=1);`.
- Always use curly braces for control structures, even for single-line bodies.

## Constructors

- Use PHP 8 constructor property promotion in `__construct()`.
    - `public function __construct(public GitHub $github) { }`
- Do not allow empty `__construct()` methods with zero parameters unless the constructor is private.

## Type Declarations

- Always use explicit return type declarations for methods and functions.
- Use appropriate PHP type hints for method parameters.

```php
protected function isAccessible(User $user, ?string $path = null): bool
{
    ...
}
```

## Enums

- Typically, keys in an Enum should be upper snake case. For example: `FAVORITE_PERSON`, `BEST_LAKE`, `MONTHLY`.

## Comments

- Prefer PHPDoc blocks over inline comments. Never use comments within the code itself unless the logic is exceptionally complex.

## PHPDoc Blocks

- Use phpstan array shapes to document arrays
- Add useful array shape type definitions when appropriate.
- When an array shape is used in several places, define it as a local phpstan type
- When an array shape is used across files, define it in the owner class as a custom phpstan type and import it into where it is needed
- If an array shape is needed across module boundaries, convert it into a DTO
