---
title: Kuweka Components Zikiwa Pure
---

<Intro>

Baadhi ya functions za JavaScript ni *pure (safi).* Pure functions hufanya hesabu tu na si zaidi ya hilo. Kwa kuandika components zako kwa kufuata kikamilifu kanuni za pure functions, unaweza kuepuka aina nzima ya bugs zinazochanganya na tabia isiyotabirika kadri msimbo wako unavyokua. Ili kupata manufaa haya, hata hivyo, kuna kanuni chache lazima uzifuate.

</Intro>

<YouWillLearn>

* Purity ni nini na jinsi inavyokusaidia kuepuka bugs
* Jinsi ya kuweka components zikiwa pure kwa kuweka mabadiliko nje ya awamu ya render
* Jinsi ya kutumia Strict Mode kupata makosa katika components zako

</YouWillLearn>

## Purity: Components kama fomula {/*purity-components-as-formulas*/}

Katika sayansi ya kompyuta (na hasa ulimwengu wa functional programming), [pure function](https://wikipedia.org/wiki/Pure_function) ni function yenye sifa zifuatazo:

* **Inashughulikia mambo yake yenyewe.** Haibadilishi objects au vigezo vyovyote vilivyokuwepo kabla haijaitwa.
* **Ingizo lilelile, matokeo yaleyale.** Ikipewa maingizo yaleyale, pure function inapaswa kurudisha matokeo yaleyale kila mara.

Huenda tayari umeshajua mfano mmoja wa pure functions: fomula katika hisabati.

Fikiria fomula hii ya hisabati: <Math><MathI>y</MathI> = 2<MathI>x</MathI></Math>.

Kama <Math><MathI>x</MathI> = 2</Math> basi <Math><MathI>y</MathI> = 4</Math>. Kila mara. 

Kama <Math><MathI>x</MathI> = 3</Math> basi <Math><MathI>y</MathI> = 6</Math>. Kila mara. 

Kama <Math><MathI>x</MathI> = 3</Math>, <MathI>y</MathI> haitakuwa mara nyingine <Math>9</Math> au <Math>–1</Math> au <Math>2.5</Math> kutegemea saa ya siku au hali ya soko la hisa. 

Kama <Math><MathI>y</MathI> = 2<MathI>x</MathI></Math> na <Math><MathI>x</MathI> = 3</Math>, <MathI>y</MathI> _kila mara_ itakuwa <Math>6</Math>. 

Kama tungeigeuza hii kuwa function ya JavaScript, ingeonekana hivi:

```js
function double(number) {
  return 2 * number;
}
```

Katika mfano ulio hapo juu, `double` ni **pure function.** Ukiipitisha `3`, itarudisha `6`. Kila mara.

React imeundwa kuzunguka dhana hii. **React inadhani kuwa kila component unayoiandika ni pure function.** Hii inamaanisha kuwa components za React unazoandika lazima kila mara zirudishe JSX ileile ikipewa maingizo yaleyale:

<Sandpack>

```js src/App.js
function Recipe({ drinkers }) {
  return (
    <ol>    
      <li>Chemsha vikombe {drinkers} vya maji.</li>
      <li>Ongeza vijiko {drinkers} vya chai na vijiko {0.5 * drinkers} vya viungo.</li>
      <li>Ongeza vikombe {0.5 * drinkers} vya maziwa vichemke na sukari kwa ladha.</li>
    </ol>
  );
}

export default function App() {
  return (
    <section>
      <h1>Kichocheo cha Chai ya Viungo</h1>
      <h2>Kwa wawili</h2>
      <Recipe drinkers={2} />
      <h2>Kwa kusanyiko</h2>
      <Recipe drinkers={4} />
    </section>
  );
}
```

</Sandpack>

Unapopitisha `drinkers={2}` kwa `Recipe`, itarudisha JSX yenye `vikombe 2 vya maji`. Kila mara. 

Ukipitisha `drinkers={4}`, itarudisha JSX yenye `vikombe 4 vya maji`. Kila mara.

Kama fomula ya hisabati tu. 

Ungeweza kufikiria components zako kama vichocheo (mapishi): ukivifuata na usiongeze viambato vipya wakati wa upishi, utapata sahani ileile kila mara. "Sahani" hiyo ni JSX ambayo component huitolea React ili [i-render.](/learn/render-and-commit)

<Illustration src="/images/docs/illustrations/i_puritea-recipe.png" alt="A tea recipe for x people: take x cups of water, add x spoons of tea and 0.5x spoons of spices, and 0.5x cups of milk" />

## Side Effects: matokeo (yasiyo)kusudiwa {/*side-effects-unintended-consequences*/}

Mchakato wa ku-render wa React lazima kila mara uwe pure. Components zinapaswa *kurudisha* JSX yao tu, na si *kubadilisha* objects au vigezo vyovyote vilivyokuwepo kabla ya ku-render—hilo lingezifanya kuwa impure!

Hapa kuna component inayovunja kanuni hii:

<Sandpack>

```js {expectedErrors: {'react-compiler': [5]}}
let guest = 0;

function Cup() {
  // Baya: kubadilisha kigezo kilichokuwepo tayari!
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

Component hii inasoma na kuandika kigezo cha `guest` kilichotangazwa nje yake. Hii inamaanisha kuwa **kuita component hii mara nyingi kutazalisha JSX tofauti!** Na zaidi ya hayo, kama components _nyingine_ zitasoma `guest`, zitazalisha JSX tofauti pia, kutegemea zilipo-render lini! Hilo halitabiriki.

Tukirudi kwenye fomula yetu <Math><MathI>y</MathI> = 2<MathI>x</MathI></Math>, sasa hata kama <Math><MathI>x</MathI> = 2</Math>, hatuwezi kuamini kuwa <Math><MathI>y</MathI> = 4</Math>. Vipimo vyetu vingeweza kufeli, watumiaji wetu wangechanganyikiwa, ndege zingeanguka kutoka angani—unaweza kuona jinsi hili lingeweza kusababisha bugs zinazochanganya!

Unaweza kurekebisha component hii kwa [kupitisha `guest` kama prop badala yake](/learn/passing-props-to-a-component):

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

Sasa component yako ni pure, kwa kuwa JSX inayorudisha inategemea prop ya `guest` pekee.

Kwa ujumla, hupaswi kutarajia components zako zi-render kwa mpangilio wowote maalum. Haijalishi kama unaita <Math><MathI>y</MathI> = 2<MathI>x</MathI></Math> kabla au baada ya <Math><MathI>y</MathI> = 5<MathI>x</MathI></Math>: fomula zote mbili zitatatuliwa bila kutegemeana. Kwa njia ileile, kila component inapaswa "kufikiri yenyewe tu", na isijaribu kuratibu na au kutegemea nyingine wakati wa ku-render. Ku-render ni kama mtihani wa shule: kila component inapaswa kuhesabu JSX yenyewe!

<DeepDive>

#### Kugundua hesabu impure kwa StrictMode {/*detecting-impure-calculations-with-strict-mode*/}

Ingawa huenda bado hujazitumia zote, katika React kuna aina tatu za maingizo unayoweza kusoma wakati wa ku-render: [props](/learn/passing-props-to-a-component), [state (hali)](/learn/state-a-components-memory), na [context.](/learn/passing-data-deeply-with-context) Unapaswa kila mara kuyachukulia maingizo haya kama ya kusoma-tu.

Unapotaka *kubadilisha* kitu kwa kujibu ingizo la mtumiaji, unapaswa [kuweka state](/learn/state-a-components-memory) badala ya kuandika kwenye kigezo. Kamwe usibadilishe vigezo au objects vilivyokuwepo tayari wakati component yako inapo-render.

React inatoa "Strict Mode" ambamo inaita function ya kila component mara mbili wakati wa uendelezaji. **Kwa kuita functions za components mara mbili, Strict Mode husaidia kupata components zinazovunja kanuni hizi.**

Angalia jinsi mfano wa awali ulivyoonyesha "Guest #2", "Guest #4", na "Guest #6" badala ya "Guest #1", "Guest #2", na "Guest #3". Function ya awali ilikuwa impure, kwa hivyo kuiita mara mbili kuliivunja. Lakini toleo lililorekebishwa lililo pure hufanya kazi hata kama function itaitwa mara mbili kila wakati. **Pure functions huhesabu tu, kwa hivyo kuziita mara mbili hakutabadilisha chochote**--kama vile kuita `double(2)` mara mbili hakubadilishi kinachorudishwa, na kutatua <Math><MathI>y</MathI> = 2<MathI>x</MathI></Math> mara mbili hakubadilishi <MathI>y</MathI> ilivyo. Maingizo yaleyale, matokeo yaleyale. Kila mara.

Strict Mode haina athari katika production, kwa hivyo haitapunguza kasi ya programu kwa watumiaji wako. Ili kuchagua kutumia Strict Mode, unaweza kufunga component yako ya mzizi ndani ya `<React.StrictMode>`. Baadhi ya frameworks hufanya hivi kwa chaguo-msingi.

</DeepDive>

### Mabadiliko ya ndani: Siri ndogo ya component yako {/*local-mutation-your-components-little-secret*/}

Katika mfano ulio hapo juu, tatizo lilikuwa kwamba component ilibadilisha kigezo *kilichokuwepo tayari* wakati wa ku-render. Hili mara nyingi huitwa **"mutation"** ili lisikike la kutisha kidogo zaidi. Pure functions hazibadilishi vigezo vilivyo nje ya wigo wa function au objects vilivyoundwa kabla ya wito—hilo huzifanya kuwa impure!

Hata hivyo, **ni sawa kabisa kubadilisha vigezo na objects ambavyo _umeviunda_ hivi punde wakati wa ku-render.** Katika mfano huu, unaunda array ya `[]`, unaiweka kwenye kigezo cha `cups`, kisha una-`push` vikombe kadhaa ndani yake:

<Sandpack>

```js
function Cup({ guest }) {
  return <h2>Kikombe cha chai kwa mgeni #{guest}</h2>;
}

export default function TeaGathering() {
  const cups = [];
  for (let i = 1; i <= 12; i++) {
    cups.push(<Cup key={i} guest={i} />);
  }
  return cups;
}
```

</Sandpack>

Kama kigezo cha `cups` au array ya `[]` vingeundwa nje ya function ya `TeaGathering`, hili lingekuwa tatizo kubwa! Ungekuwa unabadilisha object *iliyokuwepo tayari* kwa kusukuma vitu ndani ya array hiyo.

Hata hivyo, ni sawa kwa sababu umeviunda *wakati wa render ileile*, ndani ya `TeaGathering`. Hakuna msimbo nje ya `TeaGathering` utakaowahi kujua kuwa hili lilitokea. Hili huitwa **"local mutation"** (mabadiliko ya ndani)—ni kama siri ndogo ya component yako.

## Wapi _unaweza_ kusababisha side effects {/*where-you-_can_-cause-side-effects*/}

Ingawa functional programming inategemea sana purity, wakati fulani, mahali fulani, _kitu fulani_ lazima kibadilike. Hilo ndilo lengo la programming kwa namna fulani! Mabadiliko haya—kusasisha skrini, kuanzisha animation, kubadilisha data—huitwa **side effects.** Ni mambo yanayotokea _"pembeni"_, si wakati wa ku-render.

Katika React, **side effects kwa kawaida hukaa ndani ya [event handlers.](/learn/responding-to-events)** Event handlers ni functions ambazo React huziendesha unapofanya kitendo fulani—kwa mfano, unapobonyeza kitufe. Ingawa event handlers hufafanuliwa *ndani* ya component yako, hazi-endeshi *wakati* wa ku-render! **Kwa hivyo event handlers hazihitaji kuwa pure.**

Kama umeisha chaguo nyingine zote na huwezi kupata event handler sahihi kwa side effect yako, bado unaweza kuiambatanisha na JSX yako inayorudishwa kwa wito wa [`useEffect`](/reference/react/useEffect) katika component yako. Hili huiambia React iitekeleze baadaye, baada ya ku-render, wakati side effects zinaporuhusiwa. **Hata hivyo, njia hii inapaswa kuwa suluhisho lako la mwisho.**

Inapowezekana, jaribu kuelezea mantiki yako kwa ku-render pekee. Utashangaa jinsi hili linavyoweza kukupeleka mbali!

<DeepDive>

#### Kwa nini React inajali kuhusu purity? {/*why-does-react-care-about-purity*/}

Kuandika pure functions kunahitaji mazoea na nidhamu fulani. Lakini pia kunafungua fursa za ajabu:

* Components zako zingeweza kuendeshwa katika mazingira tofauti—kwa mfano, kwenye seva! Kwa kuwa zinarudisha matokeo yaleyale kwa maingizo yaleyale, component moja inaweza kuhudumia maombi mengi ya watumiaji.
* Unaweza kuboresha utendaji kwa [kuruka ku-render](/reference/react/memo) components ambazo maingizo yao hayajabadilika. Hili ni salama kwa sababu pure functions kila mara hurudisha matokeo yaleyale, kwa hivyo ni salama kuzihifadhi kwenye cache.
* Kama data fulani ikibadilika katikati ya ku-render mti wa component wa kina, React inaweza kuanzisha upya ku-render bila kupoteza muda kumalizia render iliyopitwa na wakati. Purity huifanya kuwa salama kusimamisha kuhesabu wakati wowote.

Kila kipengele kipya cha React tunachojenga hunufaika na purity. Kutoka kwenye kupata data hadi animations hadi utendaji, kuweka components zikiwa pure hufungua nguvu ya paradaimu ya React.

</DeepDive>

<Recap>

* Component lazima iwe pure, ikimaanisha:
  * **Inashughulikia mambo yake yenyewe.** Haipaswi kubadilisha objects au vigezo vyovyote vilivyokuwepo kabla ya ku-render.
  * **Ingizo lilelile, matokeo yaleyale.** Ikipewa maingizo yaleyale, component inapaswa kila mara kurudisha JSX ileile. 
* Ku-render kunaweza kutokea wakati wowote, kwa hivyo components hazipaswi kutegemea mpangilio wa ku-render wa kila mmoja.
* Hupaswi kubadilisha (mutate) maingizo yoyote ambayo components zako zinatumia kwa ku-render. Hilo linajumuisha props, state, na context. Ili kusasisha skrini, ["weka" state](/learn/state-a-components-memory) badala ya kubadilisha objects zilizokuwepo tayari.
* Jitahidi kuelezea mantiki ya component yako katika JSX unayorudisha. Unapohitaji "kubadilisha mambo", kwa kawaida utataka kufanya hivyo katika event handler. Kama suluhisho la mwisho, unaweza kutumia `useEffect`.
* Kuandika pure functions kunahitaji mazoezi kidogo, lakini kunafungua nguvu ya paradaimu ya React.

</Recap>



<Challenges>

#### Rekebisha saa iliyoharibika {/*fix-a-broken-clock*/}

Component hii inajaribu kuweka klasi ya CSS ya `<h1>` kuwa `"night"` wakati wa saa kutoka usiku wa manane hadi saa sita asubuhi, na `"day"` nyakati nyingine zote. Hata hivyo, haifanyi kazi. Je, unaweza kurekebisha component hii?

Unaweza kuthibitisha kama suluhisho lako linafanya kazi kwa kubadilisha kwa muda eneo la saa la kompyuta. Wakati saa ya sasa iko kati ya usiku wa manane na saa sita asubuhi, saa inapaswa kuwa na rangi zilizogeuzwa!

<Hint>

Ku-render ni *hesabu*, hakupaswi kujaribu "kufanya" mambo. Je, unaweza kuelezea wazo lilelile kwa namna tofauti?

</Hint>

<Sandpack>

```js src/Clock.js active
export default function Clock({ time }) {
  const hours = time.getHours();
  if (hours >= 0 && hours <= 6) {
    document.getElementById('time').className = 'night';
  } else {
    document.getElementById('time').className = 'day';
  }
  return (
    <h1 id="time">
      {time.toLocaleTimeString()}
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
  return (
    <Clock time={time} />
  );
}
```

```css
body > * {
  width: 100%;
  height: 100%;
}
.day {
  background: #fff;
  color: #222;
}
.night {
  background: #222;
  color: #fff;
}
```

</Sandpack>

<Solution>

Unaweza kurekebisha component hii kwa kuhesabu `className` na kuijumuisha katika matokeo ya render:

<Sandpack>

```js src/Clock.js active
export default function Clock({ time }) {
  const hours = time.getHours();
  let className;
  if (hours >= 0 && hours <= 6) {
    className = 'night';
  } else {
    className = 'day';
  }
  return (
    <h1 className={className}>
      {time.toLocaleTimeString()}
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
  return (
    <Clock time={time} />
  );
}
```

```css
body > * {
  width: 100%;
  height: 100%;
}
.day {
  background: #fff;
  color: #222;
}
.night {
  background: #222;
  color: #fff;
}
```

</Sandpack>

Katika mfano huu, side effect (kurekebisha DOM) haikuwa ya lazima hata kidogo. Ulihitaji tu kurudisha JSX.

</Solution>

#### Rekebisha wasifu uliovunjika {/*fix-a-broken-profile*/}

Components mbili za `Profile` zime-render kando kwa kando zikiwa na data tofauti. Bonyeza "Collapse" kwenye wasifu wa kwanza, kisha "Expand". Utagundua kuwa wasifu zote mbili sasa zinaonyesha mtu yuleyule. Hii ni bug.

Tafuta chanzo cha bug na urekebishe.

<Hint>

Msimbo wenye bug uko katika `Profile.js`. Hakikisha unausoma wote kutoka juu hadi chini!

</Hint>

<Sandpack>

```js {expectedErrors: {'react-compiler': [7]}} src/Profile.js
import Panel from './Panel.js';
import { getImageUrl } from './utils.js';

let currentPerson;

export default function Profile({ person }) {
  currentPerson = person;
  return (
    <Panel>
      <Header />
      <Avatar />
    </Panel>
  )
}

function Header() {
  return <h1>{currentPerson.name}</h1>;
}

function Avatar() {
  return (
    <img
      className="avatar"
      src={getImageUrl(currentPerson)}
      alt={currentPerson.name}
      width={50}
      height={50}
    />
  );
}
```

```js src/Panel.js hidden
import { useState } from 'react';

export default function Panel({ children }) {
  const [open, setOpen] = useState(true);
  return (
    <section className="panel">
      <button onClick={() => setOpen(!open)}>
        {open ? 'Collapse' : 'Expand'}
      </button>
      {open && children}
    </section>
  );
}
```

```js src/App.js
import Profile from './Profile.js';

export default function App() {
  return (
    <>
      <Profile person={{
        imageId: 'lrWQx8l',
        name: 'Subrahmanyan Chandrasekhar',
      }} />
      <Profile person={{
        imageId: 'MK3eW3A',
        name: 'Creola Katherine Johnson',
      }} />
    </>
  )
}
```

```js src/utils.js hidden
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
.avatar { margin: 5px; border-radius: 50%; }
.panel {
  border: 1px solid #aaa;
  border-radius: 6px;
  margin-top: 20px;
  padding: 10px;
  width: 200px;
}
h1 { margin: 5px; font-size: 18px; }
```

</Sandpack>

<Solution>

Tatizo ni kwamba component ya `Profile` inaandika kwenye kigezo kilichokuwepo tayari kiitwacho `currentPerson`, na components za `Header` na `Avatar` zinasoma kutoka kwacho. Hili hufanya *zote tatu* kuwa impure na ngumu kutabiri.

Ili kurekebisha bug, ondoa kigezo cha `currentPerson`. Badala yake, pitisha taarifa zote kutoka `Profile` kwenda `Header` na `Avatar` kupitia props. Utahitaji kuongeza prop ya `person` kwa components zote mbili na kuipitisha hadi chini kabisa.

<Sandpack>

```js src/Profile.js active
import Panel from './Panel.js';
import { getImageUrl } from './utils.js';

export default function Profile({ person }) {
  return (
    <Panel>
      <Header person={person} />
      <Avatar person={person} />
    </Panel>
  )
}

function Header({ person }) {
  return <h1>{person.name}</h1>;
}

function Avatar({ person }) {
  return (
    <img
      className="avatar"
      src={getImageUrl(person)}
      alt={person.name}
      width={50}
      height={50}
    />
  );
}
```

```js src/Panel.js hidden
import { useState } from 'react';

export default function Panel({ children }) {
  const [open, setOpen] = useState(true);
  return (
    <section className="panel">
      <button onClick={() => setOpen(!open)}>
        {open ? 'Collapse' : 'Expand'}
      </button>
      {open && children}
    </section>
  );
}
```

```js src/App.js
import Profile from './Profile.js';

export default function App() {
  return (
    <>
      <Profile person={{
        imageId: 'lrWQx8l',
        name: 'Subrahmanyan Chandrasekhar',
      }} />
      <Profile person={{
        imageId: 'MK3eW3A',
        name: 'Creola Katherine Johnson',
      }} />
    </>
  );
}
```

```js src/utils.js hidden
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
.avatar { margin: 5px; border-radius: 50%; }
.panel {
  border: 1px solid #aaa;
  border-radius: 6px;
  margin-top: 20px;
  padding: 10px;
  width: 200px;
}
h1 { margin: 5px; font-size: 18px; }
```

</Sandpack>

Kumbuka kwamba React haihakikishi kuwa functions za components zitatekelezwa kwa mpangilio wowote maalum, kwa hivyo huwezi kuwasiliana kati yao kwa kuweka vigezo. Mawasiliano yote lazima yatokee kupitia props.

</Solution>

#### Rekebisha tray ya hadithi iliyovunjika {/*fix-a-broken-story-tray*/}

Mkurugenzi Mkuu (CEO) wa kampuni yako anakuomba uongeze "hadithi" kwenye programu yako ya saa ya mtandaoni, na huwezi kukataa. Umeandika component ya `StoryTray` inayokubali orodha ya `stories`, ikifuatiwa na kishika-nafasi cha "Create Story".

Ulitekeleza kishika-nafasi cha "Create Story" kwa kusukuma hadithi bandia moja zaidi mwishoni mwa array ya `stories` unayoipokea kama prop. Lakini kwa sababu fulani, "Create Story" inaonekana zaidi ya mara moja. Rekebisha tatizo hili.

<Sandpack>

```js src/StoryTray.js active
export default function StoryTray({ stories }) {
  stories.push({
    id: 'create',
    label: 'Create Story'
  });

  return (
    <ul>
      {stories.map(story => (
        <li key={story.id}>
          {story.label}
        </li>
      ))}
    </ul>
  );
}
```

```js {expectedErrors: {'react-compiler': [16]}} src/App.js hidden
import { useState, useEffect } from 'react';
import StoryTray from './StoryTray.js';

const initialStories = [
  {id: 0, label: "Ankit's Story" },
  {id: 1, label: "Taylor's Story" },
];

export default function App() {
  const [stories, setStories] = useState([...initialStories])
  const time = useTime();

  // HACK: Prevent the memory from growing forever while you read docs.
  // We're breaking our own rules here.
  if (stories.length > 100) {
    stories.length = 100;
  }

  return (
    <div
      style={{
        width: '100%',
        height: '100%',
        textAlign: 'center',
      }}
    >
      <h2>Sasa ni saa {time.toLocaleTimeString()}.</h2>
      <StoryTray stories={stories} />
    </div>
  );
}

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
```

```css
ul {
  margin: 0;
  list-style-type: none;
}

li {
  border: 1px solid #aaa;
  border-radius: 6px;
  float: left;
  margin: 5px;
  margin-bottom: 20px;
  padding: 5px;
  width: 70px;
  height: 100px;
}
```

```js sandbox.config.json hidden
{
  "hardReloadOnChange": true
}
```

</Sandpack>

<Solution>

Angalia jinsi kila saa inaposasishwa, "Create Story" inaongezwa *mara mbili*. Hii inatoa dokezo kwamba tuna mutation wakati wa ku-render--Strict Mode inaita components mara mbili ili kufanya matatizo haya yaonekane zaidi.

Function ya `StoryTray` si pure. Kwa kuita `push` kwenye array ya `stories` iliyopokelewa (prop!), inabadilisha object iliyoundwa *kabla* `StoryTray` haijaanza ku-render. Hili huifanya kuwa na bug na ngumu sana kutabiri.

Njia rahisi zaidi ya kurekebisha ni kutogusa array kabisa, na ku-render "Create Story" kando:

<Sandpack>

```js src/StoryTray.js active
export default function StoryTray({ stories }) {
  return (
    <ul>
      {stories.map(story => (
        <li key={story.id}>
          {story.label}
        </li>
      ))}
      <li>Create Story</li>
    </ul>
  );
}
```

```js {expectedErrors: {'react-compiler': [16]}} src/App.js hidden
import { useState, useEffect } from 'react';
import StoryTray from './StoryTray.js';

const initialStories = [
  {id: 0, label: "Ankit's Story" },
  {id: 1, label: "Taylor's Story" },
];

export default function App() {
  const [stories, setStories] = useState([...initialStories])
  const time = useTime();

  // HACK: Prevent the memory from growing forever while you read docs.
  // We're breaking our own rules here.
  if (stories.length > 100) {
    stories.length = 100;
  }

  return (
    <div
      style={{
        width: '100%',
        height: '100%',
        textAlign: 'center',
      }}
    >
      <h2>Sasa ni saa {time.toLocaleTimeString()}.</h2>
      <StoryTray stories={stories} />
    </div>
  );
}

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
```

```css
ul {
  margin: 0;
  list-style-type: none;
}

li {
  border: 1px solid #aaa;
  border-radius: 6px;
  float: left;
  margin: 5px;
  margin-bottom: 20px;
  padding: 5px;
  width: 70px;
  height: 100px;
}
```

</Sandpack>

Vinginevyo, ungeweza kuunda array _mpya_ (kwa kunakili iliyopo) kabla ya kusukuma kitu ndani yake:

<Sandpack>

```js src/StoryTray.js active
export default function StoryTray({ stories }) {
  // Nakili array!
  const storiesToDisplay = stories.slice();

  // Haiathiri array ya awali:
  storiesToDisplay.push({
    id: 'create',
    label: 'Create Story'
  });

  return (
    <ul>
      {storiesToDisplay.map(story => (
        <li key={story.id}>
          {story.label}
        </li>
      ))}
    </ul>
  );
}
```

```js {expectedErrors: {'react-compiler': [16]}} src/App.js hidden
import { useState, useEffect } from 'react';
import StoryTray from './StoryTray.js';

const initialStories = [
  {id: 0, label: "Ankit's Story" },
  {id: 1, label: "Taylor's Story" },
];

export default function App() {
  const [stories, setStories] = useState([...initialStories])
  const time = useTime();

  // HACK: Prevent the memory from growing forever while you read docs.
  // We're breaking our own rules here.
  if (stories.length > 100) {
    stories.length = 100;
  }

  return (
    <div
      style={{
        width: '100%',
        height: '100%',
        textAlign: 'center',
      }}
    >
      <h2>Sasa ni saa {time.toLocaleTimeString()}.</h2>
      <StoryTray stories={stories} />
    </div>
  );
}

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
```

```css
ul {
  margin: 0;
  list-style-type: none;
}

li {
  border: 1px solid #aaa;
  border-radius: 6px;
  float: left;
  margin: 5px;
  margin-bottom: 20px;
  padding: 5px;
  width: 70px;
  height: 100px;
}
```

</Sandpack>

Hili huweka mutation yako ikiwa ya ndani na function yako ya ku-render ikiwa pure. Hata hivyo, bado unahitaji kuwa makini: kwa mfano, kama ungejaribu kubadilisha kimojawapo cha vitu vilivyopo kwenye array, ungehitaji kuvinakili vitu hivyo pia.

Ni muhimu kukumbuka ni operesheni gani kwenye arrays zinazozibadilisha, na zipi hazibadilishi. Kwa mfano, `push`, `pop`, `reverse`, na `sort` zitabadilisha array ya awali, lakini `slice`, `filter`, na `map` zitaunda mpya.

</Solution>

</Challenges>
