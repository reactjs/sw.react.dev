---
title: Quick Start
---

<Intro>

Karibu kwenye nyaraka za React! Ukurasa huu utakupa utangulizi wa 80% ya dhana za React utakazotumia kila siku.

</Intro>

<YouWillLearn>

- Jinsi ya kuunda na kupangilia vipengele
- Jinsi ya kuongeza markup na mitindo
- Jinsi ya kuonyesha data
- Jinsi ya kutoa hali na orodha
- Jinsi ya kujibu matukio na kusasisha skrini
- Jinsi ya kushirikisha data kati ya vipengele

</YouWillLearn>

## Kuunda na Kupangilia Vipengele {/*components*/}

Programu za React zinatengenezwa kwa *vipengele*. Kipengele ni sehemu ya UI (User Interface) yenye mantiki na mwonekano wake. Kipengele kinaweza kuwa kidogo kama kitufe, au kikubwa kama ukurasa mzima.

Vipengele vya React ni functions za JavaScript zinazorejesha markup:

```js
function MyButton() {
  return (
    <button>Mimi ni kitufe</button>
  );
}
```

Sasa kwa kuwa umetangaza `MyButton`, unaweza kukipangilia ndani ya kipengele kingine:

```js {5}
export default function MyApp() {
  return (
    <div>
      <h1>Karibu kwenye programu yangu</h1>
      <MyButton />
    </div>
  );
}
```

Angalia kuwa `<MyButton />` huanza na herufi kubwa. Hii ndiyo inavyoonyesha kuwa ni kipengele cha React. Majina ya vipengele vya React lazima yaanze na herufi kubwa, wakati vitambulisho vya HTML vinapaswa kuwa na herufi ndogo.

Tazama matokeo:

<Sandpack>

```js
function MyButton() {
  return (
    <button>
      I'm a button
    </button>
  );
}

export default function MyApp() {
  return (
    <div>
      <h1>Welcome to my app</h1>
      <MyButton />
    </div>
  );
}
```

</Sandpack>

