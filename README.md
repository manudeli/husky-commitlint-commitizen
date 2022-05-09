# Conventional Commit 강제하기
## [🔗 Conventional Commit](https://www.conventionalcommits.org/en/v1.0.0/)이란?
```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```
Conventional Commits은 가벼운 커밋 컨벤션입니다. 정확한 커밋 히스토리를 만드는데 가장 쉬운 룰입니다. 유명한 자동화 툴을 이 컨벤션으로 사용하기 쉽게 해줍니다. 이 컨벤션은 features, fixes, breaking changes가 커밋메시지로 만드는 SemVer(Semantic Version의 줄임말)과 잘 맞습니다.

> The Conventional Commits specification is a lightweight convention on top of commit messages. It provides an easy set of rules for creating an explicit commit history; which makes it easier to write automated tools on top of. This convention dovetails with SemVer, by describing the features, fixes, and breaking changes made in commit messages.

## Semantic Versioning
```
MAJOR.MINOR.PATCH 버전 번호가 주어지면 다음을 증가시킵니다.
1. 호환되지 않는 API 변경 시 MAJOR 버전,
2. 이전 버전과 호환되는 방식으로 기능을 추가하는 경우 MINOR 버전 및
3. 이전 버전과 호환되는 버그 수정을 할 때 PATCH 버전.
시험판 및 빌드 메타데이터에 대한 추가 레이블은 MAJOR.MINOR.PATCH 형식의 확장으로 사용할 수 있습니다.
```
### 왜 [🔗 Semantic Versioning](https://semver.org/)이 필요했나?
소프트웨어 관리의 세계에는 의존성 지옥이라 불리는 두려움이 있습니다. 점점 더 많은 패키지들을 우리의 시스템에 함께 사용할 수록 언젠가는 절망의 구덩이에 빠지게 될 가능성이 커집니다. (자세한 내용은 링크에서)

>In the world of software management there exists a dreaded place called “dependency hell.” The bigger your system grows and the more packages you integrate into your software, the more likely you are to find yourself, one day, in this pit of despair.

# `husky + commitlint + commitizen` 설정을 통한 Conventional Commit 강제

## husky
git-hook을 외부에서 사용할 수 있도록 도와주는 라이브러리

### 유명한 사용자
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
husky와 함께 사용해 commit message의 유효성검사를 하는 라이브러리

## commitizen
commit message를 정해진 형식에 맞춰서 입력하도록 선택지를 보여주는 라이브러리

## 1. husky + commitlint 로 Conventional Commit만 커밋메시지로 강제하기

### a. 의존성 추가
```
npm i -D husky @commitlint/cli @commitlint/config-conventional
```

### b. `package.json` Life Cycle scripts 중 `postinstall`에 추가후 `npm i`
다른 클론 받은 개발자가 husky를 다운로드 받은 상황에서 사용할 수 있도록 추가
```json
"script": {
  "postinstall": "husky install"
}
```

### c. root에 `commitlint.config.ts`를 추가
```typescript
import type { UserConfig } from '@commitlint/types'

const Configuration: UserConfig = {
  extends: ['@commitlint/config-conventional'],
}

module.exports = Configuration

```

### d. .husky/commit-msg 추가
```shell
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npx --no-install commitlint --edit "$1"
```

## 2. commitizen으로 커밋 메시지 선택지를 쉽게 선택하도록 하기

### a. `package.json` scripts 에 `commit` 추가 (의존성 설치하지 않고 사용하기)
```json
"scripts": {
  "commit": "npx cz-customizable"
}
```

### b. root에 `.cz-config.js`를 추가 후 원하는 typeEnum으로 나오도록 설정
```javascript
const typeEnums = {
  feat: '새로운 기능 추가 시',
  fix: '버그 해결 / 수정 시',
  docs: '도큐멘테이션 추가 또는 수정 시',
  markup: '마크업 추가시',
  revert: '작업 되돌리기 시',
  style: '스타일 추가시',
  remove: '불필요한 파일 제거 시',
  perf: '성능 개선 시',
  ci: 'CI 구성 파일 및 스크립트 변경 시',
  event: '이벤트 추가시',
  config: '설정파일 변경 / 추가시',
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

### c. 자세한 `.cz-config.js` 설정하기
[🔗 설정하기 참고 링크](https://github.com/leoforfree/cz-customizable#options)


## 3. commitlint - commitizen 룰 동기화

### a. commitlint룰이 commitizen룰의 typeEnum을 바라보도록 해서 동기화
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

# 더 알아보면 좋을 것들
## GitHub Actions + [🔗semantic-release](https://semantic-release.gitbook.io/semantic-release/) + CI
- 원하는 브랜치에 푸시받은 경우 github-actions에서 semantic-release가 실행
- semantic-release가 semantic-versioning에 따라 major/minor/patch중 어떤 버전을 올려야 할지 commit 메시지(conventional-commit의 형식)를 보고 자동으로 태그를 푸시해줍니다.
- 이 버전 태그를 보고 CI - CD를 진행하게 합니다.