---
title: Ku-render kwa Masharti
---

<Intro>

Components (vipengele) zako mara nyingi zitahitaji kuonyesha vitu tofauti kutegemea masharti tofauti. Katika React, unaweza ku-render JSX kwa masharti ukitumia sintaksia ya JavaScript kama kauli za `if`, na opereta za `&&`, na `? :`.

</Intro>

<YouWillLearn>

* Jinsi ya kurudisha JSX tofauti kutegemea sharti
* Jinsi ya kujumuisha au kutojumuisha kipande cha JSX kwa masharti
* Njia mkato za kawaida za masharti utakazokutana nazo katika misimbo ya React

</YouWillLearn>

## Kurudisha JSX kwa masharti {/*conditionally-returning-jsx*/}

Tuseme una component ya `PackingList` inayo-render `Item` kadhaa, ambazo zinaweza kuwekewa alama kuwa zimepakiwa au la:

<Sandpack>

```js
function Item({ name, isPacked }) {
  return <li className="item">{name}</li>;
}

export default function PackingList() {
  return (
    <section>
      <h1>Orodha ya kupakia ya Sally Ride</h1>
      <ul>
        <Item
          isPacked={true}
          name="Suti ya angani"
        />
        <Item
          isPacked={true}
          name="Kofia yenye jani la dhahabu"
        />
        <Item
          isPacked={false}
          name="Picha ya Tam"
        />
      </ul>
    </section>
  );
}
```

</Sandpack>

Angalia kuwa baadhi ya components za `Item` zina prop yao ya `isPacked` iliyowekwa kuwa `true` badala ya `false`. Unataka kuongeza alama ya kupe (✅) kwenye vitu vilivyopakiwa kama `isPacked={true}`.

Unaweza kuandika hili kama [kauli ya `if`/`else`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else) kama hivi:

```js
if (isPacked) {
  return <li className="item">{name} ✅</li>;
}
return <li className="item">{name}</li>;
```

Kama prop ya `isPacked` ni `true`, msimbo huu **hurudisha mti tofauti wa JSX.** Kwa mabadiliko haya, baadhi ya vitu hupata alama ya kupe mwishoni:

<Sandpack>

```js
function Item({ name, isPacked }) {
  if (isPacked) {
    return <li className="item">{name} ✅</li>;
  }
  return <li className="item">{name}</li>;
}

export default function PackingList() {
  return (
    <section>
      <h1>Orodha ya kupakia ya Sally Ride</h1>
      <ul>
        <Item
          isPacked={true}
          name="Suti ya angani"
        />
        <Item
          isPacked={true}
          name="Kofia yenye jani la dhahabu"
        />
        <Item
          isPacked={false}
          name="Picha ya Tam"
        />
      </ul>
    </section>
  );
}
```

</Sandpack>

Jaribu kuhariri kinachorudishwa katika kila hali, na uone jinsi matokeo yanavyobadilika!

Angalia jinsi unavyounda mantiki ya matawi kwa kutumia kauli za `if` na `return` za JavaScript. Katika React, mtiririko wa udhibiti (kama masharti) hushughulikiwa na JavaScript.

### Kurudisha bila kitu kwa masharti kwa kutumia `null` {/*conditionally-returning-nothing-with-null*/}

Katika baadhi ya hali, hautataka ku-render chochote kabisa. Kwa mfano, tuseme hautaki kuonyesha vitu vilivyopakiwa hata kidogo. Component lazima irudishe kitu fulani. Katika hali hii, unaweza kurudisha `null`:

```js
if (isPacked) {
  return null;
}
return <li className="item">{name}</li>;
```

Kama `isPacked` ni true, component itarudisha bila kitu, `null`. Vinginevyo, itarudisha JSX ya ku-render.

<Sandpack>

```js
function Item({ name, isPacked }) {
  if (isPacked) {
    return null;
  }
  return <li className="item">{name}</li>;
}

export default function PackingList() {
  return (
    <section>
      <h1>Orodha ya kupakia ya Sally Ride</h1>
      <ul>
        <Item
          isPacked={true}
          name="Suti ya angani"
        />
        <Item
          isPacked={true}
          name="Kofia yenye jani la dhahabu"
        />
        <Item
          isPacked={false}
          name="Picha ya Tam"
        />
      </ul>
    </section>
  );
}
```

</Sandpack>