Keywords za `export default` zinaonyesha kipengele kikuu kwenye faili. Ikiwa haujafahamiana na baadhi ya sintaksia ya JavaScript, [MDN](https://developer.mozilla.org/en-US/docs/web/javascript/reference/statements/export) na [javascript.info](https://javascript.info/import-export) zina rejea nzuri.

## Kuandika Markup ukitumia JSX {/*writing-markup-with-jsx*/}

Sintaksia ya markup uliyokutana nayo hapo juu inaitwa *JSX*. Ni hiari, lakini miradi mingi ya React hutumia JSX kwa urahisi wake. Zana zote tunazopendekeza kwa [vitumizi ya ndani](/learn/installation) zinawezesha JSX moja kwa moja.

JSX ina umanani wa juu zaidi kuliko HTML. Unapaswa kufunga vitambulisho kama `<br />`. Kipengele chako pia hakiwezi kurudisha vitambulisho vingi vya JSX bila kuvifungisha katika mzazi wa pamoja, kama `<div>...</div>` au kifungashio tupu `<>...</>`:

```js {3,6}
function AboutPage() {
  return (
    <>
      <h1>Kuhusu</h1>
      <p>Habari za hapa.<br />Hali yako?</p>
    </>
  );
}
```

Ikiwa una HTML nyingi ya kuhamisha kwenye JSX, unaweza kutumia [kibadilishaji cha mtandaoni.](https://transform.tools/html-to-jsx)

## Kuongeza mitindo {/*adding-styles*/}

Katika React, unaelezea darasa la CSS kwa kutumia `className`. Inafanya function sawa na sifa ya HTML [`class`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/class):

```js
<img className="avatar" />
```

Kisha unaandika sheria za CSS kwa ajili yake kwenye faili tofauti ya CSS:

```css
/* Katika CSS yako */
.avatar {
  border-radius: 50%;
}
```

React haielekezi jinsi ya kuongeza faili za CSS. Katika hali rahisi, utaongeza kitambulisho cha [`<link>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link) kwenye HTML yako. Ikiwa unatumia zana ya ujenzi au fremu, rejea nyaraka zake ili kujifunza jinsi ya kuongeza faili ya CSS kwenye mradi wako.

## Kuonyesha data {/*displaying-data*/}

JSX hukuruhusu kuweka markup ndani ya JavaScript. Mabano ya curly hukuruhusu "kurudi kwenye JavaScript" ili uweze kuweka kigezo kutoka kwenye msimbo wako na kukionyesha kwa mtumiaji. Kwa mfano, hii itaonyesha `user.name`:

```js {3}
return (
  <h1>
    {user.name}
  </h1>
);
```

Unaweza pia "kurudi kwenye JavaScript" kutoka kwa sifa za JSX, lakini lazima utumie mabano ya curly *badala ya* nukuu. Kwa mfano, `className="avatar"` hupitisha kamba ya `"avatar"` kama darasa la CSS, lakini `src={user.imageUrl}` husoma thamani ya kigezo cha JavaScript `user.imageUrl`, kisha hupitisha thamani hiyo kama sifa ya `src`:

```js {3,4}
return (
  <img
    className="avatar"
    src={user.imageUrl}
  />
);
```

Unaweza kuweka maonyesho magumu zaidi ndani ya mabano ya curly ya JSX pia, kwa mfano, [string concatenation](https://javascript.info/operators#string-concatenation-with-binary):

<Sandpack>

```js
const user = {
  name: 'Hedy Lamarr',
  imageUrl: 'https://i.imgur.com/yXOvdOSs.jpg',
  imageSize: 90,
};

export default function Profile() {
  return (
    <>
      <h1>{user.name}</h1>
      <img
        className="avatar"
        src={user.imageUrl}
        alt={'Photo of ' + user.name}
        style={{
          width: user.imageSize,
          height: user.imageSize
        }}
      />
    </>
  );
}
```

```css
.avatar {
  border-radius: 50%;
}

.large {
  border: 4px solid gold;
}
```

</Sandpack>

Katika mfano ulio hapo juu, `style={{}}` si sintaksia maalum, bali ni `{}` ya kawaida ya kitu (object) ndani ya mabano ya curly ya `style={ }` ya JSX. Unaweza kutumia sifa ya `style` pale ambapo mitindo yako inategemea vigezo vya JavaScript.

## Utoaji Masharti {/*conditional-rendering*/}

Katika React, hakuna sintaksia maalum ya kuandika masharti. Badala yake, utatumia mbinu zile zile unazotumia wakati wa kuandika msimbo wa kawaida wa JavaScript. Kwa mfano, unaweza kutumia kauli ya [`if`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else) ili kujumuisha JSX kwa masharti:

```js
let content;
if (isLoggedIn) {
  content = <AdminPanel />;
} else {
  content = <LoginForm />;
}
return (
  <div>
    {content}
  </div>
);
```

Ikiwa unapendelea msimbo mfupi, unaweza kutumia [opereta ya masharti `?`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator). Tofauti na `if`, inafanya function ndani ya JSX:

```js
<div>
  {isLoggedIn ? (
    <AdminPanel />
  ) : (
    <LoginForm />
  )}
</div>
```

Wakati hauhitaji tawi la `else`, unaweza pia kutumia sintaksia fupi ya [kimantiki `&&`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_AND#short-circuit_evaluation):

```js
<div>
  {isLoggedIn && <AdminPanel />}
</div>
```

Njia hizi zote pia hufanya function kwa maalumisha sifa kwa masharti. Ikiwa hujafahamiana na baadhi ya sintaksia hii ya JavaScript, unaweza kuanza kwa kila mara kutumia `if...else`.

## Utoaji wa Orodha {/*rendering-lists*/}

Utatumia vipengele vya JavaScript kama [`for` loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for) na [array `map()` function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) ili kutoa orodha za vipengele.

Kwa mfano, tuseme una safu (array) ya bidhaa:

```js
const products = [
  { title: 'Kabeji', id: 1 },
  { title: 'Kitunguu Saumu', id: 2 },
  { title: 'Tofaa', id: 3 },
];
```

Ndani ya kipengele chako, tumia function ya `map()` kubadilisha safu ya bidhaa kuwa safu ya vipengele vya `<li>`:

```js
const listItems = products.map(product =>
  <li key={product.id}>
    {product.title}
  </li>
);

return (
  <ul>{listItems}</ul>
);
```

Angalia jinsi `<li>` inavyokuwa na sifa ya `key`. Kwa kila kipengele kwenye orodha, unapaswa kupitisha kamba au namba inayotambulisha kwa kipekee kipengele hicho miongoni mwa ndugu zake. Kwa kawaida, `key` inapaswa kutoka kwenye data yako, kama vile kitambulisho cha hifadhidata. React hutumia funguo hizi kufuatilia kilichotokea ikiwa utaongeza, kufuta, au kupanga upya vipengele.

<Sandpack>

```js
const products = [
  { title: 'Cabbage', isFruit: false, id: 1 },
  { title: 'Garlic', isFruit: false, id: 2 },
  { title: 'Apple', isFruit: true, id: 3 },
];

export default function ShoppingList() {
  const listItems = products.map(product =>
    <li
      key={product.id}
      style={{
        color: product.isFruit ? 'magenta' : 'darkgreen'
      }}
    >
      {product.title}
    </li>
  );

  return (
    <ul>{listItems}</ul>
  );
}
```

</Sandpack>

## Kujibu matukio {/*responding-to-events*/}

Unaweza kujibu matukio kwa kutangaza functions za *handler za matukio* ndani ya vipengele vyako:

```js {2-4,7}
function MyButton() {
  function handleClick() {
    alert('You clicked me!');
  }

  return (
    <button onClick={handleClick}>
      Click me
    </button>
  );
}
```

Angalia jinsi `onClick={handleClick}` haina mabano mwishoni! Usiiite *event handler* moja kwa moja: unahitaji tu *kuipitisha chini*. React itaendesha `handleClick` wakati mtumiaji atabofya kitufe.

## Kusasisha skrini {/*updating-the-screen*/}

Mara nyingi, utataka kipengele chako "kikumbuke" taarifa fulani na kuionesha. Kwa mfano, labda unataka kuhesabu ni mara ngapi kitufe kimebofya. Ili kufanya hivyo, ongeza *hali* (state) kwenye kipengele chako.

Kwanza, leta [`useState`](/reference/react/useState) kutoka React:

```js
import { useState } from 'react';
```

Sasa unaweza kutangaza *variable ya hali* ndani ya kipengele chako:

```js
function MyButton() {
  const [count, setCount] = useState(0);
  // ...
```

Utapata mambo mawili kutoka kwa `useState`: hali ya sasa (`count`), na function inayokuwezesha kuibadilisha (`setCount`). Unaweza kuyapa majina yoyote, lakini desturi ni kuandika `[something, setSomething]`.

Mara ya kwanza kitufe kinapoonyeshwa, `count` itakuwa `0` kwa sababu uliipitisha `0` kwenye `useState()`. Wakati unataka kubadilisha hali, piga `setCount()` na upitishe thamani mpya kwake. Kubofya kitufe hiki kutaongeza kaunta:

```js {5}
function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      Umebofya mara {count}
    </button>
  );
}
```

React itaita tena function ya kipengele chako. Wakati huu, `count` itakuwa `1`. Kisha itakuwa `2`. Na kadhalika.

Ikiwa utaonyesha kipengele kimoja mara nyingi, kila moja kitapata hali yake. Bofya kila kitufe kando kando:

<Sandpack>

```js
import { useState } from 'react';

export default function MyApp() {
  return (
    <div>
      <h1>Counters that update separately</h1>
      <MyButton />
      <MyButton />
    </div>
  );
}

function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      Clicked {count} times
    </button>
  );
}
```

```css
button {
  display: block;
  margin-bottom: 5px;
}
```

</Sandpack>

Kumbuka jinsi kila kitufe "kinakumbuka" hali yake ya `count` na hakiathiri vitufe vingine.

## Kutumia hooks {/*using-hooks*/}

Functions zinazooanza na `use` zinaitwa *Hooks*. `useState` ni Hook ya kujengwa inayotolewa na React. Unaweza kupata Hooks nyingine zilizojengwa kwenye [marejeleo ya API.](/reference/react) Pia unaweza kuandika Hooks zako mwenyewe kwa kuchanganya zile zilizopo.

Hooks huwa restrictive kuliko functions nyingine. Unaweza tu kuita Hooks *juu* ya vipengele vyako (au Hooks nyingine). Ikiwa unataka kutumia `useState` katika hali au mzunguko, toa kijenzi kipya na kueka hapo.

## Kushirikisha data kati ya vipengele {/*sharing-data-between-components*/}

Katika mfano wa awali, kila `MyButton` ilikuwa na hali yake ya kujitegemea `count`, na kila kitufe kilipo bonyezwa, `count` ya kitufe kilichobonyezwa pekee ndiyo ilibadilika:

<DiagramGroup>

<Diagram name="sharing_data_child" height={367} width={407} alt="Picha inayoonyesha mti wa vijenzi vitatu, mzazi mmoja aliye na jina MyApp na watoto wawili waliotajwa kama MyButton. Vijenzi vyote viwili vya MyButton vina hali ya count yenye thamani sifuri.">

Awali, kila hali ya `count` ya `MyButton` ni `0`

</Diagram>

<Diagram name="sharing_data_child_clicked" height={367} width={407} alt="Picha ile ile ya awali, na hali ya `count` ya kijenzi cha kwanza cha `MyButton` ikiwa imeangaziwa ikionyesha kubonyeza na thamani ya `count` ikiwa imeongezeka hadi moja. Kijenzi cha pili cha `MyButton` bado kina thamani sifuri." >

Kijenzi cha kwanza cha `MyButton` kinasasisha `count` yake hadi `1`

</Diagram>

</DiagramGroup>

Hata hivyo, mara nyingi utahitaji vijenzi kushirikiana data na kila wakati kubadilika pamoja.

Ili kufanya vijenzi vyote viwili vya `MyButton` kuonyesha `count` sawa na kubadilika pamoja, unahitaji kuhamisha hali kutoka kwa vitufe vya kibinafsi "juu" hadi kijenzi cha karibu kinachovijumuisha vyote.

Katika mfano huu, ni `MyApp`:

<DiagramGroup>

<Diagram name="sharing_data_parent" height={385} width={410} alt="Picha inayoonyesha mti wa vijenzi vitatu, mzazi mmoja aliye na jina MyApp na watoto wawili waliotajwa kama MyButton. `MyApp` ina hali ya count yenye thamani sifuri ambayo inapasiwa kwa watoto wote wawili wa MyButton, ambao pia wanaonyesha thamani sifuri." >

Awali, hali ya `count` ya `MyApp` ni `0` na inapasiwa kwa watoto wote wawili

</Diagram>

<Diagram name="sharing_data_parent_clicked" height={385} width={410} alt="Picha ile ile ya awali, na hali ya `count` ya mzazi `MyApp` ikiwa imeangaziwa ikionyesha kubonyeza na thamani imeongezeka hadi moja. Mtiririko kwa watoto wote wawili wa `MyButton` pia umeangaziwa, na hali ya `count` katika kila mtoto imewekwa kuwa moja ikionyesha kwamba thamani ilipasiwa." >

Kwa kubonyeza, `MyApp` inasasisha hali yake ya `count` hadi `1` na kuipasi kwa watoto wote wawili

</Diagram>

</DiagramGroup>

Sasa unapobonyeza kitufe chochote, hali ya `count` katika `MyApp` itabadilika, ambayo itabadilisha hali ya `count` katika kila `MyButton`. Hivi ndivyo unavyoweza kuonyesha hili kwa msimbo.

Kwanza, *hamisha hali* kutoka `MyButton` hadi `MyApp`:

```js {2-6,18}
export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Counters that update separately</h1>
      <MyButton />
      <MyButton />
    </div>
  );
}

