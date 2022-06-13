---
postUuid: e546919a-c329-47a1-9e98-926f8f9972ab
title: Volar Cannot use JSX
slug: volar-cannot-use-jsx
tags:
  - Vue
  - Volar
categories:
  - Frontend
---

## Cannot use JSX

Cannot use JSX unless the '--jsx' flag is provided. ts(17004)

## Solution

Add `"jsx": "preserve"` to `tsconfig.json`.

Before change:

```json
{
  "extends": "@quasar/app/tsconfig-preset",
  "compilerOptions": {
    "baseUrl": "."
  }
}
```

After change:

```json
{
  "extends": "@quasar/app/tsconfig-preset",
  "compilerOptions": {
    "baseUrl": ".",
    "jsx": "preserve"
  }
}
```

## Extra details

This error occurred after upgrading the VSCode extension `Volar` from version 0.36.x to 0.37.x.

Since this causes Volar to now also check TypeScript in the `template` tag, it is possible that there are suddenly additional error messages.

A workaround to not immediately resolve the error messages is to manually install an older version of Volar. In the extension menu, right-click on the `Volar` extension and select `Install Another Version...` and choose 0.36.1.

Translated with www.DeepL.com/Translator (free version)