Kwa vitendo, kurudisha `null` kutoka kwa component si jambo la kawaida kwa sababu linaweza kumshangaza mtengenezaji anayejaribu kuii-render. Mara nyingi zaidi, ungejumuisha au kutojumuisha component kwa masharti katika JSX ya component-mzazi. Hivi ndivyo ya kufanya hivyo!

## Kujumuisha JSX kwa masharti {/*conditionally-including-jsx*/}

Katika mfano uliopita, ulidhibiti ni mti gani wa JSX (kama upo!) ungerudishwa na component. Huenda tayari umeona urudufu fulani katika matokeo ya render:

```js
<li className="item">{name} ✅</li>
```

unafanana sana na

```js
<li className="item">{name}</li>
```

Matawi yote mawili ya masharti hurudisha `<li className="item">...</li>`:

```js
if (isPacked) {
  return <li className="item">{name} ✅</li>;
}
return <li className="item">{name}</li>;
```

Ingawa urudufu huu si wenye madhara, unaweza kufanya msimbo wako kuwa mgumu zaidi kuutunza. Vipi kama unataka kubadilisha `className`? Ungelazimika kufanya hivyo katika sehemu mbili za msimbo wako! Katika hali kama hii, ungeweza kujumuisha JSX kidogo kwa masharti ili kufanya msimbo wako uwe [DRY zaidi.](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)

### Opereta ya masharti (ternary) (`? :`) {/*conditional-ternary-operator--*/}

JavaScript ina sintaksia fupi ya kuandika usemi wa masharti -- [opereta ya masharti](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator) au "opereta ya ternary".

Badala ya hii:

```js
if (isPacked) {
  return <li className="item">{name} ✅</li>;
}
return <li className="item">{name}</li>;
```

Unaweza kuandika hii:

```js
return (
  <li className="item">
    {isPacked ? name + ' ✅' : name}
  </li>
);
```

Unaweza kuisoma kama *"kama `isPacked` ni true, basi (`?`) render `name + ' ✅'`, vinginevyo (`:`) render `name`"*.

<DeepDive>

#### Je, mifano hii miwili inalingana kabisa? {/*are-these-two-examples-fully-equivalent*/}

Kama unatoka kwenye msingi wa programu inayoegemea object (object-oriented programming), unaweza kudhani kuwa mifano miwili hapo juu ina tofauti ndogo kwa sababu mmoja wao unaweza kuunda "instances" mbili tofauti za `<li>`. Lakini elementi za JSX si "instances" kwa sababu hazishikilii state (hali) yoyote ya ndani na si nodi halisi za DOM. Ni maelezo mepesi, kama ramani za ujenzi. Hivyo mifano hii miwili, kwa hakika, *inalingana* kabisa. [Kuhifadhi na Kuweka Upya State](/learn/preserving-and-resetting-state) inaeleza kwa kina jinsi hili linavyofanya kazi.

</DeepDive>

Sasa tuseme unataka kufunga maandishi ya kitu kilichokamilika ndani ya tagi nyingine ya HTML, kama `<del>` ili kuyakata. Unaweza kuongeza mistari mipya na mabano zaidi ili iwe rahisi kupachika JSX zaidi katika kila hali:

<Sandpack>

```js
function Item({ name, isPacked }) {
  return (
    <li className="item">
      {isPacked ? (
        <del>
          {name + ' ✅'}
        </del>
      ) : (
        name
      )}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Orodha ya kupakia ya Sally Ride</h1>
      <ul>
        <Item
          isPacked={true}
          name="Suti ya angani"
        />
        <Item
          isPacked={true}
          name="Kofia yenye jani la dhahabu"
        />
        <Item
          isPacked={false}
          name="Picha ya Tam"
        />
      </ul>
    </section>
  );
}
```

</Sandpack>

Mtindo huu hufanya kazi vizuri kwa masharti rahisi, lakini utumie kwa kiasi. Kama components zako zinakuwa na msongamano kwa markup nyingi sana ya masharti iliyopachikwa, fikiria kutoa components-watoto ili kusafisha mambo. Katika React, markup ni sehemu ya msimbo wako, hivyo unaweza kutumia zana kama vigezo na functions kuratibu semi zenye utata.

### Opereta ya kimantiki ya AND (`&&`) {/*logical-and-operator-*/}

