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

## Oplossing

Voeg `"jsx": "preserve"` toe aan `tsconfig.json`.

Voor wijziging:

```json
{
  "extends": "@quasar/app/tsconfig-preset",
  "compilerOptions": {
    "baseUrl": "."
  }
}
```

Na wijziging:

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

Deze error deed zich voor na het upgraden van de VSCode extensie `Volar` van versie 0.36.x naar 0.37.x.

Aangezien dit ervoor zorgt dat Volar nu ook TypeScript controleert in de `template` tag, is het mogelijk dat er plots extra foutmeldingen zijn.

Een workaround om niet meteen de foutmeldingen op te lossen, is om manueel een oudere versie te installeren van Volar. In het extensiemenu, rechtermuisklik op de `Volar` extensie en selecteer `Install Another Version...` en kies voor 0.36.1.
