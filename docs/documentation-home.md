The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).

## Abstract
World is inherently more complex than we thought. Someone can't create a package without following minimal rules. Neither update the package without this rules. So I've created this small standard for my own libraries in order to maintain the good tidy.

## Basic documentation
My own logic implies that all my development model MUST follow [PSR-2](https://www.php-fig.org/psr/psr-2/) to keep clean code. User MAY use [PHPCS](https://github.com/squizlabs/PHP_CodeSniffer) to keep following PSR-2 recommendations.

### Library naming standard
* The name of the package MUST be `alrik11es/package`
* The package name MUST use `kebab-case` as defined in [Special case styles (Wikipedia)](https://en.wikipedia.org/wiki/Letter_case#Special_case_styles) for example `alrik11es/my-lib`
* Namespace name in libraries MUST start with `\Alr` for example `\Alr\Mylib\Core`

### Folders distribution
* Source code SHOULD be in a folder called `/src` but can be defined in composer.json
* Tests folder MUST be called `/tests`
* Tests folder MAY contain subfolders for integration, unit and functional tests
