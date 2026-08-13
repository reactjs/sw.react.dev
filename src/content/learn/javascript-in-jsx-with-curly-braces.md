---
title: JavaScript ndani ya JSX kwa Mabano ya Curly
---

<Intro>

JSX hukuruhusu kuandika markup inayofanana na HTML ndani ya faili ya JavaScript, ikiweka mantiki ya ku-render na maudhui mahali pamoja. Wakati mwingine utahitaji kuongeza mantiki kidogo ya JavaScript au kurejelea sifa inayobadilika ndani ya markup hiyo. Katika hali hii, unaweza kutumia mabano ya curly ndani ya JSX yako kufungua dirisha kuelekea JavaScript.

</Intro>

<YouWillLearn>

* Jinsi ya kupitisha strings zenye alama za nukuu
* Jinsi ya kurejelea kigezo cha JavaScript ndani ya JSX kwa mabano ya curly
* Jinsi ya kuita function ya JavaScript ndani ya JSX kwa mabano ya curly
* Jinsi ya kutumia object ya JavaScript ndani ya JSX kwa mabano ya curly

</YouWillLearn>

## Kupitisha strings zenye alama za nukuu {/*passing-strings-with-quotes*/}

Unapotaka kupitisha sifa ya string kwa JSX, unaiweka ndani ya alama za nukuu moja au mbili:

<Sandpack>

```js
export default function Avatar() {
  return (
    <img
      className="avatar"
      src="https://react.dev/images/docs/scientists/7vQD0fPs.jpg"
      alt="Gregorio Y. Zara"
    />
  );
}
```

```css
.avatar { border-radius: 50%; height: 90px; }
```

</Sandpack>

Hapa, `"https://i.imgur.com/7vQD0fPs.jpg"` na `"Gregorio Y. Zara"` zinapitishwa kama strings.

Lakini itakuwaje kama unataka kubainisha `src` au maandishi ya `alt` kwa njia inayobadilika? Unaweza **kutumia thamani kutoka JavaScript kwa kubadilisha `"` na `"` kuwa `{` na `}`**:

<Sandpack>

```js
export default function Avatar() {
  const avatar = 'https://react.dev/images/docs/scientists/7vQD0fPs.jpg';
  const description = 'Gregorio Y. Zara';
  return (
    <img
      className="avatar"
      src={avatar}
      alt={description}
    />
  );
}
```

```css
.avatar { border-radius: 50%; height: 90px; }
```

</Sandpack>

Angalia tofauti kati ya `className="avatar"`, inayobainisha jina la class ya CSS `"avatar"` linalofanya picha kuwa duara, na `src={avatar}` inayosoma thamani ya kigezo cha JavaScript kiitwacho `avatar`. Hii ni kwa sababu mabano ya curly hukuruhusu kufanya kazi na JavaScript hapo hapo ndani ya markup yako!

## Kutumia mabano ya curly: Dirisha kuelekea dunia ya JavaScript {/*using-curly-braces-a-window-into-the-javascript-world*/}

JSX ni njia maalum ya kuandika JavaScript. Hilo linamaanisha inawezekana kutumia JavaScript ndani yake—kwa mabano ya curly `{ }`. Mfano ulio hapa chini kwanza hutangaza jina la mwanasayansi, `name`, kisha huipachika kwa mabano ya curly ndani ya `<h1>`:

<Sandpack>

```js
export default function TodoList() {
  const name = 'Gregorio Y. Zara';
  return (
    <h1>{name}'s To Do List</h1>
  );
}
```

</Sandpack>

Jaribu kubadilisha thamani ya `name` kutoka `'Gregorio Y. Zara'` kuwa `'Hedy Lamarr'`. Ona jinsi kichwa cha orodha kinavyobadilika?

Usemi wowote wa JavaScript utafanya kazi kati ya mabano ya curly, ikijumuisha miito ya function kama `formatDate()`:

<Sandpack>

```js
const today = new Date();

function formatDate(date) {
  return new Intl.DateTimeFormat(
    'en-US',
    { weekday: 'long' }
  ).format(date);
}

export default function TodoList() {
  return (
    <h1>To Do List for {formatDate(today)}</h1>
  );
}
```

</Sandpack>

### Wapi pa kutumia mabano ya curly {/*where-to-use-curly-braces*/}

Unaweza kutumia mabano ya curly kwa njia mbili tu ndani ya JSX:

