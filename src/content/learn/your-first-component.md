---
title: Component Yako ya Kwanza
---

<Intro>

*Components (vipengele)* ni mojawapo ya dhana kuu za React. Ni msingi ambao juu yake unajenga violesura vya mtumiaji (UI), jambo linalozifanya kuwa mahali pazuri pa kuanzia safari yako ya React!

</Intro>

<YouWillLearn>

* Component ni nini
* Nafasi gani components zinacheza katika programu ya React
* Jinsi ya kuandika component yako ya kwanza ya React

</YouWillLearn>

## Components: vipengele vya ujenzi wa UI {/*components-ui-building-blocks*/}

Kwenye Wavuti, HTML hutuwezesha kuunda hati zilizopangwa vizuri kwa kutumia seti yake ya tagi zilizojengwa ndani kama `<h1>` na `<li>`:

```html
<article>
  <h1>Component Yangu ya Kwanza</h1>
  <ol>
    <li>Components: Vipengele vya Ujenzi wa UI</li>
    <li>Kufafanua Component</li>
    <li>Kutumia Component</li>
  </ol>
</article>
```

Markup hii inawakilisha makala haya `<article>`, kichwa chake `<h1>`, na jedwali la yaliyomo (kwa muhtasari) kama orodha iliyopangwa `<ol>`. Markup kama hii, ikiunganishwa na CSS kwa ajili ya mtindo, na JavaScript kwa ajili ya mwingiliano, ipo nyuma ya kila sidebar, avatar, modal, dropdown—kila kipande cha UI unachokiona kwenye Wavuti.

React hukuruhusu kuunganisha markup, CSS, na JavaScript yako kuwa "components" maalum, **elementi za UI zinazoweza kutumika tena kwa programu yako.** Msimbo wa jedwali la yaliyomo uliouona hapo juu ungeweza kugeuzwa kuwa component ya `<TableOfContents />` ambayo ungeweza kui-render kwenye kila ukurasa. Ndani ya pazia, bado inatumia tagi zilezile za HTML kama `<article>`, `<h1>`, n.k.

Kama ilivyo kwa tagi za HTML, unaweza kupanga, kuratibu na kupachika components ili kubuni kurasa nzima. Kwa mfano, ukurasa wa nyaraka unaousoma umetengenezwa kwa components za React:

```js
<PageLayout>
  <NavigationHeader>
    <SearchBar />
    <Link to="/docs">Docs</Link>
  </NavigationHeader>
  <Sidebar />
  <PageContent>
    <TableOfContents />
    <DocumentationText />
  </PageContent>
</PageLayout>
```

