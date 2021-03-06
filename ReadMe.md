# CPF

Basic functions for handling [CPF](https://en.wikipedia.org/wiki/Cadastro_de_Pessoas_F%C3%ADsicas) numbers.

> Cadastro de Pessoas Físicas (CPF) – Portuguese for "Natural Persons Register" – is the Brazilian individual taxpayer registry identification, a number attributed by the Department of Federal Revenue of Brazil.



# Installation

Using [NPM](http://npmjs.com/):

```
$ npm install cpf --save
```

# <a name="usage"></a>Usage
Although the CPF is composed of numbers, it is good practice to treat it as a string to preserve any leading `0`s.

## TL;DR
```js
var CPF = require('cpf');

CPF.format('11144477735'); //=> '111.444.777-35'
CPF.validate('111.444.777-35'); //=> true
CPF.generate(); //=> formatted string with 14 character
```

## Formatting
The `format()` function supports incomplete values.

```js
var CPF = require('cpf');

CPF.format('111444'); //=> '111.444'
CPF.format('1114447'); //=> '111.444.7'
CPF.format('11144477'); //=> '111.444.77'
CPF.format('111444777'); //=> '111.444.777'
CPF.format('1114447773'); //=> '111.444.777-3'
CPF.format('11144477735'); //=> '111.444.777-35'
CPF.format('111-444-777-35'); //=> '111.444.777-35'
```

This allows formatting the content of a text field directly, with on the fly formatting.
```js
CPF.format('111'); //=> '111'
CPF.format('1114'); //=> '111.4'
CPF.format('111.4'); //=> '111.4'
CPF.format('111.'); //=> '111'
CPF.format('1114'); //=> '111.4'
```

It ignores any non-digit, filling the separators as they are needed.
```js
var CPF = require('cpf');

CPF.format('my cpf'); //=> ''
CPF.format('111.'); //=> '111'
CPF.format('111 4'); //=> '111.4'
CPF.format('111-444-777-35'); //=> '111.444.777-35'
```

It also clamps the string to 11 digits (14 with separators).
```js
var CPF = require('cpf');

CPF.format('11144477735'); //=> '111.444.777-35'
CPF.format('111444777355'); //=> '111.444.777-35'
CPF.format('111.444.777-355'); //=> '111.444.777-35'
```

## <a name="validation"></a>Validation
Checks if the hash of the first 9 digits match the last 2 digits. It takes either an 11-digit string or a formatted 14-character string.

```js
var CPF = require('cpf');

CPF.validate('11144477735'); //=> true
CPF.validate('111.444.777-35'); //=> true

//incomplete
CPF.validate('111.444.777-3'); //=> false

//invalid hash
CPF.validate('111.444.777-34'); //=> false

//too many digits
CPF.validate('111.444.777-355'); //=> false
```

<a name="semi-valid"></a>
Note that this returns `true` for the values bellow. Useful when testing manually.

- 000.000.000-00
- 111.111.111-11
- 222.222.222-22
- 333.333.333-33
- 444.444.444-44
- 555.555.555-55
- 666.666.666-66
- 777.777.777-77
- 888.888.888-88
- 999.999.999-99

## Generation
Generates a valid, formatted CPF for testing purposes.

```js
var CPF = require('cpf');

CPF.validate(CPF.generate()); //=> true

CPF.generate().length; //=> 14
/^\d{3}\.\d{3}\.\d{3}-\d{2}$/.test(CPF.generate()); //=> true
```

# Future work
Some ideas of extensions to this library, for anyone willing to contribute.

- [ ] Deprecate the functions in Portuguese.
- [ ] Automated testing (see examples in [Usage](#usage)).
- Custom/optional behaviour
  - [ ] Unformatted CPF generation
  - [ ] Return `false` for [some CPFs](#validation) even with a valid hash.

# License

[MIT](http://theuves.mit-license.org/)