1. **Kama maandishi** moja kwa moja ndani ya tagi ya JSX: `<h1>{name}'s To Do List</h1>` hufanya kazi, lakini `<{tag}>Gregorio Y. Zara's To Do List</{tag}>` haitafanya kazi.
2. **Kama sifa** mara tu baada ya alama ya `=`: `src={avatar}` itasoma kigezo `avatar`, lakini `src="{avatar}"` itapitisha string `"{avatar}"`.

## Kutumia "curly mbili": CSS na objects nyingine ndani ya JSX {/*using-double-curlies-css-and-other-objects-in-jsx*/}

Mbali na strings, namba, na semi nyingine za JavaScript, unaweza hata kupitisha objects ndani ya JSX. Objects pia zinaonyeshwa kwa mabano ya curly, kama `{ name: "Hedy Lamarr", inventions: 5 }`. Kwa hivyo, ili kupitisha object ya JS ndani ya JSX, lazima uifunge object ndani ya jozi nyingine ya mabano ya curly: `person={{ name: "Hedy Lamarr", inventions: 5 }}`.

Unaweza kuona hili kwa mitindo ya CSS ya ndani (inline) katika JSX. React haikulazimishi kutumia mitindo ya ndani (classes za CSS zinafanya kazi vizuri kwa hali nyingi). Lakini unapohitaji mtindo wa ndani, unapitisha object kwa sifa ya `style`:

<Sandpack>

```js
export default function TodoList() {
  return (
    <ul style={{
      backgroundColor: 'black',
      color: 'pink'
    }}>
      <li>Improve the videophone</li>
      <li>Prepare aeronautics lectures</li>
      <li>Work on the alcohol-fuelled engine</li>
    </ul>
  );
}
```

```css
body { padding: 0; margin: 0 }
ul { padding: 20px 20px 20px 40px; margin: 0; }
```

</Sandpack>

Jaribu kubadilisha thamani za `backgroundColor` na `color`.

Unaweza kweli kuiona object ya JavaScript ndani ya mabano ya curly unapoiandika hivi:

```js {2-5}
<ul style={
  {
    backgroundColor: 'black',
    color: 'pink'
  }
}>
```

Wakati ujao utakapoona `{{` na `}}` katika JSX, tambua kuwa si kitu zaidi ya object iliyowekwa ndani ya mabano ya curly ya JSX!

<Pitfall>

Sifa za `style` za ndani (inline) huandikwa kwa mtindo wa camelCase. Kwa mfano, HTML `<ul style="background-color: black">` ingeandikwa kama `<ul style={{ backgroundColor: 'black' }}>` katika component yako.

</Pitfall>

## Furaha zaidi na objects za JavaScript na mabano ya curly {/*more-fun-with-javascript-objects-and-curly-braces*/}

Unaweza kuhamisha semi kadhaa ndani ya object moja, na kuzirejelea katika JSX yako ndani ya mabano ya curly:

<Sandpack>

```js
const person = {
  name: 'Gregorio Y. Zara',
  theme: {
    backgroundColor: 'black',
    color: 'pink'
  }
};

export default function TodoList() {
  return (
    <div style={person.theme}>
      <h1>{person.name}'s Todos</h1>
      <img
        className="avatar"
        src="https://react.dev/images/docs/scientists/7vQD0fPs.jpg"
        alt="Gregorio Y. Zara"
      />
      <ul>
        <li>Improve the videophone</li>
        <li>Prepare aeronautics lectures</li>
        <li>Work on the alcohol-fuelled engine</li>
      </ul>
    </div>
  );
}
```

```css
body { padding: 0; margin: 0 }
body > div > div { padding: 20px; }
.avatar { border-radius: 50%; height: 90px; }
```

</Sandpack>

Katika mfano huu, object ya JavaScript `person` ina string ya `name` na object ya `theme`:

```js
const person = {
  name: 'Gregorio Y. Zara',
  theme: {
    backgroundColor: 'black',
    color: 'pink'
  }
};
```

Component inaweza kutumia thamani hizi kutoka `person` hivi:

```js
<div style={person.theme}>
  <h1>{person.name}'s Todos</h1>
```

JSX ni ndogo sana kama lugha ya templeti kwa sababu hukuruhusu kupanga data na mantiki kwa kutumia JavaScript.

<Recap>

Sasa unajua karibu kila kitu kuhusu JSX:

* Sifa za JSX zilizo ndani ya alama za nukuu hupitishwa kama strings.
* Mabano ya curly hukuruhusu kuleta mantiki na vigezo vya JavaScript ndani ya markup yako.
* Zinafanya kazi ndani ya maudhui ya tagi ya JSX au mara tu baada ya `=` katika sifa.
* `{{` na `}}` si sintaksia maalum: ni object ya JavaScript iliyowekwa ndani ya mabano ya curly ya JSX.