function MyButton() {
  // ... tunahamisha msimbo kutoka hapa ...
}

```

Kisha, *pitisha hali* kutoka `MyApp` hadi kila `MyButton`, pamoja na handler ya kubonyeza ya pamoja. Unaweza kupitishia habari kwa `MyButton` ukitumia mabano ya JSX, kama vile ulivyofanya awali na lebo zilizojengwa kama `<img>`:

```js {11-12}
export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Counters that update together</h1>
      <MyButton count={count} onClick={handleClick} />
      <MyButton count={count} onClick={handleClick} />
    </div>
  );
}
```

Habari unazopitishia hivi zinaitwa _props_. Sasa kipengele cha `MyApp` kina hali ya `count` na mshughulikiaji wa tukio `handleClick`, na *inapasia zote mbili kama props* kwa kila kitufe.

Mwisho, badilisha `MyButton` ili *kusoma* props ulizopitishia kutoka kwa mzazi wake:

```js {1,3}
function MyButton({ count, onClick }) {
  return (
    <button onClick={onClick}>
      Clicked {count} times
    </button>
  );
}
```

Unapobonyeza kitufe, handler ya `onClick` inawashwa. Prop ya `onClick` ya kila kitufe ilipangwa kwa function ya `handleClick` ndani ya `MyApp`, hivyo msimbo ndani yake unafanya function. Kazi hiyo inaita `setCount(count + 1)`, ikiongeza hali ya `count`. Thamani mpya ya `count` inapaswa kama prop kwa kila kitufe, hivyo vyote vinaonyesha thamani mpya. Hii inaitwa "kupandisha hali". Kwa kuhamisha hali juu, umeishirikisha kati ya vipengele.

<Sandpack>

```js
import { useState } from 'react';

export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Counters that update together</h1>
      <MyButton count={count} onClick={handleClick} />
      <MyButton count={count} onClick={handleClick} />
    </div>
  );
}

function MyButton({ count, onClick }) {
  return (
    <button onClick={onClick}>
      Clicked {count} times
    </button>
  );
}
```

```css
button {
  display: block;
  margin-bottom: 5px;
}
```

</Sandpack>

## Hatua Zifuatazo {/*next-steps*/}

Kwa sasa, unajua misingi ya jinsi ya kuandika msimbo wa React!

Angalia [Mafunzo](/learn/tutorial-tic-tac-toe) ili kufanya mazoezi na kujenga mini-app yako ya kwanza na React.