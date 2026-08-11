---
title: Kuelezea UI
---

<Intro>

React ni maktaba ya JavaScript ya ku-render violesura vya mtumiaji (UI). UI hujengwa kutokana na vipande vidogo kama vitufe, maandishi, na picha. React hukuruhusu kuviunganisha kuwa *components (vipengele)* vinavyoweza kutumika tena na kupachikwa ndani ya vingine. Kuanzia tovuti hadi programu za simu, kila kitu kwenye skrini kinaweza kugawanywa kuwa components. Katika sura hii, utajifunza kuunda, kubinafsisha, na kuonyesha components za React kwa masharti.

</Intro>

<YouWillLearn isChapter={true}>

* [Jinsi ya kuandika component yako ya kwanza ya React](/learn/your-first-component)
* [Wakati na jinsi ya kuunda faili zenye components nyingi](/learn/importing-and-exporting-components)
* [Jinsi ya kuongeza markup kwenye JavaScript kwa kutumia JSX](/learn/writing-markup-with-jsx)
* [Jinsi ya kutumia mabano ya vishazi (curly braces) na JSX kufikia utendaji wa JavaScript kutoka kwa components zako](/learn/javascript-in-jsx-with-curly-braces)
* [Jinsi ya kusanidi components kwa kutumia props](/learn/passing-props-to-a-component)
* [Jinsi ya ku-render components kwa masharti](/learn/conditional-rendering)
* [Jinsi ya ku-render components nyingi kwa wakati mmoja](/learn/rendering-lists)
* [Jinsi ya kuepuka hitilafu (bugs) zenye kutatanisha kwa kuweka components zikiwa pure](/learn/keeping-components-pure)
* [Kwa nini kuelewa UI yako kama miti (trees) ni jambo la manufaa](/learn/understanding-your-ui-as-a-tree)

</YouWillLearn>

## Component yako ya kwanza {/*your-first-component*/}

Programu za React hujengwa kutokana na vipande vilivyotengwa vya UI vinavyoitwa *components (vipengele)*. Component ya React ni function ya JavaScript ambayo unaweza kuinyunyizia markup. Components zinaweza kuwa ndogo kama kitufe, au kubwa kama ukurasa mzima. Hapa kuna component ya `Gallery` inayo-render components tatu za `Profile`:

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

<LearnMore path="/learn/your-first-component">

Soma **[Component Yako ya Kwanza](/learn/your-first-component)** ili ujifunze jinsi ya kutangaza na kutumia components za React.

</LearnMore>

## Kuingiza na kutoa components {/*importing-and-exporting-components*/}

Unaweza kutangaza components nyingi katika faili moja, lakini faili kubwa zinaweza kuwa ngumu kuzipitia. Ili kutatua hili, unaweza *export* component kwenye faili yake yenyewe, kisha *import* component hiyo kutoka faili nyingine:


<Sandpack>

```js src/App.js hidden
import Gallery from './Gallery.js';

export default function App() {
  return (
    <Gallery />
  );
}
```

```js src/Gallery.js active
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
img { margin: 0 10px 10px 0; }
```

</Sandpack>

<LearnMore path="/learn/importing-and-exporting-components">

Soma **[Kuingiza na Kutoa Components](/learn/importing-and-exporting-components)** ili ujifunze jinsi ya kugawanya components kwenye faili zao wenyewe.

</LearnMore>

## Kuandika markup kwa JSX {/*writing-markup-with-jsx*/}

Kila component ya React ni function ya JavaScript ambayo inaweza kuwa na markup fulani ambayo React hui-render kwenye kivinjari. Components za React hutumia kiendelezi cha sintaksia kinachoitwa JSX kuwakilisha markup hiyo. JSX inafanana sana na HTML, lakini ina masharti zaidi kidogo na inaweza kuonyesha taarifa zinazobadilika.

Tukibandika markup ya HTML iliyopo kwenye component ya React, haitafanya kazi kila wakati:

<Sandpack>

```js
export default function TodoList() {
  return (
    // Hii haifanyi kazi vizuri!
    <h1>Mambo ya kufanya ya Hedy Lamarr</h1>
    <img
      src="https://i.imgur.com/yXOvdOSs.jpg"
      alt="Hedy Lamarr"
      class="photo"
    >
    <ul>
      <li>Buni taa mpya za barabarani
      <li>Fanya mazoezi ya onyesho la filamu
      <li>Boresha teknolojia ya spektramu
    </ul>
  );
}
```

```css
img { height: 90px; }
```

</Sandpack>