</Recap>

<Challenges>

#### Rekebisha kosa {/*fix-the-mistake*/}

Msimbo huu unaanguka kwa hitilafu inayosema `Objects are not valid as a React child`:

<Sandpack>

```js
const person = {
  name: 'Gregorio Y. Zara',
  theme: {
    backgroundColor: 'black',
    color: 'pink'
  }
};

export default function TodoList() {
  return (
    <div style={person.theme}>
      <h1>{person}'s Todos</h1>
      <img
        className="avatar"
        src="https://react.dev/images/docs/scientists/7vQD0fPs.jpg"
        alt="Gregorio Y. Zara"
      />
      <ul>
        <li>Improve the videophone</li>
        <li>Prepare aeronautics lectures</li>
        <li>Work on the alcohol-fuelled engine</li>
      </ul>
    </div>
  );
}
```

```css
body { padding: 0; margin: 0 }
body > div > div { padding: 20px; }
.avatar { border-radius: 50%; height: 90px; }
```

</Sandpack>

Je, unaweza kupata tatizo?

<Hint>Tafuta kilicho ndani ya mabano ya curly. Je, tunaweka kitu sahihi hapo?</Hint>

<Solution>

Hili linatokea kwa sababu mfano huu una-render *object yenyewe* ndani ya markup badala ya string: `<h1>{person}'s Todos</h1>` inajaribu ku-render object nzima ya `person`! Kujumuisha objects ghafi kama maudhui ya maandishi husababisha hitilafu kwa sababu React haijui unavyotaka kuzionyesha.

Ili kurekebisha, badilisha `<h1>{person}'s Todos</h1>` kuwa `<h1>{person.name}'s Todos</h1>`:

<Sandpack>

```js
const person = {
  name: 'Gregorio Y. Zara',
  theme: {
    backgroundColor: 'black',
    color: 'pink'
  }
};

export default function TodoList() {
  return (
    <div style={person.theme}>
      <h1>{person.name}'s Todos</h1>
      <img
        className="avatar"
        src="https://react.dev/images/docs/scientists/7vQD0fPs.jpg"
        alt="Gregorio Y. Zara"
      />
      <ul>
        <li>Improve the videophone</li>
        <li>Prepare aeronautics lectures</li>
        <li>Work on the alcohol-fuelled engine</li>
      </ul>
    </div>
  );
}
```

```css
body { padding: 0; margin: 0 }
body > div > div { padding: 20px; }
.avatar { border-radius: 50%; height: 90px; }
```

</Sandpack>

</Solution>

#### Toa taarifa ndani ya object {/*extract-information-into-an-object*/}

Toa URL ya picha na uiweke ndani ya object ya `person`.

<Sandpack>

```js
const person = {
  name: 'Gregorio Y. Zara',
  theme: {
    backgroundColor: 'black',
    color: 'pink'
  }
};

export default function TodoList() {
  return (
    <div style={person.theme}>
      <h1>{person.name}'s Todos</h1>
      <img
        className="avatar"
        src="https://react.dev/images/docs/scientists/7vQD0fPs.jpg"
        alt="Gregorio Y. Zara"
      />
      <ul>
        <li>Improve the videophone</li>
        <li>Prepare aeronautics lectures</li>
        <li>Work on the alcohol-fuelled engine</li>
      </ul>
    </div>
  );
}
```

```css
body { padding: 0; margin: 0 }
body > div > div { padding: 20px; }
.avatar { border-radius: 50%; height: 90px; }
```

</Sandpack>

<Solution>

Hamisha URL ya picha ndani ya sifa iitwayo `person.imageUrl` na uisome kutoka kwa tagi ya `<img>` kwa kutumia mabano ya curly:

<Sandpack>

```js
const person = {
  name: 'Gregorio Y. Zara',
  imageUrl: "https://react.dev/images/docs/scientists/7vQD0fPs.jpg",
  theme: {
    backgroundColor: 'black',
    color: 'pink'
  }
};

export default function TodoList() {
  return (
    <div style={person.theme}>
      <h1>{person.name}'s Todos</h1>
      <img
        className="avatar"
        src={person.imageUrl}
        alt="Gregorio Y. Zara"
      />
      <ul>
        <li>Improve the videophone</li>
        <li>Prepare aeronautics lectures</li>
        <li>Work on the alcohol-fuelled engine</li>
      </ul>
    </div>
  );
}
```

