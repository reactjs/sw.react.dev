---
title: Kuingiza na Kutoa Components
---

<Intro>

Uchawi wa components (vipengele) upo katika uwezo wao wa kutumika tena: unaweza kutengeneza components zinazoundwa na components nyingine. Lakini kadri unavyopachika components nyingi zaidi na zaidi, mara nyingi inakuwa jambo la busara kuanza kuzigawanya katika faili tofauti. Hili hukuwezesha kuweka faili zako rahisi kuchunguza na kutumia tena components katika sehemu nyingi zaidi.

</Intro>

<YouWillLearn>

* Faili ya component-mzizi ni nini
* Jinsi ya ku-import na ku-export component
* Ni lini utumie default na named imports na exports
* Jinsi ya ku-import na ku-export components nyingi kutoka faili moja
* Jinsi ya kugawanya components katika faili nyingi

</YouWillLearn>

## Faili ya component-mzizi {/*the-root-component-file*/}

Katika [Component Yako ya Kwanza](/learn/your-first-component), ulitengeneza component ya `Profile` na component ya `Gallery` inayoi-render:

<Sandpack>

```js
function Profile() {
  return (
    <img
      src="https://i.imgur.com/MK3eW3As.jpg"
      alt="Katherine Johnson"
    />
  );
}

export default function Gallery() {
  return (
    <section>
      <h1>Wanasayansi wa ajabu</h1>
      <Profile />
      <Profile />
      <Profile />
    </section>
  );
}
```

```css
img { margin: 0 10px 10px 0; height: 90px; }
```

</Sandpack>

Kwa sasa hizi zinaishi katika **faili ya component-mzizi,** iitwayo `App.js` katika mfano huu. Kutegemeana na usanidi wako, component-mzizi wako anaweza kuwa katika faili nyingine, hata hivyo. Kama unatumia framework yenye uelekezaji unaotegemea faili (file-based routing), kama Next.js, component-mzizi wako atakuwa tofauti kwa kila ukurasa.

## Kutoa na kuingiza component {/*exporting-and-importing-a-component*/}

Je, iwapo utataka kubadilisha skrini ya kutua (landing screen) baadaye na kuweka orodha ya vitabu vya sayansi hapo? Au kuweka profaili zote sehemu nyingine? Ni jambo la busara kuhamisha `Gallery` na `Profile` nje ya faili ya component-mzizi. Hili litazifanya kuwa za kimoduli zaidi na zinazoweza kutumika tena katika faili nyingine. Unaweza kuhamisha component kwa hatua tatu:

1. **Tengeneza** faili mpya ya JS ya kuweka components ndani yake.
2. **Toa (Export)** function component yako kutoka faili hiyo (ukitumia ama exports za [default](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/export#using_the_default_export) au za [named](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/export#using_named_exports)).
3. **Ingiza (Import)** katika faili ambapo utaitumia component (ukitumia mbinu inayolingana ya ku-import exports za [default](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/import#importing_defaults) au za [named](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/import#import_a_single_export_from_a_module)).

Hapa `Profile` na `Gallery` zote zimehamishwa nje ya `App.js` na kuwekwa katika faili mpya iitwayo `Gallery.js`. Sasa unaweza kubadilisha `App.js` ili ii-import `Gallery` kutoka `Gallery.js`:

<Sandpack>

```js src/App.js
import Gallery from './Gallery.js';

export default function App() {
  return (
    <Gallery />
  );
}
```

```js src/Gallery.js
function Profile() {
  return (
    <img
      src="https://i.imgur.com/QIrZWGIs.jpg"
      alt="Alan L. Hart"
    />
  );
}

export default function Gallery() {
  return (
    <section>
      <h1>Wanasayansi wa ajabu</h1>
      <Profile />
      <Profile />
      <Profile />
    </section>
  );
}
```

```css
img { margin: 0 10px 10px 0; height: 90px; }
```

</Sandpack>

Angalia jinsi mfano huu sasa umegawanywa katika faili mbili za component:

1. `Gallery.js`:
     - Hufafanua component ya `Profile` ambayo hutumika tu ndani ya faili ileile na haija-export.
     - Hutoa component ya `Gallery` kama **default export.**
2. `App.js`:
     - Huingiza `Gallery` kama **default import** kutoka `Gallery.js`.
     - Hutoa component-mzizi wa `App` kama **default export.**


<Note>

Unaweza kukutana na faili zinazoacha kiendelezi cha faili `.js` kama hivi:

```js 
import Gallery from './Gallery';
```

Ama `'./Gallery.js'` au `'./Gallery'` zote zitafanya kazi na React, ingawa ile ya kwanza iko karibu zaidi na jinsi [native ES Modules](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Modules) zinavyofanya kazi.

</Note>

<DeepDive>

#### Default dhidi ya named exports {/*default-vs-named-exports*/}

Kuna njia kuu mbili za ku-export thamani kwa JavaScript: default exports na named exports. Hadi sasa, mifano yetu imetumia default exports pekee. Lakini unaweza kutumia mojawapo au zote mbili katika faili ileile. **Faili haiwezi kuwa na zaidi ya _default_ export moja, lakini inaweza kuwa na _named_ exports nyingi kadri upendavyo.**

![Default and named exports](/images/docs/illustrations/i_import-export.svg)

Jinsi unavyo-export component yako huamua jinsi lazima uii-import. Utapata hitilafu kama utajaribu ku-import default export kwa njia ileile ungeenda kui-import named export! Chati hii inaweza kukusaidia kufuatilia:

| Sintaksia        | Kauli ya export                            | Kauli ya import                           |
| -----------      | -----------                                | -----------                               |
| Default  | `export default function Button() {}` | `import Button from './Button.js';`     |
| Named    | `export function Button() {}`         | `import { Button } from './Button.js';` |

Unapoandika _default_ import, unaweza kuweka jina lolote upendalo baada ya `import`. Kwa mfano, ungeweza kuandika `import Banana from './Button.js'` badala yake na bado ingekupatia default export ileile. Kinyume chake, kwa named imports, jina lazima lilingane pande zote mbili. Ndiyo sababu zinaitwa _named_ imports!

**Watu mara nyingi hutumia default exports kama faili hii-export component moja tu, na hutumia named exports kama itatoa components na thamani nyingi.** Bila kujali mtindo wa uandishi upendao, kila wakati toa majina yenye maana kwa function za component zako na faili zinazozibeba. Components zisizo na majina, kama `export default () => {}`, hazipendekezwi kwa sababu zinafanya utatuzi wa hitilafu (debugging) kuwa mgumu zaidi.

</DeepDive>

## Kutoa na kuingiza components nyingi kutoka faili ileile {/*exporting-and-importing-multiple-components-from-the-same-file*/}

Je, iwapo utataka kuonyesha `Profile` moja tu badala ya gallery? Unaweza ku-export component ya `Profile`, pia. Lakini `Gallery.js` tayari ina *default* export, na huwezi kuwa na default exports _mbili_. Ungeweza kutengeneza faili mpya yenye default export, au ungeweza kuongeza *named* export kwa `Profile`. **Faili inaweza kuwa na default export moja tu, lakini inaweza kuwa na named exports nyingi mno!**

<Note>

Ili kupunguza mkanganyiko unaowezekana kati ya default na named exports, baadhi ya timu huchagua kushikilia mtindo mmoja tu (default au named), au kuepuka kuzichanganya katika faili moja. Fanya kile kinachokufaa zaidi!

</Note>

Kwanza, **export** `Profile` kutoka `Gallery.js` ukitumia named export (bila neno kuu `default`):

```js
export function Profile() {
  // ...
}
```

Kisha, **import** `Profile` kutoka `Gallery.js` hadi `App.js` ukitumia named import (na mabano ya curly):

```js
import { Profile } from './Gallery.js';
```

Hatimaye, **render** `<Profile />` kutoka component ya `App`:

```js
export default function App() {
  return <Profile />;
}
```

Sasa `Gallery.js` ina exports mbili: default `Gallery` export, na named `Profile` export. `App.js` huzi-import zote mbili. Jaribu kuhariri `<Profile />` kuwa `<Gallery />` na kurudi katika mfano huu:

<Sandpack>

```js src/App.js
import Gallery from './Gallery.js';
import { Profile } from './Gallery.js';

export default function App() {
  return (
    <Profile />
  );
}
```

```js src/Gallery.js
export function Profile() {
  return (
    <img
      src="https://i.imgur.com/QIrZWGIs.jpg"
      alt="Alan L. Hart"
    />
  );
}

export default function Gallery() {
  return (
    <section>
      <h1>Wanasayansi wa ajabu</h1>
      <Profile />
      <Profile />
      <Profile />
    </section>
  );
}
```

```css
img { margin: 0 10px 10px 0; height: 90px; }
```

</Sandpack>

Sasa unatumia mchanganyiko wa default na named exports:

* `Gallery.js`:
  - Hutoa component ya `Profile` kama **named export iitwayo `Profile`.**
  - Hutoa component ya `Gallery` kama **default export.**
* `App.js`:
  - Huingiza `Profile` kama **named import iitwayo `Profile`** kutoka `Gallery.js`.
  - Huingiza `Gallery` kama **default import** kutoka `Gallery.js`.
  - Hutoa component-mzizi wa `App` kama **default export.**

<Recap>

Katika ukurasa huu ulijifunza:

* Faili ya component-mzizi ni nini
* Jinsi ya ku-import na ku-export component
* Ni lini na jinsi ya kutumia default na named imports na exports
* Jinsi ya ku-export components nyingi kutoka faili ileile

</Recap>



<Challenges>

#### Gawanya components zaidi {/*split-the-components-further*/}

Kwa sasa, `Gallery.js` hu-export `Profile` na `Gallery` zote mbili, jambo ambalo linachanganya kidogo.

Hamisha component ya `Profile` kwenye `Profile.js` yake mwenyewe, kisha badilisha component ya `App` ii-render zote mbili `<Profile />` na `<Gallery />` moja baada ya nyingine.

Unaweza kutumia ama default au named export kwa `Profile`, lakini hakikisha unatumia sintaksia inayolingana ya import katika `App.js` na `Gallery.js` zote mbili! Unaweza kurejelea jedwali kutoka deep dive iliyo hapo juu:

| Sintaksia        | Kauli ya export                            | Kauli ya import                           |
| -----------      | -----------                                | -----------                               |
| Default  | `export default function Button() {}` | `import Button from './Button.js';`     |
| Named    | `export function Button() {}`         | `import { Button } from './Button.js';` |

<Hint>

Usisahau ku-import components zako pale zinapoitwa. Je, `Gallery` haitumii `Profile`, pia?

</Hint>

<Sandpack>

```js src/App.js
import Gallery from './Gallery.js';
import { Profile } from './Gallery.js';

export default function App() {
  return (
    <div>
      <Profile />
    </div>
  );
}
```

```js src/Gallery.js active
    q
// Nihamishe kwenye Profile.js!
export function Profile() {
  return (
    <img
      src="https://i.imgur.com/QIrZWGIs.jpg"
      alt="Alan L. Hart"
    />
  );
}

export default function Gallery() {
  return (
    <section>
      <h1>Wanasayansi wa ajabu</h1>
      <Profile />
      <Profile />
      <Profile />
    </section>
  );
}
```

```js src/Profile.js
```

```css
img { margin: 0 10px 10px 0; height: 90px; }
```

</Sandpack>

Baada ya kuifanya ifanye kazi na aina moja ya exports, ifanye ifanye kazi na aina nyingine.

<Solution>

Hili ndilo suluhisho lenye named exports:

<Sandpack>

```js src/App.js
import Gallery from './Gallery.js';
import { Profile } from './Profile.js';

export default function App() {
  return (
    <div>
      <Profile />
      <Gallery />
    </div>
  );
}
```

```js src/Gallery.js
import { Profile } from './Profile.js';

export default function Gallery() {
  return (
    <section>
      <h1>Wanasayansi wa ajabu</h1>
      <Profile />
      <Profile />
      <Profile />
    </section>
  );
}
```

```js src/Profile.js
export function Profile() {
  return (
    <img
      src="https://i.imgur.com/QIrZWGIs.jpg"
      alt="Alan L. Hart"
    />
  );
}
```

```css
img { margin: 0 10px 10px 0; height: 90px; }
```

</Sandpack>

Hili ndilo suluhisho lenye default exports:

<Sandpack>

```js src/App.js
import Gallery from './Gallery.js';
import Profile from './Profile.js';

export default function App() {
  return (
    <div>
      <Profile />
      <Gallery />
    </div>
  );
}
```

```js src/Gallery.js
import Profile from './Profile.js';

export default function Gallery() {
  return (
    <section>
      <h1>Wanasayansi wa ajabu</h1>
      <Profile />
      <Profile />
      <Profile />
    </section>
  );
}
```

```js src/Profile.js
export default function Profile() {
  return (
    <img
      src="https://i.imgur.com/QIrZWGIs.jpg"
      alt="Alan L. Hart"
    />
  );
}
```

```css
img { margin: 0 10px 10px 0; height: 90px; }
```

</Sandpack>

</Solution>

</Challenges>