Ikiwa una HTML iliyopo kama hii, unaweza kuirekebisha kwa kutumia [kigeuzi (converter)](https://transform.tools/html-to-jsx):

<Sandpack>

```js
export default function TodoList() {
  return (
    <>
      <h1>Mambo ya kufanya ya Hedy Lamarr</h1>
      <img
        src="https://i.imgur.com/yXOvdOSs.jpg"
        alt="Hedy Lamarr"
        className="photo"
      />
      <ul>
        <li>Buni taa mpya za barabarani</li>
        <li>Fanya mazoezi ya onyesho la filamu</li>
        <li>Boresha teknolojia ya spektramu</li>
      </ul>
    </>
  );
}
```

```css
img { height: 90px; }
```

</Sandpack>

<LearnMore path="/learn/writing-markup-with-jsx">

Soma **[Kuandika Markup kwa JSX](/learn/writing-markup-with-jsx)** ili ujifunze jinsi ya kuandika JSX halali.

</LearnMore>

## JavaScript ndani ya JSX kwa mabano ya vishazi {/*javascript-in-jsx-with-curly-braces*/}

JSX hukuruhusu kuandika markup inayofanana na HTML ndani ya faili ya JavaScript, ikiweka mantiki ya ku-render na maudhui mahali pamoja. Wakati mwingine utataka kuongeza mantiki kidogo ya JavaScript au kurejelea sifa inayobadilika ndani ya markup hiyo. Katika hali hii, unaweza kutumia mabano ya vishazi (curly braces) katika JSX yako ili "kufungua dirisha" kuelekea JavaScript:

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
      <h1>Mambo ya kufanya ya {person.name}</h1>
      <img
        className="avatar"
        src="https://i.imgur.com/7vQD0fPs.jpg"
        alt="Gregorio Y. Zara"
      />
      <ul>
        <li>Boresha simu ya video</li>
        <li>Andaa mihadhara ya anga</li>
        <li>Fanya kazi kwenye injini inayotumia alkoholi</li>
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

<LearnMore path="/learn/javascript-in-jsx-with-curly-braces">

Soma **[JavaScript ndani ya JSX kwa Mabano ya Vishazi](/learn/javascript-in-jsx-with-curly-braces)** ili ujifunze jinsi ya kufikia data ya JavaScript kutoka JSX.

</LearnMore>

## Kupitisha props kwa component {/*passing-props-to-a-component*/}

Components za React hutumia *props* kuwasiliana kati yao. Kila component-mzazi inaweza kupitisha taarifa fulani kwa components-watoto wake kwa kuwapa props. Props zinaweza kukukumbusha sifa za HTML, lakini unaweza kupitisha thamani yoyote ya JavaScript kupitia kwao, ikiwa ni pamoja na objects, arrays, functions, na hata JSX!

<Sandpack>

```js
import { getImageUrl } from './utils.js'

export default function Profile() {
  return (
    <Card>
      <Avatar
        size={100}
        person={{
          name: 'Katsuko Saruhashi',
          imageId: 'YfeOqp2'
        }}
      />
    </Card>
  );
}

function Avatar({ person, size }) {
  return (
    <img
      className="avatar"
      src={getImageUrl(person)}
      alt={person.name}
      width={size}
      height={size}
    />
  );
}

function Card({ children }) {
  return (
    <div className="card">
      {children}
    </div>
  );
}

```

```js src/utils.js
export function getImageUrl(person, size = 's') {
  return (
    'https://i.imgur.com/' +
    person.imageId +
    size +
    '.jpg'
  );
}
```

```css
.card {
  width: fit-content;
  margin: 5px;
  padding: 5px;
  font-size: 20px;
  text-align: center;
  border: 1px solid #aaa;
  border-radius: 20px;
  background: #fff;
}
.avatar {
  margin: 20px;
  border-radius: 50%;
}
```

</Sandpack>

<LearnMore path="/learn/passing-props-to-a-component">

Soma **[Kupitisha Props kwa Component](/learn/passing-props-to-a-component)** ili ujifunze jinsi ya kupitisha na kusoma props.

</LearnMore>

## Ku-render kwa masharti {/*conditional-rendering*/}

Mara nyingi components zako zitahitaji kuonyesha vitu tofauti kulingana na masharti tofauti. Katika React, unaweza ku-render JSX kwa masharti kwa kutumia sintaksia ya JavaScript kama vile kauli za `if`, `&&`, na opereta za `? :`.

Katika mfano huu, opereta ya `&&` ya JavaScript inatumika ku-render alama ya tiki kwa masharti:

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

<LearnMore path="/learn/conditional-rendering">

Soma **[Ku-render kwa Masharti](/learn/conditional-rendering)** ili ujifunze njia tofauti za ku-render maudhui kwa masharti.

</LearnMore>

## Ku-render orodha {/*rendering-lists*/}

Mara nyingi utataka kuonyesha components nyingi zinazofanana kutoka kwenye mkusanyiko wa data. Unaweza kutumia `filter()` na `map()` za JavaScript pamoja na React kuchuja na kubadilisha array yako ya data kuwa array ya components.

Kwa kila kipengee cha array, utahitaji kubainisha `key`. Kwa kawaida, utataka kutumia ID kutoka kwenye hifadhidata kama `key`. Keys huruhusu React kufuatilia nafasi ya kila kipengee kwenye orodha hata kama orodha itabadilika.

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
      <h1>Wanasayansi</h1>
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
    'https://i.imgur.com/' +
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
  grid-template-columns: 1fr 1fr;
  align-items: center;
}
img { width: 100px; height: 100px; border-radius: 50%; }
h1 { font-size: 22px; }
h2 { font-size: 20px; }
```

</Sandpack>

<LearnMore path="/learn/rendering-lists">

Soma **[Ku-render Orodha](/learn/rendering-lists)** ili ujifunze jinsi ya ku-render orodha ya components, na jinsi ya kuchagua key.

</LearnMore>

## Kuweka components zikiwa pure {/*keeping-components-pure*/}

Baadhi ya functions za JavaScript ni *pure (safi).* Function iliyo pure:

* **Hujishughulisha na mambo yake yenyewe.** Haibadilishi objects au vigezo vyovyote vilivyokuwepo kabla haijaitwa.
* **Ingizo sawa, matokeo sawa.** Ikipewa maingizo sawa, function iliyo pure inapaswa kurudisha matokeo yaleyale kila wakati.

Kwa kuandika components zako kama functions pure pekee, unaweza kuepuka kundi zima la hitilafu (bugs) zenye kutatanisha na tabia zisizotabirika kadri msimbo wako unavyokua. Hapa kuna mfano wa component isiyo pure:

<Sandpack>

```js {expectedErrors: {'react-compiler': [5]}}
let guest = 0;

