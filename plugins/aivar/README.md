# aivar — AIVAR-saker fra Claude

Kobler Claude til byggesakene i AIVAR: søk saker, les saksgrunnlag, oppdater
tema-kort og håndter vedlegg i saksmappa på Google Drive.

Pluginen bærer bare tilkoblingen. Verktøyene ligger på serveren
(`/api/mcp` i plattformen) og hentes ved tilkobling, så tool-flaten kan endres
ved deploy uten at pluginen røres. Fagvurderingen gjøres av skillene i din egen
Claude — pluginen inneholder ingen.

## Hva du trenger

| | |
|---|---|
| `base_url` | Grunn-URL til plattformen, uten skråstrek til slutt. Railway → tjenesten → Settings → Networking → Public Domain |
| `api_token` | Samme verdi som `AIVAR_API_TOKEN` i Railway |

Claude spør om begge ved installasjon. `api_token` er `sensitive`, så verdien
lagres i sikker lagring (Keychain / `~/.claude/.credentials.json`) — ikke i
`settings.json` og ikke i git.

## Installasjon

**Claude Code:**

```bash
claude plugin marketplace add nykonto/aivar-plugin
claude plugin install aivar@konsulent-as
```

Markedsplassen heter `konsulent-as` (fra `marketplace.json`), ikke det samme som
repoet — derfor `@konsulent-as` og ikke `@aivar-plugin`.

**Cowork / claude.ai:** Customize → Plugins → Add marketplace →
`nykonto/aivar-plugin`, så Install på `aivar`.

Bruk «Add custom connector» i stedet er ikke mulig: den dialogen tar bare navn,
URL og OAuth Client ID/Secret, og har ikke noe felt for bearer-token.

## Verktøy

| Verktøy | Gjør |
|---|---|
| `sok_saker` | Finn saker på adresse, kommune eller kundenavn |
| `hent_sak` | Full sakskontekst — tiltak, eiendom, kunde, plangrunnlag, kartlag |
| `les_saksgrunnlag` | Sakens tema-kort med relevans, status, innhold og åpne avklaringer |
| `oppdater_kort` | Skriv fakta inn i ett tema-kort |
| `list_filer` | Filene i sakens Drive-mappe |
| `hent_fil` | Innholdet i ett vedlegg (tekst som tekst, bilder som bilde, ellers Drive-lenke) |
| `last_opp_fil` | Legg en fil i saksmappa |

Kort oppdatert herfra får `oppdatert_av = 'mcp'`, så endringer utenfra er til å
skille fra manuelle i saksbildet.

Utenfor verktøyflaten med vilje: e-postsending, statusendring, «merk som betalt»
og sletting. De gjøres i plattformen.

## Hvis tilkoblingen feiler

**`401` ved tilkobling.** Tokenet ble ikke substituert, eller det er feil.
Sjekk at `api_token` er identisk med `AIVAR_API_TOKEN` i Railway.

**`401` bare i Cowork/claude.ai, mens Claude Code virker.** Da honorerer ikke
den runtimen `${user_config.*}` i `headers`. Dokumentert form der er
miljøvariabel, så bytt headeren i `.mcp.json` til

```json
"Authorization": "Bearer ${AIVAR_API_TOKEN}"
```

og sett `AIVAR_API_TOKEN` som miljøvariabel i den flaten.

**Tilkobling henger eller tidsavbrytes.** Claude kobler til fra Anthropics
skyinfrastruktur, ikke fra maskinen din. `base_url` må være offentlig
tilgjengelig — en server som bare er synlig på et internt nettverk virker ikke.
