# envterpolate
Tiny Dotenv Environment Variable Interpolator

[![styled with prettier](https://img.shields.io/badge/styled_with-prettier-ff69b4.svg)](https://github.com/prettier/prettier)
[![Greenkeeper badge](https://badges.greenkeeper.io/codekiln/envterpolate.svg)](https://greenkeeper.io/)
[![Travis](https://img.shields.io/travis/codekiln/envterpolate.svg)](https://travis-ci.org/codekiln/envterpolate)
[![Coverage Status](https://coveralls.io/repos/github/codekiln/envterpolate/badge.svg?branch=master)](https://coveralls.io/github/codekiln/envterpolate?branch=master)

This package implements a simple idea: interpolate strings taken from
your npm package users, using variables they defined in their dotenv files. 
This is useful for those "last mile" customization that can't be committed 
to the repository (database parameters, passwords, etc)

Given two things:

1. File: `.env`, written in [dotenv syntax](https://www.npmjs.com/package/dotenv#rules),
containing a [really important password](http://therumpus.net/2010/01/conversations-about-the-internet-5-anonymous-facebook-employee/?all=1),
NOT committed to repo and added to `.gitignore`:
```
MY_SECRET_PASSWORD=(|-|U(|< |\|0rr15
```

2. File: `package.json`, with strings containing variables defined in `.env`: 
```js
packageJson = {
  // ...
  "yourPluginConfig": {
    cliArgs: "--save --password ${MY_SECRET_PASSWORD}"
  }
  // ...
}
```

... Then you can use `envterpolate` to read the file with the dotenv environment variables expanded:
```js
import {interpolateJson} from 'envterpolate';

const packageJson = interpolateJson('package.json', '.env')
console.log(packageJson)
/**
{
  // ...
  "yourPluginConfig": {
    cliArgs: "--save --password (|-|U(|< |\|0rr15"
  }
  // ...
}
**/
```