function Cup() {
  // Baya: kubadilisha kigezo kilichokuwepo awali!
  guest = guest + 1;
  return <h2>Kikombe cha chai kwa mgeni #{guest}</h2>;
}

export default function TeaSet() {
  return (
    <>
      <Cup />
      <Cup />
      <Cup />
    </>
  );
}
```

</Sandpack>

Unaweza kuifanya component hii kuwa pure kwa kupitisha prop badala ya kurekebisha kigezo kilichokuwepo awali:

<Sandpack>

```js
function Cup({ guest }) {
  return <h2>Kikombe cha chai kwa mgeni #{guest}</h2>;
}

export default function TeaSet() {
  return (
    <>
      <Cup guest={1} />
      <Cup guest={2} />
      <Cup guest={3} />
    </>
  );
}
```

</Sandpack>

<LearnMore path="/learn/keeping-components-pure">

Soma **[Kuweka Components Zikiwa Pure](/learn/keeping-components-pure)** ili ujifunze jinsi ya kuandika components kama functions pure, zinazotabirika.

</LearnMore>

## UI yako kama mti {/*your-ui-as-a-tree*/}

React hutumia miti (trees) kuiga uhusiano kati ya components na modules.

Render tree ya React ni uwakilishi wa uhusiano wa mzazi na mtoto kati ya components.

<Diagram name="generic_render_tree" height={250} width={500} alt="A tree graph with five nodes, with each node representing a component. The root node is located at the top the tree graph and is labelled 'Root Component'. It has two arrows extending down to two nodes labelled 'Component A' and 'Component C'. Each of the arrows is labelled with 'renders'. 'Component A' has a single 'renders' arrow to a node labelled 'Component B'. 'Component C' has a single 'renders' arrow to a node labelled 'Component D'.">

Mfano wa render tree ya React.

</Diagram>

Components zilizo karibu na kilele cha mti, karibu na component-mzizi (root), huchukuliwa kama components za ngazi ya juu. Components zisizo na components-watoto ni components za majani (leaf). Uainishaji huu wa components ni wa manufaa kwa kuelewa mtiririko wa data na utendaji wa ku-render.

Kuiga uhusiano kati ya modules za JavaScript ni njia nyingine ya manufaa ya kuelewa programu yako. Tunauita mti wa utegemezi wa modules (module dependency tree).

<Diagram name="generic_dependency_tree" height={250} width={500} alt="A tree graph with five nodes. Each node represents a JavaScript module. The top-most node is labelled 'RootModule.js'. It has three arrows extending to the nodes: 'ModuleA.js', 'ModuleB.js', and 'ModuleC.js'. Each arrow is labelled as 'imports'. 'ModuleC.js' node has a single 'imports' arrow that points to a node labelled 'ModuleD.js'.">

Mfano wa mti wa utegemezi wa modules.

</Diagram>

Mti wa utegemezi mara nyingi hutumiwa na zana za kujenga (build tools) kuunganisha msimbo wote muhimu wa JavaScript ili mteja aupakue na kuu-render. Ukubwa mkubwa wa bundle hudhoofisha uzoefu wa mtumiaji kwa programu za React. Kuelewa mti wa utegemezi wa modules ni jambo la manufaa kutatua hitilafu za aina hiyo.

<LearnMore path="/learn/understanding-your-ui-as-a-tree">

Soma **[UI Yako kama Mti](/learn/understanding-your-ui-as-a-tree)** ili ujifunze jinsi ya kuunda render tree na module dependency tree kwa programu ya React na jinsi zinavyokuwa mifano bora ya kifikra ya kuboresha uzoefu wa mtumiaji na utendaji.

</LearnMore>


## Kinachofuata? {/*whats-next*/}

Elekea kwenye [Component Yako ya Kwanza](/learn/your-first-component) ili uanze kusoma sura hii ukurasa baada ya ukurasa!

Au, ikiwa tayari unazifahamu mada hizi, kwa nini usisome kuhusu [Kuongeza Mwingiliano](/learn/adding-interactivity)?
