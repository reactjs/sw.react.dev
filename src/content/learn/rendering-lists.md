---
title: Ku-render Orodha
---

<Intro>

Mara nyingi utataka kuonyesha components (vipengele) kadhaa zinazofanana kutoka kwenye mkusanyiko wa data. Unaweza kutumia [mbinu za array za JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array#) kuchakata array ya data. Kwenye ukurasa huu, utatumia [`filter()`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) na [`map()`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array/map) pamoja na React ili kuchuja na kubadilisha array yako ya data kuwa array ya components.

</Intro>

<YouWillLearn>

* Jinsi ya ku-render components kutoka kwenye array ukitumia `map()` ya JavaScript
* Jinsi ya ku-render components mahususi tu ukitumia `filter()` ya JavaScript
* Wakati na sababu ya kutumia keys za React

</YouWillLearn>

## Ku-render data kutoka kwenye arrays {/*rendering-data-from-arrays*/}

Tuseme una orodha ya maudhui.

```js
<ul>
  <li>Creola Katherine Johnson: mathematician</li>
  <li>Mario José Molina-Pasquel Henríquez: chemist</li>
  <li>Mohammad Abdus Salam: physicist</li>
  <li>Percy Lavon Julian: chemist</li>
  <li>Subrahmanyan Chandrasekhar: astrophysicist</li>
</ul>
```

Tofauti pekee kati ya vipengele hivyo vya orodha ni maudhui yao, data yao. Mara nyingi utahitaji kuonyesha nakala kadhaa za component ileile ukitumia data tofauti wakati unajenga violesura: kutoka orodha za maoni hadi maghala ya picha za profaili. Katika hali hizi, unaweza kuhifadhi data hiyo katika objects na arrays za JavaScript na kutumia mbinu kama [`map()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) na [`filter()`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) ili ku-render orodha za components kutoka humo.

Huu hapa ni mfano mfupi wa jinsi ya kuzalisha orodha ya vipengele kutoka kwenye array:

1. **Hamisha** data hiyo kwenye array:

```js
const people = [
  'Creola Katherine Johnson: mathematician',
  'Mario José Molina-Pasquel Henríquez: chemist',
  'Mohammad Abdus Salam: physicist',
  'Percy Lavon Julian: chemist',
  'Subrahmanyan Chandrasekhar: astrophysicist'
];
```

2. **Map** wanachama wa `people` kuwa array mpya ya vipengele vya JSX, `listItems`:

```js
const listItems = people.map(person => <li>{person}</li>);
```

3. **Rudisha** `listItems` kutoka kwenye component yako ikiwa imefungwa ndani ya `<ul>`:

```js
return <ul>{listItems}</ul>;
```

Huu hapa ni matokeo:

<Sandpack>

```js
const people = [
  'Creola Katherine Johnson: mathematician',
  'Mario José Molina-Pasquel Henríquez: chemist',
  'Mohammad Abdus Salam: physicist',
  'Percy Lavon Julian: chemist',
  'Subrahmanyan Chandrasekhar: astrophysicist'
];

export default function List() {
  const listItems = people.map(person =>
    <li>{person}</li>
  );
  return <ul>{listItems}</ul>;
}
```

```css
li { margin-bottom: 10px; }
```

</Sandpack>

Angalia kuwa sanduku la mchanga (sandbox) hapo juu linaonyesha hitilafu ya console:

<ConsoleBlock level="error">

Warning: Each child in a list should have a unique "key" prop.

</ConsoleBlock>

Utajifunza jinsi ya kurekebisha hitilafu hii baadaye kwenye ukurasa huu. Kabla hatujafika hapo, hebu tuongeze muundo fulani kwenye data yako.

## Kuchuja arrays za vipengele {/*filtering-arrays-of-items*/}

Data hii inaweza kupangwa kwa muundo zaidi.

```js
const people = [{
  id: 0,
  name: 'Creola Katherine Johnson',
  profession: 'mathematician',
}, {
  id: 1,
  name: 'Mario José Molina-Pasquel Henríquez',
  profession: 'chemist',
}, {
  id: 2,
  name: 'Mohammad Abdus Salam',
  profession: 'physicist',
}, {
  id: 3,
  name: 'Percy Lavon Julian',
  profession: 'chemist',
}, {
  id: 4,
  name: 'Subrahmanyan Chandrasekhar',
  profession: 'astrophysicist',
}];
```

Tuseme unataka njia ya kuonyesha watu tu ambao taaluma yao ni `'chemist'`. Unaweza kutumia mbinu ya `filter()` ya JavaScript kurudisha watu hao tu. Mbinu hii huchukua array ya vipengele, huvipitisha kupitia "jaribio" (function inayorudisha `true` au `false`), na hurudisha array mpya ya vipengele vile tu vilivyofaulu jaribio (vilivyorudisha `true`).

Unataka tu vile vipengele ambavyo `profession` yake ni `'chemist'`. Function ya "jaribio" ya hili inaonekana kama `(person) => person.profession === 'chemist'`. Hivi ndivyo ya kuiunganisha pamoja:

1. **Tengeneza** array mpya ya watu "chemist" tu, `chemists`, kwa kuita `filter()` kwenye `people` ukichuja kwa `person.profession === 'chemist'`:

```js
const chemists = people.filter(person =>
  person.profession === 'chemist'
);
```

2. Sasa **map** juu ya `chemists`:

```js {1,13}
const listItems = chemists.map(person =>
  <li>
     <img
       src={getImageUrl(person)}
       alt={person.name}
     />
     <p>
       <b>{person.name}:</b>
       {' ' + person.profession + ' '}
       maarufu kwa {person.accomplishment}
     </p>
  </li>
);
```

3. Mwisho, **rudisha** `listItems` kutoka kwenye component yako:

```js
return <ul>{listItems}</ul>;
```

<Sandpack>

```js src/App.js
import { people } from './data.js';
import { getImageUrl } from './utils.js';

export default function List() {
  const chemists = people.filter(person =>
    person.profession === 'chemist'
  );
  const listItems = chemists.map(person =>
    <li>
      <img
        src={getImageUrl(person)}
        alt={person.name}
      />
      <p>
        <b>{person.name}:</b>
        {' ' + person.profession + ' '}
        maarufu kwa {person.accomplishment}
      </p>
    </li>
  );
  return <ul>{listItems}</ul>;
}
```

```js src/data.js
export const people = [{
  id: 0,
  name: 'Creola Katherine Johnson',
  profession: 'mathematician',
  accomplishment: 'spaceflight calculations',
  imageId: 'MK3eW3A'
}, {
  id: 1,
  name: 'Mario José Molina-Pasquel Henríquez',
  profession: 'chemist',
  accomplishment: 'discovery of Arctic ozone hole',
  imageId: 'mynHUSa'
}, {
  id: 2,
  name: 'Mohammad Abdus Salam',
  profession: 'physicist',
  accomplishment: 'electromagnetism theory',
  imageId: 'bE7W1ji'
}, {
  id: 3,
  name: 'Percy Lavon Julian',
  profession: 'chemist',
  accomplishment: 'pioneering cortisone drugs, steroids and birth control pills',
  imageId: 'IOjWm71'
}, {
  id: 4,
  name: 'Subrahmanyan Chandrasekhar',
  profession: 'astrophysicist',
  accomplishment: 'white dwarf star mass calculations',
  imageId: 'lrWQx8l'
}];
```

```js src/utils.js
export function getImageUrl(person) {
  return (
    'https://react.dev/images/docs/scientists/' +
    person.imageId +
    's.jpg'
  );
}
```

```css
ul { list-style-type: none; padding: 0px 10px; }
li {
  margin-bottom: 10px;
  display: grid;
  grid-template-columns: auto 1fr;
  gap: 20px;
  align-items: center;
}
img { width: 100px; height: 100px; border-radius: 50%; }
```

</Sandpack>

<Pitfall>

Arrow functions hurudisha kwa siri expression iliyo mara tu baada ya `=>`, hivyo hukuhitaji kauli ya `return`:

```js
const listItems = chemists.map(person =>
  <li>...</li> // Kurudisha kwa siri!
);
```

Hata hivyo, **lazima uandike `return` waziwazi ikiwa `=>` yako inafuatwa na kufunga kwa mabano ya curly `{`!**

```js
const listItems = chemists.map(person => { // Mabano ya curly
  return <li>...</li>;
});
```

Arrow functions zenye `=> {` husemekana kuwa na ["block body".](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions#function_body) Hukuruhusu kuandika zaidi ya mstari mmoja wa msimbo, lakini *lazima* uandike kauli ya `return` mwenyewe. Ukiisahau, hakuna kitakachorudishwa!

</Pitfall>

## Kuweka vipengele vya orodha katika mpangilio kwa `key` {/*keeping-list-items-in-order-with-key*/}

Angalia kuwa masanduku yote ya mchanga hapo juu yanaonyesha hitilafu kwenye console:

<ConsoleBlock level="error">

Warning: Each child in a list should have a unique "key" prop.

</ConsoleBlock>

Unahitaji kumpa kila kipengele cha array `key` -- mfululizo wa herufi au nambari inayokitambulisha kwa upekee miongoni mwa vipengele vingine katika array hiyo:

```js
<li key={person.id}>...</li>
```

<Note>

Elementi za JSX zilizo moja kwa moja ndani ya wito wa `map()` daima zinahitaji keys!

</Note>

Keys huiambia React ni kipengele kipi cha array kila component kinacholingana nacho, ili iweze kuzilinganisha baadaye. Hili huwa muhimu ikiwa vipengele vya array yako vinaweza kuhama (mfano kutokana na kupanga), kuingizwa, au kufutwa. `key` iliyochaguliwa vizuri humsaidia React kubaini kwa hakika kilichotokea, na kufanya masasisho sahihi kwenye mti (tree) wa DOM.

Badala ya kuzalisha keys papo hapo, unapaswa kuzijumuisha ndani ya data yako:

<Sandpack>

```js src/App.js
import { people } from './data.js';
import { getImageUrl } from './utils.js';

export default function List() {
  const listItems = people.map(person =>
    <li key={person.id}>
      <img
        src={getImageUrl(person)}
        alt={person.name}
      />
      <p>
        <b>{person.name}</b>
          {' ' + person.profession + ' '}
          maarufu kwa {person.accomplishment}
      </p>
    </li>
  );
  return <ul>{listItems}</ul>;
}
```

```js src/data.js active
export const people = [{
  id: 0, // Inatumika katika JSX kama key
  name: 'Creola Katherine Johnson',
  profession: 'mathematician',
  accomplishment: 'spaceflight calculations',
  imageId: 'MK3eW3A'
}, {
  id: 1, // Inatumika katika JSX kama key
  name: 'Mario José Molina-Pasquel Henríquez',
  profession: 'chemist',
  accomplishment: 'discovery of Arctic ozone hole',
  imageId: 'mynHUSa'
}, {
  id: 2, // Inatumika katika JSX kama key
  name: 'Mohammad Abdus Salam',
  profession: 'physicist',
  accomplishment: 'electromagnetism theory',
  imageId: 'bE7W1ji'
}, {
  id: 3, // Inatumika katika JSX kama key
  name: 'Percy Lavon Julian',
  profession: 'chemist',
  accomplishment: 'pioneering cortisone drugs, steroids and birth control pills',
  imageId: 'IOjWm71'
}, {
  id: 4, // Inatumika katika JSX kama key
  name: 'Subrahmanyan Chandrasekhar',
  profession: 'astrophysicist',
  accomplishment: 'white dwarf star mass calculations',
  imageId: 'lrWQx8l'
}];
```

```js src/utils.js
export function getImageUrl(person) {
  return (
    'https://react.dev/images/docs/scientists/' +
    person.imageId +
    's.jpg'
  );
}
```

```css
ul { list-style-type: none; padding: 0px 10px; }
li {
  margin-bottom: 10px;
  display: grid;
  grid-template-columns: auto 1fr;
  gap: 20px;
  align-items: center;
}
img { width: 100px; height: 100px; border-radius: 50%; }
```

</Sandpack>

<DeepDive>

#### Kuonyesha nodi kadhaa za DOM kwa kila kipengele cha orodha {/*displaying-several-dom-nodes-for-each-list-item*/}

Unafanyaje wakati kila kipengele kinahitaji ku-render si nodi moja ya DOM, bali kadhaa?

Sintaksia fupi ya [Fragment `<>...</>`](/reference/react/Fragment) haitakuruhusu kupitisha key, hivyo unahitaji ama kuvikusanya ndani ya `<div>` moja, au kutumia [sintaksia ndefu kidogo na wazi zaidi ya `<Fragment>`:](/reference/react/Fragment#rendering-a-list-of-fragments)

```js
import { Fragment } from 'react';

// ...

const listItems = people.map(person =>
  <Fragment key={person.id}>
    <h1>{person.name}</h1>
    <p>{person.bio}</p>
  </Fragment>
);
```

Fragments hutoweka kutoka kwenye DOM, hivyo hii itazalisha orodha bapa ya `<h1>`, `<p>`, `<h1>`, `<p>`, na kadhalika.

</DeepDive>

### Wapi pa kupata `key` yako {/*where-to-get-your-key*/}

Vyanzo tofauti vya data hutoa vyanzo tofauti vya keys:

* **Data kutoka kwenye database:** Ikiwa data yako inatoka kwenye database, unaweza kutumia keys/IDs za database, ambazo ni za kipekee kwa asili yake.
* **Data iliyozalishwa kienyeji:** Ikiwa data yako imezalishwa na kuhifadhiwa kienyeji (mfano madokezo katika programu ya kuchukua madokezo), tumia kihesabu kinachoongezeka, [`crypto.randomUUID()`](https://developer.mozilla.org/en-US/docs/Web/API/Crypto/randomUUID) au kifurushi kama [`uuid`](https://www.npmjs.com/package/uuid) wakati wa kutengeneza vipengele.

### Kanuni za keys {/*rules-of-keys*/}

* **Keys lazima ziwe za kipekee miongoni mwa ndugu.** Hata hivyo, ni sawa kutumia keys zilezile kwa nodi za JSX katika arrays _tofauti_.
* **Keys hazipaswi kubadilika** ama hilo linaharibu kusudi lao! Usizizalishe wakati wa ku-render.

### Kwa nini React inahitaji keys? {/*why-does-react-need-keys*/}

Fikiria kwamba faili kwenye desktop yako hazikuwa na majina. Badala yake, ungezirejelea kwa mpangilio wao -- faili ya kwanza, faili ya pili, na kadhalika. Ungeweza kuzoea, lakini mara tu unapofuta faili, ingekuwa jambo la kutatanisha. Faili ya pili ingekuwa faili ya kwanza, faili ya tatu ingekuwa faili ya pili, na kadhalika.

Majina ya faili katika folda na keys za JSX katika array hutimiza kusudi linalofanana. Hutuwezesha kukitambulisha kipengele kwa upekee kati ya ndugu zake. `key` iliyochaguliwa vizuri hutoa taarifa zaidi kuliko nafasi ndani ya array. Hata kama _nafasi_ itabadilika kutokana na kupanga upya, `key` humwezesha React kukitambua kipengele katika maisha yake yote.

<Pitfall>

Huenda ukashawishika kutumia index ya kipengele katika array kama key yake. Kwa kweli, hicho ndicho React itatumia ikiwa hutabainisha `key` hata kidogo. Lakini mpangilio ambao unavyo-render vipengele utabadilika baada ya muda ikiwa kipengele kitaingizwa, kufutwa, au ikiwa array itapangwa upya. Index kama key mara nyingi husababisha hitilafu (bugs) za hila na za kutatanisha.

Vivyo hivyo, usizalishe keys papo hapo, mfano kwa `key={Math.random()}`. Hii itasababisha keys kutolingana kamwe kati ya renders, ikipelekea components zako zote na DOM kuundwa upya kila mara. Si tu kwamba hili ni la polepole, bali pia litapoteza input yoyote ya mtumiaji ndani ya vipengele vya orodha. Badala yake, tumia ID thabiti inayotegemea data.

Kumbuka kuwa components zako hazitapokea `key` kama prop. Inatumika tu kama kidokezo na React yenyewe. Ikiwa component yako inahitaji ID, lazima uipitishe kama prop tofauti: `<Profile key={id} userId={id} />`.

</Pitfall>

<Recap>

Kwenye ukurasa huu ulijifunza:

* Jinsi ya kuhamisha data nje ya components na kuiweka katika miundo ya data kama arrays na objects.
* Jinsi ya kuzalisha seti za components zinazofanana kwa `map()` ya JavaScript.
* Jinsi ya kutengeneza arrays za vipengele vilivyochujwa kwa `filter()` ya JavaScript.
* Kwa nini na jinsi ya kuweka `key` kwenye kila component katika mkusanyiko ili React iweze kufuatilia kila mojawapo hata kama nafasi au data yao itabadilika.

</Recap>



<Challenges>

#### Kugawanya orodha kuwa mbili {/*splitting-a-list-in-two*/}

Mfano huu unaonyesha orodha ya watu wote.

Ubadilishe ili uonyeshe orodha mbili tofauti moja baada ya nyingine: **Chemists** na **Everyone Else.** Kama ilivyo awali, unaweza kubaini iwapo mtu ni chemist kwa kuangalia kama `person.profession === 'chemist'`.

<Sandpack>

```js src/App.js
import { people } from './data.js';
import { getImageUrl } from './utils.js';

export default function List() {
  const listItems = people.map(person =>
    <li key={person.id}>
      <img
        src={getImageUrl(person)}
        alt={person.name}
      />
      <p>
        <b>{person.name}:</b>
        {' ' + person.profession + ' '}
        maarufu kwa {person.accomplishment}
      </p>
    </li>
  );
  return (
    <article>
      <h1>Scientists</h1>
      <ul>{listItems}</ul>
    </article>
  );
}
```

```js src/data.js
export const people = [{
  id: 0,
  name: 'Creola Katherine Johnson',
  profession: 'mathematician',
  accomplishment: 'spaceflight calculations',
  imageId: 'MK3eW3A'
}, {
  id: 1,
  name: 'Mario José Molina-Pasquel Henríquez',
  profession: 'chemist',
  accomplishment: 'discovery of Arctic ozone hole',
  imageId: 'mynHUSa'
}, {
  id: 2,
  name: 'Mohammad Abdus Salam',
  profession: 'physicist',
  accomplishment: 'electromagnetism theory',
  imageId: 'bE7W1ji'
}, {
  id: 3,
  name: 'Percy Lavon Julian',
  profession: 'chemist',
  accomplishment: 'pioneering cortisone drugs, steroids and birth control pills',
  imageId: 'IOjWm71'
}, {
  id: 4,
  name: 'Subrahmanyan Chandrasekhar',
  profession: 'astrophysicist',
  accomplishment: 'white dwarf star mass calculations',
  imageId: 'lrWQx8l'
}];
```

```js src/utils.js
export function getImageUrl(person) {
  return (
    'https://react.dev/images/docs/scientists/' +
    person.imageId +
    's.jpg'
  );
}
```

```css
ul { list-style-type: none; padding: 0px 10px; }
li {
  margin-bottom: 10px;
  display: grid;
  grid-template-columns: auto 1fr;
  gap: 20px;
  align-items: center;
}
img { width: 100px; height: 100px; border-radius: 50%; }
```

</Sandpack>

<Solution>

Unaweza kutumia `filter()` mara mbili, ukitengeneza arrays mbili tofauti, kisha `map` juu ya zote mbili:

<Sandpack>

```js src/App.js
import { people } from './data.js';
import { getImageUrl } from './utils.js';

export default function List() {
  const chemists = people.filter(person =>
    person.profession === 'chemist'
  );
  const everyoneElse = people.filter(person =>
    person.profession !== 'chemist'
  );
  return (
    <article>
      <h1>Scientists</h1>
      <h2>Chemists</h2>
      <ul>
        {chemists.map(person =>
          <li key={person.id}>
            <img
              src={getImageUrl(person)}
              alt={person.name}
            />
            <p>
              <b>{person.name}:</b>
              {' ' + person.profession + ' '}
              known for {person.accomplishment}
            </p>
          </li>
        )}
      </ul>
      <h2>Everyone Else</h2>
      <ul>
        {everyoneElse.map(person =>
          <li key={person.id}>
            <img
              src={getImageUrl(person)}
              alt={person.name}
            />
            <p>
              <b>{person.name}:</b>
              {' ' + person.profession + ' '}
              known for {person.accomplishment}
            </p>
          </li>
        )}
      </ul>
    </article>
  );
}
```

```js src/data.js
export const people = [{
  id: 0,
  name: 'Creola Katherine Johnson',
  profession: 'mathematician',
  accomplishment: 'spaceflight calculations',
  imageId: 'MK3eW3A'
}, {
  id: 1,
  name: 'Mario José Molina-Pasquel Henríquez',
  profession: 'chemist',
  accomplishment: 'discovery of Arctic ozone hole',
  imageId: 'mynHUSa'
}, {
  id: 2,
  name: 'Mohammad Abdus Salam',
  profession: 'physicist',
  accomplishment: 'electromagnetism theory',
  imageId: 'bE7W1ji'
}, {
  id: 3,
  name: 'Percy Lavon Julian',
  profession: 'chemist',
  accomplishment: 'pioneering cortisone drugs, steroids and birth control pills',
  imageId: 'IOjWm71'
}, {
  id: 4,
  name: 'Subrahmanyan Chandrasekhar',
  profession: 'astrophysicist',
  accomplishment: 'white dwarf star mass calculations',
  imageId: 'lrWQx8l'
}];
```

```js src/utils.js
export function getImageUrl(person) {
  return (
    'https://react.dev/images/docs/scientists/' +
    person.imageId +
    's.jpg'
  );
}
```

```css
ul { list-style-type: none; padding: 0px 10px; }
li {
  margin-bottom: 10px;
  display: grid;
  grid-template-columns: auto 1fr;
  gap: 20px;
  align-items: center;
}
img { width: 100px; height: 100px; border-radius: 50%; }
```

</Sandpack>

Katika suluhisho hili, wito wa `map` umewekwa moja kwa moja ndani ya elementi za mzazi `<ul>`, lakini ungeweza kuanzisha vigezo kwa ajili yao kama utaona hilo linasomeka vizuri zaidi.

Bado kuna urudufu kidogo kati ya orodha zilizo-render. Unaweza kwenda mbali zaidi na kutoa sehemu zinazojirudia kuwa component ya `<ListSection>`:

<Sandpack>

```js src/App.js
import { people } from './data.js';
import { getImageUrl } from './utils.js';

function ListSection({ title, people }) {
  return (
    <>
      <h2>{title}</h2>
      <ul>
        {people.map(person =>
          <li key={person.id}>
            <img
              src={getImageUrl(person)}
              alt={person.name}
            />
            <p>
              <b>{person.name}:</b>
              {' ' + person.profession + ' '}
              known for {person.accomplishment}
            </p>
          </li>
        )}
      </ul>
    </>
  );
}

export default function List() {
  const chemists = people.filter(person =>
    person.profession === 'chemist'
  );
  const everyoneElse = people.filter(person =>
    person.profession !== 'chemist'
  );
  return (
    <article>
      <h1>Scientists</h1>
      <ListSection
        title="Chemists"
        people={chemists}
      />
      <ListSection
        title="Everyone Else"
        people={everyoneElse}
      />
    </article>
  );
}
```

```js src/data.js
export const people = [{
  id: 0,
  name: 'Creola Katherine Johnson',
  profession: 'mathematician',
  accomplishment: 'spaceflight calculations',
  imageId: 'MK3eW3A'
}, {
  id: 1,
  name: 'Mario José Molina-Pasquel Henríquez',
  profession: 'chemist',
  accomplishment: 'discovery of Arctic ozone hole',
  imageId: 'mynHUSa'
}, {
  id: 2,
  name: 'Mohammad Abdus Salam',
  profession: 'physicist',
  accomplishment: 'electromagnetism theory',
  imageId: 'bE7W1ji'
}, {
  id: 3,
  name: 'Percy Lavon Julian',
  profession: 'chemist',
  accomplishment: 'pioneering cortisone drugs, steroids and birth control pills',
  imageId: 'IOjWm71'
}, {
  id: 4,
  name: 'Subrahmanyan Chandrasekhar',
  profession: 'astrophysicist',
  accomplishment: 'white dwarf star mass calculations',
  imageId: 'lrWQx8l'
}];
```

```js src/utils.js
export function getImageUrl(person) {
  return (
    'https://react.dev/images/docs/scientists/' +
    person.imageId +
    's.jpg'
  );
}
```

```css
ul { list-style-type: none; padding: 0px 10px; }
li {
  margin-bottom: 10px;
  display: grid;
  grid-template-columns: auto 1fr;
  gap: 20px;
  align-items: center;
}
img { width: 100px; height: 100px; border-radius: 50%; }
```

</Sandpack>

Msomaji makini sana anaweza kutambua kuwa kwa wito wa `filter` mara mbili, tunaangalia taaluma ya kila mtu mara mbili. Kuangalia sifa (property) ni haraka sana, hivyo katika mfano huu ni sawa. Ikiwa mantiki yako ingekuwa ghali zaidi ya hivyo, ungeweza kubadilisha wito wa `filter` na kitanzi kinachounda arrays kwa mkono na kuangalia kila mtu mara moja tu.

Kwa hakika, ikiwa `people` haibadiliki kamwe, ungeweza kuhamisha msimbo huu nje ya component yako. Kwa mtazamo wa React, kinachohesabika ni kwamba unaipa array ya nodi za JSX mwishoni. Haijali jinsi unavyozalisha array hiyo:

<Sandpack>

```js src/App.js
import { people } from './data.js';
import { getImageUrl } from './utils.js';

let chemists = [];
let everyoneElse = [];
people.forEach(person => {
  if (person.profession === 'chemist') {
    chemists.push(person);
  } else {
    everyoneElse.push(person);
  }
});

function ListSection({ title, people }) {
  return (
    <>
      <h2>{title}</h2>
      <ul>
        {people.map(person =>
          <li key={person.id}>
            <img
              src={getImageUrl(person)}
              alt={person.name}
            />
            <p>
              <b>{person.name}:</b>
              {' ' + person.profession + ' '}
              known for {person.accomplishment}
            </p>
          </li>
        )}
      </ul>
    </>
  );
}

export default function List() {
  return (
    <article>
      <h1>Scientists</h1>
      <ListSection
        title="Chemists"
        people={chemists}
      />
      <ListSection
        title="Everyone Else"
        people={everyoneElse}
      />
    </article>
  );
}
```

```js src/data.js
export const people = [{
  id: 0,
  name: 'Creola Katherine Johnson',
  profession: 'mathematician',
  accomplishment: 'spaceflight calculations',
  imageId: 'MK3eW3A'
}, {
  id: 1,
  name: 'Mario José Molina-Pasquel Henríquez',
  profession: 'chemist',
  accomplishment: 'discovery of Arctic ozone hole',
  imageId: 'mynHUSa'
}, {
  id: 2,
  name: 'Mohammad Abdus Salam',
  profession: 'physicist',
  accomplishment: 'electromagnetism theory',
  imageId: 'bE7W1ji'
}, {
  id: 3,
  name: 'Percy Lavon Julian',
  profession: 'chemist',
  accomplishment: 'pioneering cortisone drugs, steroids and birth control pills',
  imageId: 'IOjWm71'
}, {
  id: 4,
  name: 'Subrahmanyan Chandrasekhar',
  profession: 'astrophysicist',
  accomplishment: 'white dwarf star mass calculations',
  imageId: 'lrWQx8l'
}];
```

```js src/utils.js
export function getImageUrl(person) {
  return (
    'https://react.dev/images/docs/scientists/' +
    person.imageId +
    's.jpg'
  );
}
```

```css
ul { list-style-type: none; padding: 0px 10px; }
li {
  margin-bottom: 10px;
  display: grid;
  grid-template-columns: auto 1fr;
  gap: 20px;
  align-items: center;
}
img { width: 100px; height: 100px; border-radius: 50%; }
```

</Sandpack>

</Solution>

#### Orodha zilizopachikwa katika component moja {/*nested-lists-in-one-component*/}

Tengeneza orodha ya mapishi kutoka kwenye array hii! Kwa kila pishi katika array, onyesha jina lake kama `<h2>` na orodhesha viungo vyake katika `<ul>`.

<Hint>

Hili litahitaji kupachika wito wa `map` mbili tofauti.

</Hint>

<Sandpack>

```js src/App.js
import { recipes } from './data.js';

export default function RecipeList() {
  return (
    <div>
      <h1>Recipes</h1>
    </div>
  );
}
```

```js src/data.js
export const recipes = [{
  id: 'greek-salad',
  name: 'Greek Salad',
  ingredients: ['tomatoes', 'cucumber', 'onion', 'olives', 'feta']
}, {
  id: 'hawaiian-pizza',
  name: 'Hawaiian Pizza',
  ingredients: ['pizza crust', 'pizza sauce', 'mozzarella', 'ham', 'pineapple']
}, {
  id: 'hummus',
  name: 'Hummus',
  ingredients: ['chickpeas', 'olive oil', 'garlic cloves', 'lemon', 'tahini']
}];
```

</Sandpack>

<Solution>

Hii hapa ni njia moja ambayo ungeweza kufanya hivyo:

<Sandpack>

```js src/App.js
import { recipes } from './data.js';

export default function RecipeList() {
  return (
    <div>
      <h1>Recipes</h1>
      {recipes.map(recipe =>
        <div key={recipe.id}>
          <h2>{recipe.name}</h2>
          <ul>
            {recipe.ingredients.map(ingredient =>
              <li key={ingredient}>
                {ingredient}
              </li>
            )}
          </ul>
        </div>
      )}
    </div>
  );
}
```

```js src/data.js
export const recipes = [{
  id: 'greek-salad',
  name: 'Greek Salad',
  ingredients: ['tomatoes', 'cucumber', 'onion', 'olives', 'feta']
}, {
  id: 'hawaiian-pizza',
  name: 'Hawaiian Pizza',
  ingredients: ['pizza crust', 'pizza sauce', 'mozzarella', 'ham', 'pineapple']
}, {
  id: 'hummus',
  name: 'Hummus',
  ingredients: ['chickpeas', 'olive oil', 'garlic cloves', 'lemon', 'tahini']
}];
```

</Sandpack>

Kila mojawapo ya `recipes` tayari inajumuisha uga wa `id`, hivyo hicho ndicho kitanzi cha nje kinachotumia kwa `key` yake. Hakuna ID ambayo ungeweza kutumia kuzunguka viungo. Hata hivyo, ni jambo la kimantiki kudhani kwamba kiungo kilekile hakitaorodheshwa mara mbili ndani ya pishi lilelile, hivyo jina lake linaweza kutumika kama `key`. Vinginevyo, ungeweza kubadilisha muundo wa data kuongeza IDs, au kutumia index kama `key` (kwa tahadhari kwamba huwezi kupanga upya viungo kwa usalama).

</Solution>

#### Kutoa component ya kipengele cha orodha {/*extracting-a-list-item-component*/}

Component hii ya `RecipeList` ina wito wa `map` mbili zilizopachikwa. Ili kuirahisisha, toa component ya `Recipe` kutoka kwake ambayo itakubali props za `id`, `name`, na `ingredients`. Unaweka `key` ya nje wapi na kwa nini?

<Sandpack>

```js src/App.js
import { recipes } from './data.js';

export default function RecipeList() {
  return (
    <div>
      <h1>Recipes</h1>
      {recipes.map(recipe =>
        <div key={recipe.id}>
          <h2>{recipe.name}</h2>
          <ul>
            {recipe.ingredients.map(ingredient =>
              <li key={ingredient}>
                {ingredient}
              </li>
            )}
          </ul>
        </div>
      )}
    </div>
  );
}
```

```js src/data.js
export const recipes = [{
  id: 'greek-salad',
  name: 'Greek Salad',
  ingredients: ['tomatoes', 'cucumber', 'onion', 'olives', 'feta']
}, {
  id: 'hawaiian-pizza',
  name: 'Hawaiian Pizza',
  ingredients: ['pizza crust', 'pizza sauce', 'mozzarella', 'ham', 'pineapple']
}, {
  id: 'hummus',
  name: 'Hummus',
  ingredients: ['chickpeas', 'olive oil', 'garlic cloves', 'lemon', 'tahini']
}];
```

</Sandpack>

<Solution>

Unaweza kunakili-na-kubandika JSX kutoka kwenye `map` ya nje kwenye component mpya ya `Recipe` na kurudisha JSX hiyo. Kisha unaweza kubadilisha `recipe.name` kuwa `name`, `recipe.id` kuwa `id`, na kadhalika, na kuzipitisha kama props kwa `Recipe`:

<Sandpack>

```js
import { recipes } from './data.js';

function Recipe({ id, name, ingredients }) {
  return (
    <div>
      <h2>{name}</h2>
      <ul>
        {ingredients.map(ingredient =>
          <li key={ingredient}>
            {ingredient}
          </li>
        )}
      </ul>
    </div>
  );
}

export default function RecipeList() {
  return (
    <div>
      <h1>Recipes</h1>
      {recipes.map(recipe =>
        <Recipe {...recipe} key={recipe.id} />
      )}
    </div>
  );
}
```

```js src/data.js
export const recipes = [{
  id: 'greek-salad',
  name: 'Greek Salad',
  ingredients: ['tomatoes', 'cucumber', 'onion', 'olives', 'feta']
}, {
  id: 'hawaiian-pizza',
  name: 'Hawaiian Pizza',
  ingredients: ['pizza crust', 'pizza sauce', 'mozzarella', 'ham', 'pineapple']
}, {
  id: 'hummus',
  name: 'Hummus',
  ingredients: ['chickpeas', 'olive oil', 'garlic cloves', 'lemon', 'tahini']
}];
```

</Sandpack>

Hapa, `<Recipe {...recipe} key={recipe.id} />` ni njia fupi ya sintaksia inayosema "pitisha sifa zote za object ya `recipe` kama props kwa component ya `Recipe`". Ungeweza pia kuandika kila prop waziwazi: `<Recipe id={recipe.id} name={recipe.name} ingredients={recipe.ingredients} key={recipe.id} />`.

**Angalia kuwa `key` imebainishwa kwenye `<Recipe>` yenyewe badala ya kwenye `<div>` ya mzizi (root) inayorudishwa kutoka `Recipe`.** Hii ni kwa sababu `key` hii inahitajika moja kwa moja ndani ya muktadha wa array inayozunguka. Awali, ulikuwa na array ya `<div>`s hivyo kila mmoja ulihitaji `key`, lakini sasa una array ya `<Recipe>`s. Kwa maneno mengine, unapotoa component, usisahau kuacha `key` nje ya JSX unayonakili na kubandika.

</Solution>

#### Orodha yenye kitenganishi {/*list-with-a-separator*/}

Mfano huu una-render haiku maarufu ya Tachibana Hokushi, kila mstari ukiwa umefungwa ndani ya tagi ya `<p>`. Kazi yako ni kuingiza kitenganishi `<hr />` kati ya kila aya. Muundo wako wa matokeo unapaswa kuonekana hivi:

```js
<article>
  <p>I write, erase, rewrite</p>
  <hr />
  <p>Erase again, and then</p>
  <hr />
  <p>A poppy blooms.</p>
</article>
```

Haiku ina mistari mitatu tu, lakini suluhisho lako linapaswa kufanya kazi kwa idadi yoyote ya mistari. Angalia kuwa elementi za `<hr />` zinaonekana *kati* ya elementi za `<p>` tu, si mwanzoni au mwishoni!

<Sandpack>

```js
const poem = {
  lines: [
    'I write, erase, rewrite',
    'Erase again, and then',
    'A poppy blooms.'
  ]
};

export default function Poem() {
  return (
    <article>
      {poem.lines.map((line, index) =>
        <p key={index}>
          {line}
        </p>
      )}
    </article>
  );
}
```

```css
body {
  text-align: center;
}
p {
  font-family: Georgia, serif;
  font-size: 20px;
  font-style: italic;
}
hr {
  margin: 0 120px 0 120px;
  border: 1px dashed #45c3d8;
}
```

</Sandpack>

(Hii ni hali nadra ambapo index kama key inakubalika kwa sababu mistari ya shairi haitapangwa upya kamwe.)

<Hint>

Utahitaji ama kubadilisha `map` kuwa kitanzi cha mkono, au kutumia Fragment.

</Hint>

<Solution>

Unaweza kuandika kitanzi cha mkono, ukiingiza `<hr />` na `<p>...</p>` kwenye array ya matokeo unapoendelea:

<Sandpack>

```js
const poem = {
  lines: [
    'I write, erase, rewrite',
    'Erase again, and then',
    'A poppy blooms.'
  ]
};

export default function Poem() {
  let output = [];

  // Jaza array ya matokeo
  poem.lines.forEach((line, i) => {
    output.push(
      <hr key={i + '-separator'} />
    );
    output.push(
      <p key={i + '-text'}>
        {line}
      </p>
    );
  });
  // Ondoa <hr /> ya kwanza
  output.shift();

  return (
    <article>
      {output}
    </article>
  );
}
```

```css
body {
  text-align: center;
}
p {
  font-family: Georgia, serif;
  font-size: 20px;
  font-style: italic;
}
hr {
  margin: 0 120px 0 120px;
  border: 1px dashed #45c3d8;
}
```

</Sandpack>

Kutumia index halisi ya mstari kama `key` hakufanyi kazi tena kwa sababu kila kitenganishi na aya sasa ziko katika array ileile. Hata hivyo, unaweza kumpa kila mmoja key tofauti ukitumia kiambishi tamati, mfano `key={i + '-text'}`.

Vinginevyo, ungeweza ku-render mkusanyiko wa Fragments zinazo `<hr />` na `<p>...</p>`. Hata hivyo, sintaksia fupi ya `<>...</>` haiungi mkono kupitisha keys, hivyo ungelazimika kuandika `<Fragment>` waziwazi:

<Sandpack>

```js
import { Fragment } from 'react';

const poem = {
  lines: [
    'I write, erase, rewrite',
    'Erase again, and then',
    'A poppy blooms.'
  ]
};

export default function Poem() {
  return (
    <article>
      {poem.lines.map((line, i) =>
        <Fragment key={i}>
          {i > 0 && <hr />}
          <p>{line}</p>
        </Fragment>
      )}
    </article>
  );
}
```

```css
body {
  text-align: center;
}
p {
  font-family: Georgia, serif;
  font-size: 20px;
  font-style: italic;
}
hr {
  margin: 0 120px 0 120px;
  border: 1px dashed #45c3d8;
}
```

</Sandpack>

Kumbuka, Fragments (mara nyingi zikiandikwa kama `<> </>`) hukuwezesha kukusanya nodi za JSX bila kuongeza `<div>`s za ziada!

</Solution>

</Challenges>