```css
body { padding: 0; margin: 0 }
body > div > div { padding: 20px; }
.avatar { border-radius: 50%; height: 90px; }
```

</Sandpack>

</Solution>

#### Andika usemi ndani ya mabano ya curly ya JSX {/*write-an-expression-inside-jsx-curly-braces*/}

Katika object iliyo hapa chini, URL kamili ya picha imegawanywa katika sehemu nne: URL msingi, `imageId`, `imageSize`, na kiendelezi cha faili.

Tunataka URL ya picha iunganishe sifa hizi pamoja: URL msingi (daima `'https://i.imgur.com/'`), `imageId` (`'7vQD0fP'`), `imageSize` (`'s'`), na kiendelezi cha faili (daima `'.jpg'`). Hata hivyo, kuna kitu hakiko sawa kuhusu jinsi tagi ya `<img>` inavyobainisha `src` yake.

Je, unaweza kuirekebisha?

<Sandpack>

```js

const baseUrl = 'https://react.dev/images/docs/scientists/';
const person = {
  name: 'Gregorio Y. Zara',
  imageId: '7vQD0fP',
  imageSize: 's',
  theme: {
    backgroundColor: 'black',
    color: 'pink'
  }
};

export default function TodoList() {
  return (
    <div style={person.theme}>
      <h1>{person.name}'s Todos</h1>
      <img
        className="avatar"
        src="{baseUrl}{person.imageId}{person.imageSize}.jpg"
        alt={person.name}
      />
      <ul>
        <li>Improve the videophone</li>
        <li>Prepare aeronautics lectures</li>
        <li>Work on the alcohol-fuelled engine</li>
      </ul>
    </div>
  );
}
```

```css
body { padding: 0; margin: 0 }
body > div > div { padding: 20px; }
.avatar { border-radius: 50%; }
```

</Sandpack>

Ili kuthibitisha kuwa marekebisho yako yamefanya kazi, jaribu kubadilisha thamani ya `imageSize` kuwa `'b'`. Picha inapaswa kubadilika ukubwa baada ya uhariri wako.

<Solution>

Unaweza kuiandika kama `src={baseUrl + person.imageId + person.imageSize + '.jpg'}`.

1. `{` hufungua usemi wa JavaScript
2. `baseUrl + person.imageId + person.imageSize + '.jpg'` huzalisha string sahihi ya URL
3. `}` hufunga usemi wa JavaScript

<Sandpack>

```js
const baseUrl = 'https://react.dev/images/docs/scientists/';
const person = {
  name: 'Gregorio Y. Zara',
  imageId: '7vQD0fP',
  imageSize: 's',
  theme: {
    backgroundColor: 'black',
    color: 'pink'
  }
};

export default function TodoList() {
  return (
    <div style={person.theme}>
      <h1>{person.name}'s Todos</h1>
      <img
        className="avatar"
        src={baseUrl + person.imageId + person.imageSize + '.jpg'}
        alt={person.name}
      />
      <ul>
        <li>Improve the videophone</li>
        <li>Prepare aeronautics lectures</li>
        <li>Work on the alcohol-fuelled engine</li>
      </ul>
    </div>
  );
}
```

```css
body { padding: 0; margin: 0 }
body > div > div { padding: 20px; }
.avatar { border-radius: 50%; }
```

</Sandpack>

Unaweza pia kuhamisha usemi huu ndani ya function tofauti kama `getImageUrl` iliyo hapa chini:

<Sandpack>

```js src/App.js
import { getImageUrl } from './utils.js'

const person = {
  name: 'Gregorio Y. Zara',
  imageId: '7vQD0fP',
  imageSize: 's',
  theme: {
    backgroundColor: 'black',
    color: 'pink'
  }
};

export default function TodoList() {
  return (
    <div style={person.theme}>
      <h1>{person.name}'s Todos</h1>
      <img
        className="avatar"
        src={getImageUrl(person)}
        alt={person.name}
      />
      <ul>
        <li>Improve the videophone</li>
        <li>Prepare aeronautics lectures</li>
        <li>Work on the alcohol-fuelled engine</li>
      </ul>
    </div>
  );
}
```

```js src/utils.js
export function getImageUrl(person) {
  return (
    'https://react.dev/images/docs/scientists/' +
    person.imageId +
    person.imageSize +
    '.jpg'
  );
}
```

```css
body { padding: 0; margin: 0 }
body > div > div { padding: 20px; }
.avatar { border-radius: 50%; }
```

</Sandpack>

Vigezo na functions vinaweza kukusaidia kuweka markup rahisi!

</Solution>

</Challenges>
