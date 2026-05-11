---
title: Mpangilio wa Kihariri
---

<Intro>

Kihariri kilichosawazishwa vizuri kinaweza kufanya kodi iwe rahisi kusoma na haraka kuandika. Inaweza hata kukusaidia kugundua makosa unapoandika! Ikiwa ni mara yako ya kwanza kupanga kihariri au unatafuta kuboresha kihariri chako cha sasa, tuna mapendekezo machache.

</Intro>

<YouWillLearn>

* Vihariri maarufu zaidi ni vipi
* Jinsi ya kupanga muundo wa kodi yako kiotomatiki

</YouWillLearn>

## Kihariri chako {/*your-editor*/}

[VS Code](https://code.visualstudio.com/) ni moja ya vihariri maarufu zaidi vinavyotumika leo. Ina soko kubwa la viendelezi na inaingiliana vizuri na huduma maarufu kama GitHub. Nyingi ya maumbili yalivyoorodheshwa hapa chini yanaweza kuongezwa kwenye VS Code kama viendelezi pia, na kuifanya iwe rahisi sana kuisuka!

Vihariri vingine vya maandishi maarufu vinavyotumika katika jamii ya React ni pamoja na:

* [WebStorm](https://www.jetbrains.com/webstorm/) ni mazingira jumuishi ya usanidi yaliyoundwa mahususi kwa ajili ya JavaScript.
* [Sublime Text](https://www.sublimetext.com/) ina msaada wa JSX na TypeScript, [uangaziaji wa sintaksi](https://stackoverflow.com/a/70960574/458193) (syntax highlighting) na ukamilishaji wa maneno (autocomplete) uliomo ndani yake.
* [Vim](https://www.vim.org/) ni kihariri cha maandishi kinachoweza kusukwa sana kilichoundwa ili kufanya uundaji na ubadilishaji wa aina yoyote ya maandishi kuwa wenye ufanisi mkubwa. Inajumuishwa kama "vi" kwenye mifumo mingi ya UNIX na Apple OS X.

## Maumbile ya kihariri cha maandishi yanavyopendekezwa {/*recommended-text-editor-features*/}

Baadhi ya vihariri huja na maumbile haya yakiwa yamejengwa ndani, lakini vingine vinaweza kuhitaji kuongeza kiendelezi. Angalia msaada unaotolewa na kihariri chako unachopendelea ili kuwa na uhakika!

### Linting {/*linting*/}

<<<<<<< HEAD
Code linters hupata matatizo katika kodi yako unapoandika, zikikusaidia kuyarekebisha mapema. [ESLint](https://eslint.org/) ni linter maarufu ya chanzo wazi (open source) kwa ajili ya JavaScript.
=======
Code linters find problems in your code as you write, helping you fix them early. [ESLint](https://eslint.org/) is a popular, open source linter for JavaScript.
>>>>>>> abe931a8cb3aee3e8b15ef7e187214789164162a

* [Sakinisha ESLint kwa mpangilio uliopendekezwa kwa React](https://www.npmjs.com/package/eslint-config-react-app) (hakikisha una [Node imesakinishwa!](https://nodejs.org/en/download/current/))
* [Unganisha ESLint katika VSCode na kiendelezi rasmi](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

**Hakikisha kuwa umewezesha sheria zote za [`eslint-plugin-react-hooks`](https://www.npmjs.com/package/eslint-plugin-react-hooks) kwa mradi wako.** Ni muhimu na hugundua makosa makubwa zaidi mapema. Mpangilio wa [`eslint-config-react-app`](https://www.npmjs.com/package/eslint-config-react-app) uliopendekezwa tayari unazijumuisha.

### Uumbizaji {/*formatting*/}

Jambo la mwisho unalotaka kufanya unapoandika kodi na mchangiaji mwingine ni kuingia katika mjadala kuhusu [tabs vs spaces](https://www.google.com/search?q=tabs+vs+spaces)! Kwa bahati nzuri, [Prettier](https://prettier.io/) itasafisha kodi yako kwa kuiumbiza upya ili kufuata sheria zilizowekwa. Endesha Prettier, na tabo zako zote zitabadilishwa kuwa nafasi (spaces)—na mpangilio wako, alama za nukuu, n.k. pia zitabadilishwa ili kufuata usanidi. Katika mpangilio bora, Prettier itaendesha unapohifadhi faili yako, ikikufanyia marekebisho haya haraka.

Unaweza kusakinisha [kiendelezi cha Prettier katika VSCode](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) kwa kufuata hatua hizi:

1. Fungua VS Code
2. Tumia Quick Open (bonyeza Ctrl/Cmd+P)
3. Bandika `ext install esbenp.prettier-vscode`
4. Bonyeza Enter

#### Kuumbiza wakati wa kuhifadhi {/*formatting-on-save*/}

Kwa kweli, unapaswa kuumbiza kodi yako kila unapohifadhi. VS Code ina mipangilio ya hili!

1. Katika VS Code, bonyeza `CTRL/CMD + SHIFT + P`.
2. Andika "settings"
3. Bonyeza Enter
4. Kwenye sehemu ya utafutaji, andika "format on save"
5. Hakikisha chaguo la "format on save" limewekwa alama ya kuteua (ticked)!

> Ikiwa mpangilio wako wa ESLint una sheria za uumbizaji, zinaweza kugongana na Prettier. Tunapendekeza kulemaza sheria zote za uumbizaji katika mpangilio wako wa ESLint ukitumia [`eslint-config-prettier`](https://github.com/prettier/eslint-config-prettier) ili ESLint itumike *tu* kwa ajili ya kugundua makosa ya kimantiki. Ikiwa unataka kuhakikisha kuwa faili zimeumbizwa kabla ya pull request kuunganishwa, tumia [`prettier --check`](https://prettier.io/docs/en/cli.html#--check) kwa muunganisho wako unaoendelea (CI).