Njia mkato nyingine ya kawaida utakayokutana nayo ni [opereta ya kimantiki ya AND (`&&`) ya JavaScript.](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_AND#:~:text=The%20logical%20AND%20(%20%26%26%20)%20operator,it%20returns%20a%20Boolean%20value.) Ndani ya components za React, mara nyingi hujitokeza wakati unataka ku-render JSX fulani wakati sharti ni true, **au render bila kitu vinginevyo.** Kwa `&&`, ungeweza ku-render alama ya kupe kwa masharti tu kama `isPacked` ni `true`:

```js
return (
  <li className="item">
    {name} {isPacked && '✅'}
  </li>
);
```

Unaweza kuisoma kama *"kama `isPacked`, basi (`&&`) render alama ya kupe, vinginevyo, render bila kitu"*.

Hii hapa ikifanya kazi:

<Sandpack>

```js
function Item({ name, isPacked }) {
  return (
    <li className="item">
      {name} {isPacked && '✅'}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Orodha ya kupakia ya Sally Ride</h1>
      <ul>
        <Item
          isPacked={true}
          name="Suti ya angani"
        />
        <Item
          isPacked={true}
          name="Kofia yenye jani la dhahabu"
        />
        <Item
          isPacked={false}
          name="Picha ya Tam"
        />
      </ul>
    </section>
  );
}
```

</Sandpack>

[Usemi wa && wa JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_AND) hurudisha thamani ya upande wake wa kulia (katika hali yetu, alama ya kupe) kama upande wa kushoto (sharti letu) ni `true`. Lakini kama sharti ni `false`, usemi mzima unakuwa `false`. React huchukulia `false` kama "shimo" katika mti wa JSX, kama tu `null` au `undefined`, na hairenderi chochote mahali pake.


<Pitfall>

**Usiweke namba upande wa kushoto wa `&&`.**

Ili kupima sharti, JavaScript hubadilisha upande wa kushoto kuwa boolean kiotomatiki. Hata hivyo, kama upande wa kushoto ni `0`, basi usemi mzima unapata thamani hiyo (`0`), na React itaenda mbele kwa furaha ku-render `0` badala ya bila kitu.

Kwa mfano, kosa la kawaida ni kuandika msimbo kama `messageCount && <p>New messages</p>`. Ni rahisi kudhani kuwa hairenderi chochote wakati `messageCount` ni `0`, lakini kwa hakika inarender `0` yenyewe!

Ili kurekebisha hilo, fanya upande wa kushoto kuwa boolean: `messageCount > 0 && <p>New messages</p>`.

</Pitfall>

### Kukabidhi JSX kwa kigezo kwa masharti {/*conditionally-assigning-jsx-to-a-variable*/}

Wakati njia mkato zinakwamisha kuandika msimbo wa kawaida, jaribu kutumia kauli ya `if` na kigezo. Unaweza kukabidhi upya vigezo vilivyofafanuliwa kwa [`let`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let), hivyo anza kwa kutoa maudhui chaguo-msingi unayotaka kuonyesha, jina:

```js
let itemContent = name;
```

Tumia kauli ya `if` kukabidhi upya usemi wa JSX kwa `itemContent` kama `isPacked` ni `true`:

```js
if (isPacked) {
  itemContent = name + " ✅";
}
```

[Mabano ya curly hufungua "dirisha la kuingia JavaScript".](/learn/javascript-in-jsx-with-curly-braces#using-curly-braces-a-window-into-the-javascript-world) Pachika kigezo kwa mabano ya curly katika mti wa JSX unaorudishwa, ukipachika usemi uliokokotolewa awali ndani ya JSX:

```js
<li className="item">
  {itemContent}
</li>
```

Mtindo huu ni wenye maneno mengi zaidi, lakini pia ni unaonyumbulika zaidi. Hii hapa ikifanya kazi:

<Sandpack>

```js
function Item({ name, isPacked }) {
  let itemContent = name;
  if (isPacked) {
    itemContent = name + " ✅";
  }
  return (
    <li className="item">
      {itemContent}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Orodha ya kupakia ya Sally Ride</h1>
      <ul>
        <Item
          isPacked={true}
          name="Suti ya angani"
        />
        <Item
          isPacked={true}
          name="Kofia yenye jani la dhahabu"
        />
        <Item
          isPacked={false}
          name="Picha ya Tam"
        />
      </ul>
    </section>
  );
}
```

</Sandpack>

Kama ilivyokuwa awali, hii hufanya kazi si tu kwa maandishi, bali pia kwa JSX yoyote ile:

<Sandpack>

```js
function Item({ name, isPacked }) {
  let itemContent = name;
  if (isPacked) {
    itemContent = (
      <del>
        {name + " ✅"}
      </del>
    );
  }
  return (
    <li className="item">
      {itemContent}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Orodha ya kupakia ya Sally Ride</h1>
      <ul>
        <Item
          isPacked={true}
          name="Suti ya angani"
        />
        <Item
          isPacked={true}
          name="Kofia yenye jani la dhahabu"
        />
        <Item
          isPacked={false}
          name="Picha ya Tam"
        />
      </ul>
    </section>
  );
}
```

</Sandpack>

Kama hufahamu JavaScript vizuri, aina hii mbalimbali ya mitindo inaweza kuonekana kuwa nyingi mno mwanzoni. Hata hivyo, kuzijifunza kutakusaidia kusoma na kuandika msimbo wowote wa JavaScript -- na si components za React tu! Chagua ule unaoupendelea kwa kuanzia, kisha rejea kwenye rejeleo hili tena kama utasahau jinsi zile nyingine zinavyofanya kazi.

<Recap>

* Katika React, unadhibiti mantiki ya matawi kwa JavaScript.
* Unaweza kurudisha usemi wa JSX kwa masharti kwa kauli ya `if`.
* Unaweza kuhifadhi JSX fulani kwa kigezo kwa masharti kisha kuijumuisha ndani ya JSX nyingine kwa kutumia mabano ya curly.
* Katika JSX, `{cond ? <A /> : <B />}` inamaanisha *"kama `cond`, render `<A />`, vinginevyo `<B />`"*.
* Katika JSX, `{cond && <A />}` inamaanisha *"kama `cond`, render `<A />`, vinginevyo bila kitu"*.
* Njia mkato ni za kawaida, lakini si lazima uzitumie kama unapendelea `if` ya kawaida.

</Recap>



<Challenges>

#### Onyesha aikoni kwa vitu visivyokamilika kwa `? :` {/*show-an-icon-for-incomplete-items-with--*/}

Tumia opereta ya masharti (`cond ? a : b`) ku-render ❌ kama `isPacked` si `true`.

<Sandpack>

```js
function Item({ name, isPacked }) {
  return (
    <li className="item">
      {name} {isPacked && '✅'}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Orodha ya kupakia ya Sally Ride</h1>
      <ul>
        <Item
          isPacked={true}
          name="Suti ya angani"
        />
        <Item
          isPacked={true}
          name="Kofia yenye jani la dhahabu"
        />
        <Item
          isPacked={false}
          name="Picha ya Tam"
        />
      </ul>
    </section>
  );
}
```

</Sandpack>

<Solution>

<Sandpack>

```js
function Item({ name, isPacked }) {
  return (
    <li className="item">
      {name} {isPacked ? '✅' : '❌'}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Orodha ya kupakia ya Sally Ride</h1>
      <ul>
        <Item
          isPacked={true}
          name="Suti ya angani"
        />
        <Item
          isPacked={true}
          name="Kofia yenye jani la dhahabu"
        />
        <Item
          isPacked={false}
          name="Picha ya Tam"
        />
      </ul>
    </section>
  );
}
```

</Sandpack>

</Solution>

#### Onyesha umuhimu wa kitu kwa `&&` {/*show-the-item-importance-with-*/}

Katika mfano huu, kila `Item` hupokea prop ya kinamba ya `importance`. Tumia opereta ya `&&` ku-render "_(Umuhimu: X)_" kwa italiki, lakini kwa vitu vyenye umuhimu usio sifuri tu. Orodha yako ya vitu inapaswa kuishia kuonekana hivi:

* Suti ya angani _(Umuhimu: 9)_
* Kofia yenye jani la dhahabu
* Picha ya Tam _(Umuhimu: 6)_

Usisahau kuongeza nafasi kati ya lebo mbili!

<Sandpack>

```js
function Item({ name, importance }) {
  return (
    <li className="item">
      {name}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Orodha ya kupakia ya Sally Ride</h1>
      <ul>
        <Item
          importance={9}
          name="Suti ya angani"
        />
        <Item
          importance={0}
          name="Kofia yenye jani la dhahabu"
        />
        <Item
          importance={6}
          name="Picha ya Tam"
        />
      </ul>
    </section>
  );
}
```

</Sandpack>

<Solution>

Hii inapaswa kufanya kazi:

<Sandpack>

```js
function Item({ name, importance }) {
  return (
    <li className="item">
      {name}
      {importance > 0 && ' '}
      {importance > 0 &&
        <i>(Umuhimu: {importance})</i>
      }
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Orodha ya kupakia ya Sally Ride</h1>
      <ul>
        <Item
          importance={9}
          name="Suti ya angani"
        />
        <Item
          importance={0}
          name="Kofia yenye jani la dhahabu"
        />
        <Item
          importance={6}
          name="Picha ya Tam"
        />
      </ul>
    </section>
  );
}
```

</Sandpack>

Zingatia kuwa lazima uandike `importance > 0 && ...` badala ya `importance && ...` ili kama `importance` ni `0`, `0` isirenderiwe kama matokeo!

Katika suluhisho hili, masharti mawili tofauti yanatumika kuingiza nafasi kati ya jina na lebo ya umuhimu. Vinginevyo, ungeweza kutumia Fragment yenye nafasi mwanzoni: `importance > 0 && <> <i>...</i></>` au kuongeza nafasi mara moja ndani ya `<i>`:  `importance > 0 && <i> ...</i>`.

</Solution>

#### Boresha msururu wa `? :` kuwa `if` na vigezo {/*refactor-a-series-of---to-if-and-variables*/}

Component hii ya `Drink` inatumia msururu wa masharti ya `? :` kuonyesha taarifa tofauti kutegemea kama prop ya `name` ni `"tea"` au `"coffee"`. Tatizo ni kwamba taarifa kuhusu kila kinywaji zimetawanyika katika masharti mengi. Boresha msimbo huu utumie kauli moja ya `if` badala ya masharti matatu ya `? :`.

<Sandpack>

```js
function Drink({ name }) {
  return (
    <section>
      <h1>{name}</h1>
      <dl>
        <dt>Part of plant</dt>
        <dd>{name === 'tea' ? 'leaf' : 'bean'}</dd>
        <dt>Caffeine content</dt>
        <dd>{name === 'tea' ? '15–70 mg/cup' : '80–185 mg/cup'}</dd>
        <dt>Age</dt>
        <dd>{name === 'tea' ? '4,000+ years' : '1,000+ years'}</dd>
      </dl>
    </section>
  );
}

export default function DrinkList() {
  return (
    <div>
      <Drink name="tea" />
      <Drink name="coffee" />
    </div>
  );
}
```

</Sandpack>

Mara tu utakapoboresha msimbo utumie `if`, je, una mawazo zaidi ya jinsi ya kuurahisisha?

<Solution>

Kuna njia nyingi ungeweza kufuata, lakini hapa kuna mahali pa kuanzia:

<Sandpack>

```js
function Drink({ name }) {
  let part, caffeine, age;
  if (name === 'tea') {
    part = 'leaf';
    caffeine = '15–70 mg/cup';
    age = '4,000+ years';
  } else if (name === 'coffee') {
    part = 'bean';
    caffeine = '80–185 mg/cup';
    age = '1,000+ years';
  }
  return (
    <section>
      <h1>{name}</h1>
      <dl>
        <dt>Part of plant</dt>
        <dd>{part}</dd>
        <dt>Caffeine content</dt>
        <dd>{caffeine}</dd>
        <dt>Age</dt>
        <dd>{age}</dd>
      </dl>
    </section>
  );
}

export default function DrinkList() {
  return (
    <div>
      <Drink name="tea" />
      <Drink name="coffee" />
    </div>
  );
}
```

</Sandpack>

Hapa taarifa kuhusu kila kinywaji zimewekwa pamoja badala ya kutawanyika katika masharti mengi. Hili hurahisisha kuongeza vinywaji zaidi siku zijazo.

Suluhisho jingine lingekuwa kuondoa sharti kabisa kwa kuhamisha taarifa ndani ya objects:

<Sandpack>

```js
const drinks = {
  tea: {
    part: 'leaf',
    caffeine: '15–70 mg/cup',
    age: '4,000+ years'
  },
  coffee: {
    part: 'bean',
    caffeine: '80–185 mg/cup',
    age: '1,000+ years'
  }
};

function Drink({ name }) {
  const info = drinks[name];
  return (
    <section>
      <h1>{name}</h1>
      <dl>
        <dt>Part of plant</dt>
        <dd>{info.part}</dd>
        <dt>Caffeine content</dt>
        <dd>{info.caffeine}</dd>
        <dt>Age</dt>
        <dd>{info.age}</dd>
      </dl>
    </section>
  );
}

export default function DrinkList() {
  return (
    <div>
      <Drink name="tea" />
      <Drink name="coffee" />
    </div>
  );
}
```

</Sandpack>

</Solution>

</Challenges>
