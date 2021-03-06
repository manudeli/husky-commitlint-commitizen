# πΌ Conventional Commit λ°ν μμ (μλλ₯Ό ν΄λ¦­νμΈμ)
[![Conventional Commit λ§€μ¬μ λ°ν](http://img.youtube.com/vi/l4bRepOMXs4/0.jpg)](https://youtu.be/l4bRepOMXs4)

# Conventional Commit κ°μ νκΈ°
## [π Conventional Commit](https://www.conventionalcommits.org/en/v1.0.0/)μ΄λ?
```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```
Conventional Commitsμ κ°λ²Όμ΄ μ»€λ° μ»¨λ²€μμλλ€. μ νν μ»€λ° νμ€ν λ¦¬λ₯Ό λ§λλλ° κ°μ₯ μ¬μ΄ λ£°μλλ€. μ λͺν μλν ν΄μ μ΄ μ»¨λ²€μμΌλ‘ μ¬μ©νκΈ° μ½κ² ν΄μ€λλ€. μ΄ μ»¨λ²€μμ features, fixes, breaking changesκ° μ»€λ°λ©μμ§λ‘ λ§λλ SemVer(Semantic Versionμ μ€μλ§)κ³Ό μ λ§μ΅λλ€.

> The Conventional Commits specification is a lightweight convention on top of commit messages. It provides an easy set of rules for creating an explicit commit history; which makes it easier to write automated tools on top of. This convention dovetails with SemVer, by describing the features, fixes, and breaking changes made in commit messages.

## Semantic Versioning
```
MAJOR.MINOR.PATCH λ²μ  λ²νΈκ° μ£Όμ΄μ§λ©΄ λ€μμ μ¦κ°μν΅λλ€.
1. νΈνλμ§ μλ API λ³κ²½ μ MAJOR λ²μ ,
2. μ΄μ  λ²μ κ³Ό νΈνλλ λ°©μμΌλ‘ κΈ°λ₯μ μΆκ°νλ κ²½μ° MINOR λ²μ  λ°
3. μ΄μ  λ²μ κ³Ό νΈνλλ λ²κ·Έ μμ μ ν  λ PATCH λ²μ .
μνν λ° λΉλ λ©νλ°μ΄ν°μ λν μΆκ° λ μ΄λΈμ MAJOR.MINOR.PATCH νμμ νμ₯μΌλ‘ μ¬μ©ν  μ μμ΅λλ€.
```
### μ [π Semantic Versioning](https://semver.org/)μ΄ νμνλ?
μννΈμ¨μ΄ κ΄λ¦¬μ μΈκ³μλ μμ‘΄μ± μ§μ₯μ΄λΌ λΆλ¦¬λ λλ €μμ΄ μμ΅λλ€. μ μ  λ λ§μ ν¨ν€μ§λ€μ μ°λ¦¬μ μμ€νμ ν¨κ» μ¬μ©ν  μλ‘ μΈμ  κ°λ μ λ§μ κ΅¬λ©μ΄μ λΉ μ§κ² λ  κ°λ₯μ±μ΄ μ»€μ§λλ€. (μμΈν λ΄μ©μ λ§ν¬μμ)

>In the world of software management there exists a dreaded place called βdependency hell.β The bigger your system grows and the more packages you integrate into your software, the more likely you are to find yourself, one day, in this pit of despair.

# `husky + commitlint + commitizen` μ€μ μ ν΅ν Conventional Commit κ°μ 

## husky
git-hookμ μΈλΆμμ μ¬μ©ν  μ μλλ‘ λμμ£Όλ λΌμ΄λΈλ¬λ¦¬

### μ λͺν μ¬μ©μ
> webpack/webpack
angular/angular
angular/angular-cli
angular/components
vercel/hyper
blitz-js/blitz
facebook/docusaurus
typescript-eslint/typescript-eslint
11ty/eleventy
stylelint/stylelint
rollup/rollup
tauri-apps/tauri
NativeScript/NativeScript
formatjs/formatjs
react-bootstrap/react-bootstrap
react-dnd/react-dnd
react-grid-layout/react-grid-layout
snabbdom/snabbdom
logaretm/vee-validate
zenorocha/clipboard.js
NodeBB/NodeBB
ant-design/ant-design
And more

## commitlint
huskyμ ν¨κ» μ¬μ©ν΄ commit messageμ μ ν¨μ±κ²μ¬λ₯Ό νλ λΌμ΄λΈλ¬λ¦¬

## commitizen
commit messageλ₯Ό μ ν΄μ§ νμμ λ§μΆ°μ μλ ₯νλλ‘ μ νμ§λ₯Ό λ³΄μ¬μ£Όλ λΌμ΄λΈλ¬λ¦¬

## 1. husky + commitlint λ‘ Conventional Commitλ§ μ»€λ°λ©μμ§λ‘ κ°μ νκΈ°

### a. μμ‘΄μ± μΆκ°
```
npm i -D husky @commitlint/cli @commitlint/config-conventional
```

### b. `package.json` Life Cycle scripts μ€ `postinstall`μ μΆκ°ν `npm i`
λ€λ₯Έ ν΄λ‘  λ°μ κ°λ°μκ° huskyλ₯Ό λ€μ΄λ‘λ λ°μ μν©μμ μ¬μ©ν  μ μλλ‘ μΆκ°
```json
"script": {
  "postinstall": "husky install"
}
```

### c. rootμ `commitlint.config.ts`λ₯Ό μΆκ°
```typescript
import type { UserConfig } from '@commitlint/types'

const Configuration: UserConfig = {
  extends: ['@commitlint/config-conventional'],
}

module.exports = Configuration

```

### d. .husky/commit-msg μΆκ°
```shell
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npx --no-install commitlint --edit "$1"
```

## 2. commitizenμΌλ‘ μ»€λ° λ©μμ§ μ νμ§λ₯Ό μ½κ² μ ννλλ‘ νκΈ°

### a. `package.json` scripts μ `commit` μΆκ° (μμ‘΄μ± μ€μΉνμ§ μκ³  μ¬μ©νκΈ°)
```json
"scripts": {
  "commit": "npx cz-customizable"
}
```

### b. rootμ `.cz-config.js`λ₯Ό μΆκ° ν μνλ typeEnumμΌλ‘ λμ€λλ‘ μ€μ 
```javascript
const typeEnums = {
  feat: 'μλ‘μ΄ κΈ°λ₯ μΆκ° μ',
  fix: 'λ²κ·Έ ν΄κ²° / μμ  μ',
  docs: 'λνλ©νμ΄μ μΆκ° λλ μμ  μ',
  markup: 'λ§ν¬μ μΆκ°μ',
  revert: 'μμ λλλ¦¬κΈ° μ',
  style: 'μ€νμΌ μΆκ°μ',
  remove: 'λΆνμν νμΌ μ κ±° μ',
  perf: 'μ±λ₯ κ°μ  μ',
  ci: 'CI κ΅¬μ± νμΌ λ° μ€ν¬λ¦½νΈ λ³κ²½ μ',
  event: 'μ΄λ²€νΈ μΆκ°μ',
  config: 'μ€μ νμΌ λ³κ²½ / μΆκ°μ',
}

const maxSpaceLength = Object.keys(typeEnums).reduce(
  (acc, { length }) => (length > acc ? length : acc),
  0,
)

const commitizenConfig = {
  types: Object.entries(typeEnums).map(([type, description]) => ({
    value: type,
    name: `${type}:     ${' '.repeat(maxSpaceLength - type.length)}` + description,
  })),
}

module.exports = commitizenConfig
```

### c. μμΈν `.cz-config.js` μ€μ νκΈ°
[π μ€μ νκΈ° μ°Έκ³  λ§ν¬](https://github.com/leoforfree/cz-customizable#options)


## 3. commitlint - commitizen λ£° λκΈ°ν

### a. commitlintλ£°μ΄ commitizenλ£°μ typeEnumμ λ°λΌλ³΄λλ‘ ν΄μ λκΈ°ν
```typescript
import type { UserConfig } from '@commitlint/types'
import commitizenConfig from './.cz-config.js'

const Configuration: UserConfig = {
  extends: ['@commitlint/config-conventional'],
  rules: {
    'type-enum': [2, 'always', commitizenConfig.types.map(({ value }) => value)],
    'type-case': [2, 'always', 'lower-case'],
    'scope-empty': [2, 'never'],
  },
}

module.exports = Configuration

```

# λ μμλ³΄λ©΄ μ’μ κ²λ€
## GitHub Actions + [πsemantic-release](https://semantic-release.gitbook.io/semantic-release/) + CI
- μνλ λΈλμΉμ νΈμλ°μ κ²½μ° github-actionsμμ semantic-releaseκ° μ€ν
- semantic-releaseκ° semantic-versioningμ λ°λΌ major/minor/patchμ€ μ΄λ€ λ²μ μ μ¬λ €μΌ ν μ§ commit λ©μμ§(conventional-commitμ νμ)λ₯Ό λ³΄κ³  μλμΌλ‘ νκ·Έλ₯Ό νΈμν΄μ€λλ€.
- μ΄ λ²μ  νκ·Έλ₯Ό λ³΄κ³  CI - CDλ₯Ό μ§ννκ² ν©λλ€.

## Semantic Release μμ λ ν¬
- π[manudeli/semantic-release-test](https://github.com/manudeli/semantic-release-test/releases)
