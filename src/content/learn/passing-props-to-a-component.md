---
title: Kupitisha Props kwa Component
---

<Intro>

Components (vipengele) za React hutumia *props* kuwasiliana na kila mmoja. Kila component-mzazi inaweza kupitisha taarifa fulani kwa components zake za mtoto kwa kuzipa props. Props zinaweza kukukumbusha sifa za HTML, lakini unaweza kupitisha thamani yoyote ya JavaScript kupitia kwao, ikijumuisha objects, arrays (safu), na functions.

</Intro>

<YouWillLearn>

* Jinsi ya kupitisha props kwa component
* Jinsi ya kusoma props kutoka kwa component
* Jinsi ya kubainisha thamani chaguo-msingi kwa props
* Jinsi ya kupitisha JSX fulani kwa component
* Jinsi props zinavyobadilika baada ya muda

</YouWillLearn>

## Props zinazofahamika {/*familiar-props*/}

Props ni taarifa unazopitisha kwa tagi ya JSX. Kwa mfano, `className`, `src`, `alt`, `width`, na `height` ni baadhi ya props unazoweza kupitisha kwa `<img>`:

<Sandpack>

```js
function Avatar() {
  return (
    <img
      className="avatar"
      src="https://react.dev/images/docs/scientists/1bX5QH6.jpg"
      alt="Lin Lanying"
      width={100}
      height={100}
    />
  );
}

export default function Profile() {
  return (
    <Avatar />
  );
}
```

```css
body { min-height: 120px; }
.avatar { margin: 20px; border-radius: 50%; }
```

</Sandpack>

