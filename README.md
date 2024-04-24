# passfather
[![version](https://img.shields.io/npm/v/passfather.svg?style=flat-square)](https://www.npmjs.com/package/passfather)
[![npm downloads](https://img.shields.io/npm/dw/passfather.svg?style=flat-square)](https://www.npmjs.com/package/passfather)
[![license](https://img.shields.io/github/license/vyushin/passfather.svg?style=flat-square)](https://github.com/vyushin/passfather/blob/master/LICENSE)

**passfather** is a very fast and powerful utility with zero dependencies, designed to generate strong passwords or random strings.

> **passfather is free** and will always remain free <br/>
> A simple and quick way to support the project is to **buy me a coffee**. <br/>It will take no more than 5 minutes and will allow the project to keep going

<a href="https://buymeacoffee.com/vyushin" target="_blank" title="Buy me a coffee">
  <img height="50" alt="Buy me a coffee" src="https://github.com/vyushin/passfather/assets/8006957/3e894205-6dd9-47da-b547-5ea09353a7dd">
</a>

## Table of contents
* [Features](#features)
* [Installation](#installation)
* [Example](#example)
* [Options](#options)
* [Contributing](#contributing)
* [Licence](#license)

## Features

* Supports both browsers and Node.js.;
* Offers multiple random number algorithms such as Alea, KISS07, Kybos, LFib, LFIB4, MRG32k3a, Xorshift03.
By default, it uses [getRandomValues](https://developer.mozilla.org/ru/docs/Web/API/RandomSource/getRandomValues) for browsers and [getRandomBytes](https://nodejs.org/api/crypto.html#crypto_crypto_randombytes_size_callback) for Node.js;
* Supports seeding with entropy;
* Optionally utilizes any Unicode characters;
* Allows for any password length.

## Installation

###### NPM
`npm install --save passfather`

###### Yarn
`yarn add passfather`

###### ESM
```html
<script type="module">
    import passfather from 'https://unpkg.com/passfather@latest/dist/passfather.min.mjs'
    console.log( passfather() ); // Output "vFR_@1hDMhAr"
</script>
```

###### UMD

```html
<script src="https://unpkg.com/passfather@latest/dist/passfather.min.js"></script>
<script>
    console.log( passfather() ); // Output "r_@1hDvFRMhA"
</script>
```

## Example

It's very easy! Just import **passfather** and run the function.

```javascript
import passfather from 'passfather';
const password = passfather();
console.log(password); // Output "9g'Jta75Gl3w"
```

By default, passfather does not require any options. However, it is possible to pass an options object to customize the password.

```javascript
import passfather from 'passfather';
const password = passfather({
  numbers: true,
  uppercase: true,
  lowercase: true,
  symbols: false, // Disable symbols
  length: 16,
});
console.log(password); // Output "40rAe2hqiM0UzTmN"
```

**NOTE:** an options object is passed, it merges with the default options object.

## Options

|Name|Type|Default|Description
|---|---|---|---
|numbers|boolean|`true`|Enable/disable numbers
|uppercase|boolean|`true`|Enable/disable uppercase
|lowercase|boolean|`true`|Enable/disable lowercase
|symbols|boolean|`true`|Enable/disable symbols
|length|integer|`12`|Final string length
|prng|string|`default`| The algorithm for generating random strings uses random numbers generated by a [pseudorandom number generator]((https://en.wikipedia.org/wiki/Pseudorandom_number_generator)) (PRNG). Options include default, Alea, KISS07, Kybos, LFib, LFIB4, MRG32k3a, and Xorshift03. By default, it uses [getRandomValues](https://developer.mozilla.org/ru/docs/Web/API/RandomSource/getRandomValues) for browsers and [getRandomBytes](https://nodejs.org/api/crypto.html#crypto_crypto_randombytes_size_callback) for Node.js;
|seed|array|[seed.js](https://github.com/vyushin/passfather/blob/master/src/seed.js)|Seed for the PRNG. See [random seed](https://en.wikipedia.org/wiki/Random_seed) for details. NOTE: The default value may not have sufficient entropy. It is recommended to use your own values for better security.
|ranges|array|`null`|UTF-8 character ranges that will be used to generate the random string. See below for details.

### Options: `ranges` (custom characters)

Passfather can create passwords containing custom characters through the `ranges` option.

For example:

```javascript
import passfather from 'passfather';
const password = passfather({
  numbers: false,
  uppercase: false,
  lowercase: false,
  symbols: false,
  length: 16,
  ranges: [ 
    [[9800, 9807], [9818, 9823]], // Group of character ranges including zodiac signs and chess figures
    [[9698, 9701], [0x2586, 0x258B]], // Geometric figures
  ],
});
console.log(password); // Output "▋▆♟◥◢♎◥♚♞♚▆♚◥▆▉♝"
```

The `ranges` option is an array of UTF-8 character ranges<br/>
> Passfather supports a range from 0 to 65535 in the decimal system, and from 0x0000 to 0xFFFF in the hexadecimal system.<br/>
You can find all of them on the [unicode table](https://unicode-table.com/unicode-table)

The example above includes UTF-8 characters with codes ranging from 9800 to 9807 and from 9818 to 9823, which represent zodiac signs and chess figures.<br/>
It also includes characters with codes from 9698 to 9701 and from 0x2586 to 0x258B, representing geometric figures.

This implies that the password will **necessarily** contain **one or more** chess figures **or** zodiac signs **and** one or more geometric figures.<br/>
However, it does not mean that the password will contain both zodiac signs and chess figures, as they are part of the same range.<br/>
If you want to create a password that includes **both** zodiac signs **and** chess figures, you should move the chess figures to a new range.

For example:

```javascript
import passfather from 'passfather';
const password = passfather({
  numbers: false,
  uppercase: false,
  lowercase: false,
  symbols: false,
  length: 16,
  ranges: [ 
    [[9800, 9807]], // Group of character ranges including zodiac signs
    [[9818, 9823]], // Chess figures.
    [[9698, 9701], [0x2586, 0x258B]], // Geometric figures
  ],
});
console.log(password); // Output "♏◣♛◥♚♟♚♝♌▆♌♚♞▉♞♞"
```

By creating a new range, you ensure that at least one character from that range will be part of the password.

Ranges can be used in conjunction with the number, uppercase, lowercase, or symbols options.

For example:

```javascript
import passfather from 'passfather';
const password = passfather({
  // Number, uppercase, lowercase, and symbols are enabled by default, 
  // so there is no need to pass them separately
  length: 16,
  ranges: [ 
    [[9800, 9807]],
    [[9818, 9823]],
    [[9698, 9701], [0x2586, 0x258B]],
  ],
});
console.log(password); // Output "♚!N◢♊q6DO1,3▉♌k5♞"
```

## Contributing

See [contributing](https://github.com/vyushin/passfather/blob/master/CONTRIBUTING.md) guideline.

## License
[MIT LICENSE](https://github.com/vyushin/passfather/blob/master/LICENSE)