Kadri mradi wako unavyokua, utagundua kuwa michoro yako mingi inaweza kuundwa kwa kutumia tena components ulizoandika tayari, ikiharakisha uendelezaji wako. Jedwali letu la yaliyomo hapo juu lingeweza kuongezwa kwenye skrini yoyote kwa `<TableOfContents />`! Unaweza hata kuanzisha mradi wako kwa kasi kwa kutumia maelfu ya components zinazoshirikiwa na jamii ya chanzo huria ya React kama [Chakra UI](https://chakra-ui.com/) na [Material UI.](https://material-ui.com/)

## Kufafanua component {/*defining-a-component*/}

Kijadi, wakati wa kuunda kurasa za wavuti, watengenezaji wa wavuti waliweka markup kwenye maudhui yao kisha waliongeza mwingiliano kwa kunyunyiza JavaScript kidogo. Hili lilifanya kazi vizuri wakati mwingiliano ulikuwa jambo la ziada tu kwenye wavuti. Sasa unatarajiwa kwa tovuti nyingi na programu zote. React huweka mwingiliano mbele huku ikitumia teknolojia ileile: **component ya React ni function ya JavaScript ambayo unaweza _kuinyunyizia markup_.** Hivi ndivyo inavyoonekana (unaweza kuhariri mfano ulio hapa chini):

<Sandpack>

```js
export default function Profile() {
  return (
    <img
      src="https://i.imgur.com/MK3eW3Am.jpg"
      alt="Katherine Johnson"
    />
  )
}
```

```css
img { height: 200px; }
```

</Sandpack>

Na hivi ndivyo unavyojenga component:

### Hatua ya 1: Export component {/*step-1-export-the-component*/}

Kiambishi `export default` ni [sintaksia ya kawaida ya JavaScript](https://developer.mozilla.org/docs/web/javascript/reference/statements/export) (siyo mahususi kwa React). Hukuruhusu kutia alama function kuu katika faili ili baadaye uweze kuii-import kutoka faili nyingine. (Zaidi kuhusu ku-import katika [Kuingiza na Kutoa Components](/learn/importing-and-exporting-components)!)

### Hatua ya 2: Fafanua function {/*step-2-define-the-function*/}

Kwa `function Profile() { }` unafafanua function ya JavaScript yenye jina `Profile`.

<Pitfall>

Components za React ni functions za kawaida za JavaScript, lakini **majina yao lazima yaanze na herufi kubwa** ama hazitafanya kazi!

</Pitfall>

### Hatua ya 3: Ongeza markup {/*step-3-add-markup*/}

Component hurudisha tagi ya `<img />` yenye sifa za `src` na `alt`. `<img />` imeandikwa kama HTML, lakini kwa hakika ni JavaScript ndani ya pazia! Sintaksia hii inaitwa [JSX](/learn/writing-markup-with-jsx), na hukuruhusu kupachika markup ndani ya JavaScript.

Kauli za return zinaweza kuandikwa zote katika mstari mmoja, kama katika component hii:

```js
return <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />;
```

Lakini kama markup yako haiko yote katika mstari mmoja na neno kuu la `return`, lazima uifunge ndani ya jozi ya mabano:

```js
return (
  <div>
    <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />
  </div>
);
```

<Pitfall>

Bila mabano, msimbo wowote ulio katika mistari baada ya `return` [utapuuzwa](https://stackoverflow.com/questions/2846283/what-are-the-rules-for-javascripts-automatic-semicolon-insertion-asi)!

</Pitfall>

## Kutumia component {/*using-a-component*/}

Sasa kwa kuwa umefafanua component yako ya `Profile`, unaweza kuipachika ndani ya components nyingine. Kwa mfano, unaweza ku-export component ya `Gallery` inayotumia components nyingi za `Profile`:

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

### Kile kivinjari kinachokiona {/*what-the-browser-sees*/}

Angalia tofauti katika matumizi ya herufi kubwa na ndogo:

* `<section>` ni herufi ndogo, hivyo React inajua tunarejelea tagi ya HTML.
* `<Profile />` inaanza na herufi kubwa `P`, hivyo React inajua tunataka kutumia component yetu iitwayo `Profile`.

Na `Profile` ina HTML zaidi hata: `<img />`. Mwishoni, hiki ndicho kivinjari kinachokiona:

```html
<section>
  <h1>Wanasayansi wa ajabu</h1>
  <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />
  <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />
  <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />
</section>
```

### Kupachika na kupanga components {/*nesting-and-organizing-components*/}

Components ni functions za kawaida za JavaScript, hivyo unaweza kuweka components nyingi katika faili moja. Hili ni rahisi wakati components ni ndogo kiasi au zinahusiana kwa karibu. Kama faili hii itakuwa na msongamano, unaweza kila wakati kuhamisha `Profile` kwenye faili tofauti. Utajifunza jinsi ya kufanya hivi hivi punde kwenye [ukurasa kuhusu imports.](/learn/importing-and-exporting-components)

Kwa sababu components za `Profile` zina-render ndani ya `Gallery`—hata mara kadhaa!—tunaweza kusema kuwa `Gallery` ni **component-mzazi,** ikii-render kila `Profile` kama "mtoto". Hii ni sehemu ya uchawi wa React: unaweza kufafanua component mara moja, kisha kuitumia katika sehemu nyingi na mara nyingi upendavyo.

<Pitfall>

Components zinaweza ku-render components nyingine, lakini **kamwe usipachike ufafanuzi wao:**

```js {2-5}
export default function Gallery() {
  // 🔴 Kamwe usifafanue component ndani ya component nyingine!
  function Profile() {
    // ...
  }
  // ...
}
```

Kipande cha msimbo hapo juu ni [cha polepole sana na husababisha hitilafu (bugs).](/learn/preserving-and-resetting-state#different-components-at-the-same-position-reset-state) Badala yake, fafanua kila component katika ngazi ya juu:

```js {5-8}
export default function Gallery() {
  // ...
}

// ✅ Tangaza components katika ngazi ya juu
function Profile() {
  // ...
}
```

Wakati component-mtoto inahitaji data fulani kutoka kwa mzazi, [ipitishe kwa props](/learn/passing-props-to-a-component) badala ya kupachika ufafanuzi.

</Pitfall>

<DeepDive>

#### Components hadi chini kabisa {/*components-all-the-way-down*/}

Programu yako ya React huanza kwenye component ya "mzizi" (root). Kwa kawaida, huundwa kiotomatiki unapoanzisha mradi mpya. Kwa mfano, ukitumia [CodeSandbox](https://codesandbox.io/) au ukitumia framework [Next.js](https://nextjs.org/), component-mzizi hufafanuliwa katika `pages/index.js`. Katika mifano hii, umekuwa uki-export components-mizizi.

Programu nyingi za React hutumia components hadi chini kabisa. Hii inamaanisha hutatumia components kwa vipande vinavyoweza kutumika tena kama vitufe tu, bali pia kwa vipande vikubwa kama sidebars, orodha, na hatimaye, kurasa kamili! Components ni njia rahisi ya kupanga msimbo wa UI na markup, hata kama baadhi yao hutumika mara moja tu.

[Frameworks zinazotegemea React](/learn/creating-a-react-app) huchukua hili hatua zaidi. Badala ya kutumia faili tupu ya HTML na kuiacha React "ichukue jukumu" la kusimamia ukurasa kwa JavaScript, *pia* huzalisha HTML kiotomatiki kutoka kwa components zako za React. Hili huruhusu programu yako kuonyesha maudhui fulani kabla msimbo wa JavaScript haujapakiwa.

Hata hivyo, tovuti nyingi hutumia React kwa ajili ya [kuongeza mwingiliano kwenye kurasa za HTML zilizopo tu.](/learn/add-react-to-an-existing-project#using-react-for-a-part-of-your-existing-page) Zina components-mizizi nyingi badala ya moja kwa ukurasa mzima. Unaweza kutumia React kwa wingi—au kwa uchache—kadri unavyohitaji.

</DeepDive>

<Recap>

Umepata onjo lako la kwanza la React! Hebu tupitie tena baadhi ya mambo muhimu.

* React hukuruhusu kuunda components, **elementi za UI zinazoweza kutumika tena kwa programu yako.**
* Katika programu ya React, kila kipande cha UI ni component.
* Components za React ni functions za kawaida za JavaScript isipokuwa:

  1. Majina yao huanza na herufi kubwa kila wakati.
  2. Hurudisha markup ya JSX.

</Recap>



<Challenges>

#### Export component {/*export-the-component*/}

Sanduku hili la mchanga (sandbox) halifanyi kazi kwa sababu component-mzizi haija-export:

<Sandpack>

```js
function Profile() {
  return (
    <img
      src="https://i.imgur.com/lICfvbD.jpg"
      alt="Aklilu Lemma"
    />
  );
}
```

```css
img { height: 181px; }
```

</Sandpack>

Jaribu kuirekebisha mwenyewe kabla ya kuangalia suluhisho!

<Solution>

Ongeza `export default` kabla ya ufafanuzi wa function kama hivi:

<Sandpack>

```js
export default function Profile() {
  return (
    <img
      src="https://i.imgur.com/lICfvbD.jpg"
      alt="Aklilu Lemma"
    />
  );
}
```

```css
img { height: 181px; }
```

</Sandpack>

Huenda unajiuliza kwa nini kuandika `export` peke yake hakutoshi kurekebisha mfano huu. Unaweza kujifunza tofauti kati ya `export` na `export default` katika [Kuingiza na Kutoa Components.](/learn/importing-and-exporting-components)

</Solution>

#### Rekebisha kauli ya return {/*fix-the-return-statement*/}

Kuna kitu hakiko sawa kuhusu kauli hii ya `return`. Je, unaweza kuirekebisha?

<Hint>

Huenda ukapata hitilafu ya "Unexpected token" wakati unajaribu kurekebisha hili. Katika hali hiyo, hakikisha kuwa nukta-mkato (semicolon) inaonekana *baada ya* mabano ya kufunga. Kuacha nukta-mkato ndani ya `return ( )` kutasababisha hitilafu.

</Hint>


<Sandpack>

```js
export default function Profile() {
  return
    <img src="https://i.imgur.com/jA8hHMpm.jpg" alt="Katsuko Saruhashi" />;
}
```

```css
img { height: 180px; }
```

</Sandpack>

<Solution>

Unaweza kurekebisha component hii kwa kuhamisha kauli ya return kwenye mstari mmoja kama hivi:

<Sandpack>

```js
export default function Profile() {
  return <img src="https://i.imgur.com/jA8hHMpm.jpg" alt="Katsuko Saruhashi" />;
}
```

```css
img { height: 180px; }
```

</Sandpack>

Au kwa kufunga markup ya JSX inayorudishwa ndani ya mabano yanayofunguka mara tu baada ya `return`:

<Sandpack>

```js
export default function Profile() {
  return (
    <img 
      src="https://i.imgur.com/jA8hHMpm.jpg" 
      alt="Katsuko Saruhashi" 
    />
  );
}
```

```css
img { height: 180px; }
```

</Sandpack>

</Solution>

#### Gundua kosa {/*spot-the-mistake*/}

Kuna tatizo kuhusu jinsi component ya `Profile` inavyotangazwa na kutumika. Je, unaweza kugundua kosa? (Jaribu kukumbuka jinsi React inavyotofautisha components na tagi za kawaida za HTML!)

<Sandpack>

```js
function profile() {
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
      <profile />
      <profile />
      <profile />
    </section>
  );
}
```

```css
img { margin: 0 10px 10px 0; height: 90px; }
```

</Sandpack>

<Solution>

Majina ya components za React lazima yaanze na herufi kubwa.

Badilisha `function profile()` kuwa `function Profile()`, kisha badilisha kila `<profile />` kuwa `<Profile />`:

<Sandpack>

```js
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
img { margin: 0 10px 10px 0; }
```

</Sandpack>

</Solution>

#### Component yako mwenyewe {/*your-own-component*/}

Andika component kutoka mwanzo. Unaweza kuipa jina lolote halali na kurudisha markup yoyote. Kama umeishiwa mawazo, unaweza kuandika component ya `Congratulations` inayoonyesha `<h1>Kazi nzuri!</h1>`. Usisahau kui-export!

<Sandpack>

```js
// Andika component yako hapa chini!

```

</Sandpack>

<Solution>

<Sandpack>

```js
export default function Congratulations() {
  return (
    <h1>Kazi nzuri!</h1>
  );
}
```

</Sandpack>

</Solution>

</Challenges>
