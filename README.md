# passfather
[![version](https://img.shields.io/npm/v/passfather.svg?style=flat-square)](https://www.npmjs.com/package/passfather)
[![license](https://img.shields.io/github/license/vyushin/passfather.svg?style=flat-square)](https://github.com/vyushin/passfather/blob/master/LICENSE)

**passfather** is very fast and powerful utility with zero dependencies to generate strong password or random string.

## Table of contents
* [Features](#features)
* [Installation](#installation)
* [Example](#example)
* [Options](#options)
* [Contributing](#contributing)
* [Licence](#license)

## Features

* Support browsers and Node.js;
* Multiple random number algorithmes such as Alea, KISS07, Kybos, LFib, LFIB4, MRG32k3a, Xorshift03.
By default using [getRandomValues](https://developer.mozilla.org/ru/docs/Web/API/RandomSource/getRandomValues) for browsers and [getRandomBytes](https://nodejs.org/api/crypto.html#crypto_crypto_randombytes_size_callback) for Node.js;
* Support seed with entropy;
* Optional using any of unicode chars (ranges). By default there are uppercase, lowercase, numbers and about ten symbols;
* Any length;

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

It's very easy! Just import **passfather** and run it.

```javascript
import passfather from 'passfather';
const password = passfather();
console.log(password); // Output "9g'Jta75Gl3w"
```

By default passfather doesn't require any options. But it's possible to pass options object to
customize password.

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

**NOTE:** if option object is passed then it merges with default options object.

## Options

|Name|Type|Default|Description
|---|---|---|---
|numbers|boolean|`true`|Enable/disable numbers
|uppercase|boolean|`true`|Enable/disable uppercase
|lowercase|boolean|`true`|Enable/disable lowercase
|symbols|boolean|`true`|Enable/disable symbols
|length|integer|`12`|Final string length
|prng|string|`default`|[Pseudorandom Number Generator](https://en.wikipedia.org/wiki/Pseudorandom_number_generator). Generating random string algorithm is using random numbers that generated by prng. Might be: default, Alea, KISS07, Kybos, LFib, LFIB4, MRG32k3a, Xorshift03.
|seed|array|[seed.js](https://github.com/vyushin/passfather/blob/master/src/seed.js)|Seed for prng. See [random seed](https://en.wikipedia.org/wiki/Random_seed). NOTE: Default value doesn't have enough entropy. Please, using your own values.
|ranges|array|`null`|UTF-8 char ranges that will using to generate random string. See below.

### Options: `ranges` (custom chars)

Passfather can make password containing custom chars.<br/>
It's possible via ranges option.

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
    [[9800, 9807], [9818, 9823]], // Group of char range. Zodiac signs, chess figures.
    [[9698, 9701], [9606, 9611]], // Group of char range. Geometric figures
  ],
});
console.log(password); // Output "▋▆♟◥◢♎◥♚♞♚▆♚◥▆▉♝"
```

The ranges option is array of UTF-8 char ranges.<br/>
You can find all of them on [unicode table](https://unicode-table.com/ru/#box-drawing)

Example above contains UTF-8 chars with codes from 9800 to 9807 and from 9818 to 9823 (zodiac signs and chess figures).<br/>
The example also contains UTF-8 chars with codes from 9698 to 9701 and from 9606 to 9611 (geometric figures).

This means that password will **necessarily** contain **one or more** chess figures **or** zodiac signs **and** one or more geometric figures.<br/>
But it doesn't mean that password will contain zodiac signs **and** chess figures because they are part of one range.<br/>
If you want make password with zodiac signs **and** chess figures you should move chess figures to new range.

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
    [[9800, 9807]], // Group of char range. Zodiac signs
    [[9818, 9823]], // Group of char range. Chess figures.
    [[9698, 9701], [9606, 9611]], // Group of char range. Geometric figures
  ],
});
console.log(password); // Output "♏◣♛◥♚♟♚♝♌▆♌♚♞▉♞♞"
```

Making new range you get guarantee that one of char from range will be part of password.

Ranges may using together with number, uppercase, lowercase or symbols option.

For example:

```javascript
import passfather from 'passfather';
const password = passfather({
  // number, uppercase, lowercase or symbols enabled by default 
  // so we just don't pass them.
  length: 16,
  ranges: [ 
    [[9800, 9807]],
    [[9818, 9823]],
    [[9698, 9701], [9606, 9611]],
  ],
});
console.log(password); // Output "♚!N◢♊q6DO1,3▉♌k5♞"
```

## Contributing

See [contributing](https://github.com/vyushin/passfather/blob/master/CONTRIBUTING.md) guideline.

## License
[MIT LICENSE](https://github.com/vyushin/passfather/blob/master/LICENSE)
