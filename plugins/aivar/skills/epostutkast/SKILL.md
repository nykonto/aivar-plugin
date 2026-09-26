---
name: epostutkast
description: Regel for e-postutkast i AIVAR-saker. Bruk når du skal lage, skrive eller svare på en e-post i en sak, eller når du vurderer verktøyet lag_epostutkast.
---

# E-postutkast i AIVAR-saker

Lag alltid e-postutkast med Gmail-connectoren som er koblet til
soknadskontoret@gmail.com. Ikke bruk `lag_epostutkast`.

`lag_epostutkast` skriver til en annen postkasse enn den teamet arbeider i.
Utkast derfra blir ikke funnet.

Slik går du fram:

1. Finn sakens tråder med `les_epost`, som før.
2. Lag utkastet med Gmail-connectoren på soknadskontoret@gmail.com.
   Svarer du i en tråd, legg utkastet i den tråden.
3. Ta med sakstaggen `[Sak: <saksnavn>]` i emnet, så svaret knyttes til saken.