Props unazoweza kupitisha kwa tagi ya `<img>` zimebainishwa mapema (ReactDOM inafuata [kiwango cha HTML](https://www.w3.org/TR/html52/semantics-embedded-content.html#the-img-element)). Lakini unaweza kupitisha props zozote kwa *components zako mwenyewe*, kama vile `<Avatar>`, ili kuzibinafsisha. Hivi ndivyo unavyofanya!

## Kupitisha props kwa component {/*passing-props-to-a-component*/}

Katika msimbo huu, component ya `Profile` haipitishi props zozote kwa component yake ya mtoto, `Avatar`:

```js
export default function Profile() {
  return (
    <Avatar />
  );
}
```

Unaweza kuipa `Avatar` props fulani kwa hatua mbili.

### Hatua ya 1: Pitisha props kwa component-mtoto {/*step-1-pass-props-to-the-child-component*/}

Kwanza, pitisha props fulani kwa `Avatar`. Kwa mfano, hebu tupitishe props mbili: `person` (object), na `size` (namba):

```js
export default function Profile() {
  return (
    <Avatar
      person={{ name: 'Lin Lanying', imageId: '1bX5QH6' }}
      size={100}
    />
  );
}
```

<Note>

Kama mabano-viungo mawili baada ya `person=` yanakuchanganya, kumbuka kuwa [ni object tu](/learn/javascript-in-jsx-with-curly-braces#using-double-curlies-css-and-other-objects-in-jsx) ndani ya mabano-viungo ya JSX.

</Note>

Sasa unaweza kusoma props hizi ndani ya component ya `Avatar`.

### Hatua ya 2: Soma props ndani ya component-mtoto {/*step-2-read-props-inside-the-child-component*/}

Unaweza kusoma props hizi kwa kuorodhesha majina yao `person, size` yakitenganishwa na koma ndani ya `({` na `})` mara tu baada ya `function Avatar`. Hili hukuruhusu kuzitumia ndani ya msimbo wa `Avatar`, kama ambavyo ungefanya na kigezo.

```js
function Avatar({ person, size }) {
  // person na size zinapatikana hapa
}
```

Ongeza mantiki fulani kwa `Avatar` inayotumia props za `person` na `size` kwa ku-render, nawe umemaliza.

Sasa unaweza kusanidi `Avatar` ili i-render kwa njia nyingi tofauti kwa props tofauti. Jaribu kubadilisha thamani!

<Sandpack>

```js src/App.js
import { getImageUrl } from './utils.js';

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

export default function Profile() {
  return (
    <div>
      <Avatar
        size={100}
        person={{
          name: 'Katsuko Saruhashi',
          imageId: 'YfeOqp2'
        }}
      />
      <Avatar
        size={80}
        person={{
          name: 'Aklilu Lemma',
          imageId: 'OKS67lh'
        }}
      />
      <Avatar
        size={50}
        person={{
          name: 'Lin Lanying',
          imageId: '1bX5QH6'
        }}
      />
    </div>
  );
}
```

```js src/utils.js
export function getImageUrl(person, size = 's') {
  return (
    'https://react.dev/images/docs/scientists/' +
    person.imageId +
    size +
    '.jpg'
  );
}
```

```css
body { min-height: 120px; }
.avatar { margin: 10px; border-radius: 50%; }
```

</Sandpack>

Props hukuruhusu kufikiria kuhusu components za mzazi na mtoto kwa kujitegemea. Kwa mfano, unaweza kubadilisha props za `person` au `size` ndani ya `Profile` bila kulazimika kufikiria jinsi `Avatar` inavyozitumia. Vivyo hivyo, unaweza kubadilisha jinsi `Avatar` inavyotumia props hizi, bila kuangalia `Profile`.

Unaweza kufikiria props kama "vifundo" (knobs) unavyoweza kurekebisha. Hutimiza jukumu lilelile ambalo hoja (arguments) hutimiza kwa functions—kwa hakika, props _ndicho_ pekee kilicho hoja kwa component yako! Functions za components za React hukubali hoja moja tu, object ya `props`:

```js
function Avatar(props) {
  let person = props.person;
  let size = props.size;
  // ...
}
```

Kwa kawaida hauhitaji object nzima ya `props` yenyewe, hivyo unaipasua (destructure) kuwa props mahususi.

<Pitfall>

**Usisahau jozi ya mabano-viungo `{` na `}`** ndani ya `(` na `)` unapotangaza props:

```js
function Avatar({ person, size }) {
  // ...
}
```

Sintaksia hii inaitwa ["destructuring"](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Unpacking_fields_from_objects_passed_as_a_function_parameter) na inalingana na kusoma sifa kutoka kwa kigezo cha function:

```js
function Avatar(props) {
  let person = props.person;
  let size = props.size;
  // ...
}
```

</Pitfall>

## Kubainisha thamani chaguo-msingi kwa prop {/*specifying-a-default-value-for-a-prop*/}

Kama unataka kuipa prop thamani chaguo-msingi ya kurejea pale ambapo hakuna thamani iliyobainishwa, unaweza kufanya hivyo kwa destructuring kwa kuweka `=` na thamani chaguo-msingi mara tu baada ya kigezo:

```js
function Avatar({ person, size = 100 }) {
  // ...
}
```

Sasa, kama `<Avatar person={...} />` ita-render bila prop ya `size`, `size` itawekwa kuwa `100`.

Thamani chaguo-msingi hutumika tu kama prop ya `size` haipo au kama unapitisha `size={undefined}`. Lakini kama unapitisha `size={null}` au `size={0}`, thamani chaguo-msingi **haitatumika**.

## Kupeleka props kwa sintaksia ya JSX spread {/*forwarding-props-with-the-jsx-spread-syntax*/}

Wakati mwingine, kupitisha props kunakuwa na marudio mengi sana:

```js
function Profile({ person, size, isSepia, thickBorder }) {
  return (
    <div className="card">
      <Avatar
        person={person}
        size={size}
        isSepia={isSepia}
        thickBorder={thickBorder}
      />
    </div>
  );
}
```

Hakuna ubaya wowote kwa msimbo wenye marudio—unaweza kuwa unaosomeka zaidi. Lakini wakati mwingine unaweza kuthamini ufupi. Baadhi ya components hupeleka props zao zote kwa watoto wao, kama ambavyo `Profile` hii inavyofanya na `Avatar`. Kwa sababu hazitumii yoyote kati ya props zao moja kwa moja, inaweza kuwa na maana kutumia sintaksia fupi zaidi ya "spread":

```js
function Profile(props) {
  return (
    <div className="card">
      <Avatar {...props} />
    </div>
  );
}
```

Hili hupeleka props zote za `Profile` kwa `Avatar` bila kuorodhesha kila jina lao.

**Tumia sintaksia ya spread kwa kiasi.** Kama unaitumia kwenye kila component nyingine, kuna kitu hakiko sawa. Mara nyingi, hili huashiria kuwa unapaswa kugawanya components zako na kupitisha children kama JSX. Zaidi kuhusu hilo baadaye!

## Kupitisha JSX kama children {/*passing-jsx-as-children*/}

Ni jambo la kawaida kupachika tagi za kivinjari zilizojengwa ndani:

```js
<div>
  <img />
</div>
```

Wakati mwingine utataka kupachika components zako mwenyewe kwa njia ileile:

```js
<Card>
  <Avatar />
</Card>
```

Unapopachika maudhui ndani ya tagi ya JSX, component-mzazi itapokea maudhui hayo katika prop iitwayo `children`. Kwa mfano, component ya `Card` iliyo hapa chini itapokea prop ya `children` iliyowekwa kuwa `<Avatar />` na kui-render ndani ya div ya kufunika:

<Sandpack>

```js src/App.js
import Avatar from './Avatar.js';

function Card({ children }) {
  return (
    <div className="card">
      {children}
    </div>
  );
}

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
```

```js src/Avatar.js
import { getImageUrl } from './utils.js';

export default function Avatar({ person, size }) {
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
```

```js src/utils.js
export function getImageUrl(person, size = 's') {
  return (
    'https://react.dev/images/docs/scientists/' +
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

Jaribu kubadilisha `<Avatar>` iliyo ndani ya `<Card>` na maandishi fulani ili kuona jinsi component ya `Card` inavyoweza kufunika maudhui yoyote yaliyopachikwa. Haihitaji "kujua" kile kinachо-render ndani yake. Utaona mpangilio huu unaonyumbulika katika sehemu nyingi.

Unaweza kufikiria component yenye prop ya `children` kama iliyo na "shimo" linaloweza "kujazwa" na components zake za mzazi kwa JSX yoyote. Mara nyingi utatumia prop ya `children` kwa vifuniko vya kuonekana: paneli, gridi, n.k.

<Illustration src="/images/docs/illustrations/i_children-prop.png" alt='A puzzle-like Card tile with a slot for "children" pieces like text and Avatar' />

## Jinsi props zinavyobadilika baada ya muda {/*how-props-change-over-time*/}

Component ya `Clock` iliyo hapa chini hupokea props mbili kutoka kwa component yake ya mzazi: `color` na `time`. (Msimbo wa component-mzazi umeachwa kwa sababu unatumia [state (hali)](/learn/state-a-components-memory), ambayo hatutaingia ndani yake bado.)

Jaribu kubadilisha rangi kwenye sanduku la kuchagua lililo hapa chini:

<Sandpack>

```js src/Clock.js active
export default function Clock({ color, time }) {
  return (
    <h1 style={{ color: color }}>
      {time}
    </h1>
  );
}
```

```js src/App.js hidden
import { useState, useEffect } from 'react';
import Clock from './Clock.js';

function useTime() {
  const [time, setTime] = useState(() => new Date());
  useEffect(() => {
    const id = setInterval(() => {
      setTime(new Date());
    }, 1000);
    return () => clearInterval(id);
  }, []);
  return time;
}

export default function App() {
  const time = useTime();
  const [color, setColor] = useState('lightcoral');
  return (
    <div>
      <p>
        Chagua rangi:{' '}
        <select value={color} onChange={e => setColor(e.target.value)}>
          <option value="lightcoral">lightcoral</option>
          <option value="midnightblue">midnightblue</option>
          <option value="rebeccapurple">rebeccapurple</option>
        </select>
      </p>
      <Clock color={color} time={time.toLocaleTimeString()} />
    </div>
  );
}
```

</Sandpack>

Mfano huu unaonyesha kuwa **component inaweza kupokea props tofauti baada ya muda.** Props si tuli kila wakati! Hapa, prop ya `time` hubadilika kila sekunde, na prop ya `color` hubadilika unapochagua rangi nyingine. Props huakisi data ya component wakati wowote, badala ya mwanzoni tu.

Hata hivyo, props ni [immutable](https://en.wikipedia.org/wiki/Immutable_object)—neno kutoka sayansi ya kompyuta linalomaanisha "isiyoweza kubadilika". Wakati component inahitaji kubadilisha props zake (kwa mfano, kama jibu kwa mwingiliano wa mtumiaji au data mpya), itabidi "iiombe" component yake ya mzazi ipitishe _props tofauti_—object mpya! Props zake za zamani kisha zitatupwa kando, na hatimaye injini ya JavaScript itarudisha kumbukumbu iliyochukuliwa nazo.

**Usijaribu "kubadilisha props".** Unapohitaji kujibu mwingiliano wa mtumiaji (kama kubadilisha rangi iliyochaguliwa), utahitaji "set state", ambayo unaweza kujifunza kuihusu katika [State: Kumbukumbu ya Component.](/learn/state-a-components-memory)

<Recap>

* Kupitisha props, ziongeze kwenye JSX, kama ambavyo ungefanya na sifa za HTML.
* Kusoma props, tumia sintaksia ya destructuring ya `function Avatar({ person, size })`.
* Unaweza kubainisha thamani chaguo-msingi kama `size = 100`, ambayo hutumika kwa props zisizopo na za `undefined`.
* Unaweza kupeleka props zote kwa sintaksia ya JSX spread ya `<Avatar {...props} />`, lakini usiitumie kupita kiasi!
* JSX iliyopachikwa kama `<Card><Avatar /></Card>` itaonekana kama prop ya `children` ya component ya `Card`.
* Props ni picha za mnepo za kusoma-tu (read-only) kwa wakati fulani: kila render hupokea toleo jipya la props.
* Huwezi kubadilisha props. Unapohitaji mwingiliano, utahitaji kuweka state.

</Recap>



<Challenges>

#### Toa component {/*extract-a-component*/}

Component hii ya `Gallery` ina markup inayofanana sana kwa profaili mbili. Toa component ya `Profile` kutoka kwake ili kupunguza marudio. Utahitaji kuchagua props gani za kuipitisha.

<Sandpack>

```js src/App.js
import { getImageUrl } from './utils.js';

export default function Gallery() {
  return (
    <div>
      <h1>Wanasayansi Maarufu</h1>
      <section className="profile">
        <h2>Maria Skłodowska-Curie</h2>
        <img
          className="avatar"
          src={getImageUrl('szV5sdG')}
          alt="Maria Skłodowska-Curie"
          width={70}
          height={70}
        />
        <ul>
          <li>
            <b>Taaluma: </b>
            mwanafizikia na mwanakemia
          </li>
          <li>
            <b>Tuzo: 4 </b>
            (Tuzo ya Nobel ya Fizikia, Tuzo ya Nobel ya Kemia, Nishani ya Davy, Nishani ya Matteucci)
          </li>
          <li>
            <b>Aligundua: </b>
            polonium (elementi ya kemikali)
          </li>
        </ul>
      </section>
      <section className="profile">
        <h2>Katsuko Saruhashi</h2>
        <img
          className="avatar"
          src={getImageUrl('YfeOqp2')}
          alt="Katsuko Saruhashi"
          width={70}
          height={70}
        />
        <ul>
          <li>
            <b>Taaluma: </b>
            mwanajiokemia
          </li>
          <li>
            <b>Tuzo: 2 </b>
            (Tuzo ya Miyake ya jiokemia, Tuzo ya Tanaka)
          </li>
          <li>
            <b>Aligundua: </b>
            mbinu ya kupima kaboni dioksidi katika maji ya bahari
          </li>
        </ul>
      </section>
    </div>
  );
}
```

```js src/utils.js
export function getImageUrl(imageId, size = 's') {
  return (
    'https://react.dev/images/docs/scientists/' +
    imageId +
    size +
    '.jpg'
  );
}
```

```css
.avatar { margin: 5px; border-radius: 50%; min-height: 70px; }
.profile {
  border: 1px solid #aaa;
  border-radius: 6px;
  margin-top: 20px;
  padding: 10px;
}
h1, h2 { margin: 5px; }
h1 { margin-bottom: 10px; }
ul { padding: 0px 10px 0px 20px; }
li { margin: 5px; }
```

</Sandpack>

<Hint>

Anza kwa kutoa markup ya mmoja wa wanasayansi. Kisha tafuta vipande visivyolingana nayo katika mfano wa pili, na uvifanye vinavyoweza kusanidiwa kwa props.

</Hint>

<Solution>

Katika suluhisho hili, component ya `Profile` inakubali props kadhaa: `imageId` (string), `name` (string), `profession` (string), `awards` (array ya strings), `discovery` (string), na `imageSize` (namba).

Kumbuka kuwa prop ya `imageSize` ina thamani chaguo-msingi, ndiyo maana hatuipitishi kwa component.

<Sandpack>

```js src/App.js
import { getImageUrl } from './utils.js';

function Profile({
  imageId,
  name,
  profession,
  awards,
  discovery,
  imageSize = 70
}) {
  return (
    <section className="profile">
      <h2>{name}</h2>
      <img
        className="avatar"
        src={getImageUrl(imageId)}
        alt={name}
        width={imageSize}
        height={imageSize}
      />
      <ul>
        <li><b>Taaluma:</b> {profession}</li>
        <li>
          <b>Tuzo: {awards.length} </b>
          ({awards.join(', ')})
        </li>
        <li>
          <b>Aligundua: </b>
          {discovery}
        </li>
      </ul>
    </section>
  );
}

export default function Gallery() {
  return (
    <div>
      <h1>Wanasayansi Maarufu</h1>
      <Profile
        imageId="szV5sdG"
        name="Maria Skłodowska-Curie"
        profession="physicist and chemist"
        discovery="polonium (chemical element)"
        awards={[
          'Nobel Prize in Physics',
          'Nobel Prize in Chemistry',
          'Davy Medal',
          'Matteucci Medal'
        ]}
      />
      <Profile
        imageId='YfeOqp2'
        name='Katsuko Saruhashi'
        profession='geochemist'
        discovery="a method for measuring carbon dioxide in seawater"
        awards={[
          'Miyake Prize for geochemistry',
          'Tanaka Prize'
        ]}
      />
    </div>
  );
}
```

```js src/utils.js
export function getImageUrl(imageId, size = 's') {
  return (
    'https://react.dev/images/docs/scientists/' +
    imageId +
    size +
    '.jpg'
  );
}
```

```css
.avatar { margin: 5px; border-radius: 50%; min-height: 70px; }
.profile {
  border: 1px solid #aaa;
  border-radius: 6px;
  margin-top: 20px;
  padding: 10px;
}
h1, h2 { margin: 5px; }
h1 { margin-bottom: 10px; }
ul { padding: 0px 10px 0px 20px; }
li { margin: 5px; }
```

</Sandpack>

Angalia jinsi usivyohitaji prop tofauti ya `awardCount` kama `awards` ni array. Kisha unaweza kutumia `awards.length` kuhesabu idadi ya tuzo. Kumbuka kuwa props zinaweza kuchukua thamani zozote, na hilo linajumuisha arrays pia!

Suluhisho lingine, ambalo linafanana zaidi na mifano ya awali kwenye ukurasa huu, ni kuweka pamoja taarifa zote kuhusu mtu katika object moja, na kupitisha object hiyo kama prop moja:

<Sandpack>

```js src/App.js
import { getImageUrl } from './utils.js';

function Profile({ person, imageSize = 70 }) {
  const imageSrc = getImageUrl(person)

  return (
    <section className="profile">
      <h2>{person.name}</h2>
      <img
        className="avatar"
        src={imageSrc}
        alt={person.name}
        width={imageSize}
        height={imageSize}
      />
      <ul>
        <li>
          <b>Taaluma:</b> {person.profession}
        </li>
        <li>
          <b>Tuzo: {person.awards.length} </b>
          ({person.awards.join(', ')})
        </li>
        <li>
          <b>Aligundua: </b>
          {person.discovery}
        </li>
      </ul>
    </section>
  )
}

export default function Gallery() {
  return (
    <div>
      <h1>Wanasayansi Maarufu</h1>
      <Profile person={{
        imageId: 'szV5sdG',
        name: 'Maria Skłodowska-Curie',
        profession: 'physicist and chemist',
        discovery: 'polonium (chemical element)',
        awards: [
          'Nobel Prize in Physics',
          'Nobel Prize in Chemistry',
          'Davy Medal',
          'Matteucci Medal'
        ],
      }} />
      <Profile person={{
        imageId: 'YfeOqp2',
        name: 'Katsuko Saruhashi',
        profession: 'geochemist',
        discovery: 'a method for measuring carbon dioxide in seawater',
        awards: [
          'Miyake Prize for geochemistry',
          'Tanaka Prize'
        ],
      }} />
    </div>
  );
}
```

```js src/utils.js
export function getImageUrl(person, size = 's') {
  return (
    'https://react.dev/images/docs/scientists/' +
    person.imageId +
    size +
    '.jpg'
  );
}
```

```css
.avatar { margin: 5px; border-radius: 50%; min-height: 70px; }
.profile {
  border: 1px solid #aaa;
  border-radius: 6px;
  margin-top: 20px;
  padding: 10px;
}
h1, h2 { margin: 5px; }
h1 { margin-bottom: 10px; }
ul { padding: 0px 10px 0px 20px; }
li { margin: 5px; }
```

</Sandpack>

Ingawa sintaksia inaonekana tofauti kidogo kwa sababu unaeleza sifa za object ya JavaScript badala ya mkusanyiko wa sifa za JSX, mifano hii inalingana kwa kiasi kikubwa, nawe unaweza kuchagua mbinu yoyote.

</Solution>

#### Rekebisha ukubwa wa picha kulingana na prop {/*adjust-the-image-size-based-on-a-prop*/}

Katika mfano huu, `Avatar` hupokea prop ya `size` ya kinamba inayobaini upana na urefu wa `<img>`. Prop ya `size` imewekwa kuwa `40` katika mfano huu. Hata hivyo, kama utafungua picha katika tabo mpya, utagundua kuwa picha yenyewe ni kubwa zaidi (`160` pikseli). Ukubwa halisi wa picha hubainishwa na ukubwa gani wa kijipicha (thumbnail) unaoomba.

Badilisha component ya `Avatar` ili iombe ukubwa wa picha ulio karibu zaidi kulingana na prop ya `size`. Hasa, kama `size` ni chini ya `90`, pitisha `'s'` ("small") badala ya `'b'` ("big") kwa function ya `getImageUrl`. Thibitisha kuwa mabadiliko yako yanafanya kazi kwa ku-render avatars kwa thamani tofauti za prop ya `size` na kufungua picha katika tabo mpya.

<Sandpack>

```js src/App.js
import { getImageUrl } from './utils.js';

function Avatar({ person, size }) {
  return (
    <img
      className="avatar"
      src={getImageUrl(person, 'b')}
      alt={person.name}
      width={size}
      height={size}
    />
  );
}

export default function Profile() {
  return (
    <Avatar
      size={40}
      person={{
        name: 'Gregorio Y. Zara',
        imageId: '7vQD0fP'
      }}
    />
  );
}
```

```js src/utils.js
export function getImageUrl(person, size) {
  return (
    'https://react.dev/images/docs/scientists/' +
    person.imageId +
    size +
    '.jpg'
  );
}
```

```css
.avatar { margin: 20px; border-radius: 50%; }
```

</Sandpack>

<Solution>

Hivi ndivyo unavyoweza kulifanya:

<Sandpack>

```js src/App.js
import { getImageUrl } from './utils.js';

function Avatar({ person, size }) {
  let thumbnailSize = 's';
  if (size > 90) {
    thumbnailSize = 'b';
  }
  return (
    <img
      className="avatar"
      src={getImageUrl(person, thumbnailSize)}
      alt={person.name}
      width={size}
      height={size}
    />
  );
}

export default function Profile() {
  return (
    <>
      <Avatar
        size={40}
        person={{
          name: 'Gregorio Y. Zara',
          imageId: '7vQD0fP'
        }}
      />
      <Avatar
        size={120}
        person={{
          name: 'Gregorio Y. Zara',
          imageId: '7vQD0fP'
        }}
      />
    </>
  );
}
```

```js src/utils.js
export function getImageUrl(person, size) {
  return (
    'https://react.dev/images/docs/scientists/' +
    person.imageId +
    size +
    '.jpg'
  );
}
```

```css
.avatar { margin: 20px; border-radius: 50%; }
```

</Sandpack>

Unaweza pia kuonyesha picha iliyo wazi zaidi kwa skrini za DPI kubwa kwa kuzingatia [`window.devicePixelRatio`](https://developer.mozilla.org/en-US/docs/Web/API/Window/devicePixelRatio):

<Sandpack>

```js src/App.js
import { getImageUrl } from './utils.js';

const ratio = window.devicePixelRatio;

function Avatar({ person, size }) {
  let thumbnailSize = 's';
  if (size * ratio > 90) {
    thumbnailSize = 'b';
  }
  return (
    <img
      className="avatar"
      src={getImageUrl(person, thumbnailSize)}
      alt={person.name}
      width={size}
      height={size}
    />
  );
}

export default function Profile() {
  return (
    <>
      <Avatar
        size={40}
        person={{
          name: 'Gregorio Y. Zara',
          imageId: '7vQD0fP'
        }}
      />
      <Avatar
        size={70}
        person={{
          name: 'Gregorio Y. Zara',
          imageId: '7vQD0fP'
        }}
      />
      <Avatar
        size={120}
        person={{
          name: 'Gregorio Y. Zara',
          imageId: '7vQD0fP'
        }}
      />
    </>
  );
}
```

```js src/utils.js
export function getImageUrl(person, size) {
  return (
    'https://react.dev/images/docs/scientists/' +
    person.imageId +
    size +
    '.jpg'
  );
}
```

```css
.avatar { margin: 20px; border-radius: 50%; }
```

</Sandpack>

Props hukuruhusu kufungasha mantiki kama hii ndani ya component ya `Avatar` (na kuibadilisha baadaye kama itahitajika) ili kila mtu aweze kutumia component ya `<Avatar>` bila kufikiria jinsi picha zinavyoombwa na kurekebishwa ukubwa.

</Solution>

#### Kupitisha JSX katika prop ya `children` {/*passing-jsx-in-a-children-prop*/}

Toa component ya `Card` kutoka kwa markup iliyo hapa chini, na utumie prop ya `children` kupitisha JSX tofauti kwake:

<Sandpack>

```js
export default function Profile() {
  return (
    <div>
      <div className="card">
        <div className="card-content">
          <h1>Picha</h1>
          <img
            className="avatar"
            src="https://react.dev/images/docs/scientists/OKS67lhm.jpg"
            alt="Aklilu Lemma"
            width={70}
            height={70}
          />
        </div>
      </div>
      <div className="card">
        <div className="card-content">
          <h1>Kuhusu</h1>
          <p>Aklilu Lemma alikuwa mwanasayansi mashuhuri wa Kiethiopia aliyegundua tiba ya asili ya kichocho.</p>
        </div>
      </div>
    </div>
  );
}
```

```css
.card {
  width: fit-content;
  margin: 20px;
  padding: 20px;
  border: 1px solid #aaa;
  border-radius: 20px;
  background: #fff;
}
.card-content {
  text-align: center;
}
.avatar {
  margin: 10px;
  border-radius: 50%;
}
h1 {
  margin: 5px;
  padding: 0;
  font-size: 24px;
}
```

</Sandpack>

<Hint>

JSX yoyote unayoiweka ndani ya tagi ya component itapitishwa kama prop ya `children` kwa component hiyo.

</Hint>

<Solution>

Hivi ndivyo unavyoweza kutumia component ya `Card` katika sehemu zote mbili:

<Sandpack>

```js
function Card({ children }) {
  return (
    <div className="card">
      <div className="card-content">
        {children}
      </div>
    </div>
  );
}

export default function Profile() {
  return (
    <div>
      <Card>
        <h1>Picha</h1>
        <img
          className="avatar"
          src="https://react.dev/images/docs/scientists/OKS67lhm.jpg"
          alt="Aklilu Lemma"
          width={100}
          height={100}
        />
      </Card>
      <Card>
        <h1>Kuhusu</h1>
        <p>Aklilu Lemma alikuwa mwanasayansi mashuhuri wa Kiethiopia aliyegundua tiba ya asili ya kichocho.</p>
      </Card>
    </div>
  );
}
```

```css
.card {
  width: fit-content;
  margin: 20px;
  padding: 20px;
  border: 1px solid #aaa;
  border-radius: 20px;
  background: #fff;
}
.card-content {
  text-align: center;
}
.avatar {
  margin: 10px;
  border-radius: 50%;
}
h1 {
  margin: 5px;
  padding: 0;
  font-size: 24px;
}
```

</Sandpack>

Unaweza pia kufanya `title` kuwa prop tofauti kama unataka kila `Card` iwe na kichwa daima:

<Sandpack>

```js
function Card({ children, title }) {
  return (
    <div className="card">
      <div className="card-content">
        <h1>{title}</h1>
        {children}
      </div>
    </div>
  );
}

export default function Profile() {
  return (
    <div>
      <Card title="Picha">
        <img
          className="avatar"
          src="https://react.dev/images/docs/scientists/OKS67lhm.jpg"
          alt="Aklilu Lemma"
          width={100}
          height={100}
        />
      </Card>
      <Card title="Kuhusu">
        <p>Aklilu Lemma alikuwa mwanasayansi mashuhuri wa Kiethiopia aliyegundua tiba ya asili ya kichocho.</p>
      </Card>
    </div>
  );
}
```

```css
.card {
  width: fit-content;
  margin: 20px;
  padding: 20px;
  border: 1px solid #aaa;
  border-radius: 20px;
  background: #fff;
}
.card-content {
  text-align: center;
}
.avatar {
  margin: 10px;
  border-radius: 50%;
}
h1 {
  margin: 5px;
  padding: 0;
  font-size: 24px;
}
```

</Sandpack>

</Solution>

</Challenges>
