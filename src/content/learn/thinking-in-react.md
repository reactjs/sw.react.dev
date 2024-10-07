---
title: Kuwaza katika React
---

<Intro>

React inaweza kubadilisha jinsi unavyowazia kuhusu miundo unayotazama na programu unazounda. Unapounda kiolesura cha mtumiaji na React, kwanza utakigawanya kiwe vipande vipande vinavyoitwa *vijenzi (components)*. Kisha, utaelezea hali tofauti za kuona kwa kila kijenzi chako. Hatimaye, utaunganisha vijenzi vyako pamoja ili data ipite kupitia kwao. Katika funzo hili, tutakuongoza kupitia mchakato wa mawazo wa kuunda jedwali la data ya bidhaa inayoweza kutafutwa kwa kutumia React.

</Intro>

## Anza na kiigizo {/*start-with-the-mockup*/}

Wazia kuwa tayari unayo API ya JSON na picha kutoka kwa mbuni.

JSON API inarejesha data fulani ambayo inaonekana kama hii:

```json
[
  { category: "Fruits", price: "$1", stocked: true, name: "Apple" },
  { category: "Fruits", price: "$1", stocked: true, name: "Dragonfruit" },
  { category: "Fruits", price: "$2", stocked: false, name: "Passionfruit" },
  { category: "Vegetables", price: "$2", stocked: true, name: "Spinach" },
  { category: "Vegetables", price: "$4", stocked: false, name: "Pumpkin" },
  { category: "Vegetables", price: "$1", stocked: true, name: "Peas" }
]
```

Kiigizo (Mockup) kinaonekana hivii:

<img src="/images/docs/s_thinking-in-react_ui.png" width="300" style={{margin: '0 auto'}} />

Ili kutekeleza UI katika React, kwa kawaida utafuata hatua tano sawa.

## Hatua ya 1: Vunja UI kuwa daraja la vijenzi {/*step-1-break-the-ui-into-a-component-hierarchy*/}

Anza kwa kuchora visanduku kuzunguka kila kijenzi na kijenzi kidogo kwenye kiigizo na uvipe majina. Ikiwa unafanya kazi na mbuni, wanaweza kuwa tayari wamevipa majina vijenzi hivi kwenye zana yao ya kubuni. Waulize!

Kulingana na msingi wako, unaweza kufikiria kugawanya muundo katika vijenzi kwa njia tofauti:

* **Programming**--tumia mbinu zile zile za kuamua ikiwa unapaswa kuunda kitendaji kipya au object. Mbinu moja kama hiyo ni [kanuni ya uwajibikaji mmoja (single responsibility principle)](https://en.wikipedia.org/wiki/Single_responsibility_principle), yaani, kijenzi kinapaswa kufanya jambo moja tu. Ikiwa kitaishia kukua, inapaswa kigawanywe kiwe vijenzi vidogo.
* **CSS**--fikiria ni nini ungetengeneza viteule vya darasa. (Hatahivyo, vijenzi ni vidogo kwa kiasi fulani.)
* **Design**--fikiria jinsi unavyoweza kupanga safu za muundo.

Ikiwa JSON yako imeundwa vizuri, mara nyingi utapata kwamba inaelekeza kwa muundo wa vijenzi vya UI yako. Hiyo ni kwa sababu UI na miundo ya data mara nyingi huwa na usanifu sawa wa habari--yaani, umbo sawa. Tenganisha UI yako katika vijenzi, ambapo kila kijenzi kinalingana na kipande kimoja cha muundo yako wa data.

Kuna vijenzi vitano kwenye skrini hii:

<FullWidth>

<CodeDiagram flip>

<img src="/images/docs/s_thinking-in-react_ui_outline.png" width="500" style={{margin: '0 auto'}} />
1. `FilterableProductTable` (kijivu) ina programu nzima.
2. `SearchBar` (bluu) inapokea ingizo la mtumiaji.
3. `ProductTable` (lavender) huonyesha na kuchuja orodha kulingana na ingizo la mtumiaji.
4. `ProductCategoryRow` (kijani) huonyesha kichwa kwa kila aina.
5. `ProductRow` (manjano) huonyesha safu mlalo kwa kila bidhaa.

</CodeDiagram>

</FullWidth>

Ukiangalia `ProductTable` (lavenda), utaona kwamba kichwa cha jedwali (kilicho na lebo za "Name" na "Price") si kijenzi chake chenyewe. Hili ni suala la upendeleo, na unaweza kwenda kwa njia yoyote. Kwa mfano huu, ni sehemu ya `ProductTable` kwa sababu inaonekana ndani ya orodha ya `ProductTable`. Hatahivyo, ikiwa kichwa hiki kitakua kikubwa (k.m., ukiongeza kupanga), unaweza kukisogeza hadi kijenzi chake cha `ProductTableHeader`.

Sasa kwa kuwa umetambua vijenzi kwenye kiigizo, vipange katika daraja (hierachy). Vijenzi vinavyoonekana ndani ya kijenzi kingine katika kiigizo vinapaswa kuonekana kama mtoto katika daraja:

* `FilterableProductTable`
    * `SearchBar`
    * `ProductTable`
        * `ProductCategoryRow`
        * `ProductRow`

## Hatua ya 2: Unda toleo tuli katika React {/*step-2-build-a-static-version-in-react*/}

Kwa kuwa sasa unalo daraja lako la vijenzi, ni wakati wa kutekeleza programu yako. Mbinu iliyonyooka zaidi ni kuunda toleo ambalo linatoa UI kutoka kwa muundo wako wa data bila kuongeza mwingiliano wowote... bado! Mara nyingi ni rahisi kuunda toleo tuli kwanza na kuongeza mwingiliano baadaye. Kuunda toleo tuli (static version) kunahitaji kuandika sana bila kufikiria, lakini kuongeza mwingiliano kunahitaji kufikiria sana na sio kuandika sana.

Ili kuunda toleo tuli la programu yako linalotoa muundo wa data yako, utataka kuunda [vijenzi](/learn/your-first-component) ambavyo vinatumia tena vipengele vingine na kupitisha data kwa kutumia [viunga (props).](/learn/passing-props-to-a-component) Viunga ni njia ya kupitisha data kutoka kwa mzazi hadi kwa mtoto. (Ikiwa unafahamu dhana ya [hali (state)](/learn/state-a-components-memory), usitumie hali hata kidogo kuunda toleo hili tuli. Hali imetengwa kwa ajili ya mwingiliano pekee, yaani, data ambayo hubadilika baada ya muda. Kwa kuwa hili ni toleo tuli la programu, huhihitaji.)

Unaweza kujenga "juu chini" kwa kuanza na kujenga vijenzi juu zaidi katika daraja (kama `FilterableProductTable`) au "chini juu" kwa kufanya kazi kutoka kwa vijenzi chini chini (kama `ProductRow`). Katika mifano rahisi, kwa kawaida ni rahisi kwenda juu-chini, na kwenye miradi mikubwa, ni rahisi kwenda chini-juu.

<Sandpack>

```jsx src/App.js
function ProductCategoryRow({ category }) {
  return (
    <tr>
      <th colSpan="2">
        {category}
      </th>
    </tr>
  );
}

function ProductRow({ product }) {
  const name = product.stocked ? product.name :
    <span style={{ color: 'red' }}>
      {product.name}
    </span>;

  return (
    <tr>
      <td>{name}</td>
      <td>{product.price}</td>
    </tr>
  );
}

function ProductTable({ products }) {
  const rows = [];
  let lastCategory = null;

  products.forEach((product) => {
    if (product.category !== lastCategory) {
      rows.push(
        <ProductCategoryRow
          category={product.category}
          key={product.category} />
      );
    }
    rows.push(
      <ProductRow
        product={product}
        key={product.name} />
    );
    lastCategory = product.category;
  });

  return (
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Price</th>
        </tr>
      </thead>
      <tbody>{rows}</tbody>
    </table>
  );
}

function SearchBar() {
  return (
    <form>
      <input type="text" placeholder="Search..." />
      <label>
        <input type="checkbox" />
        {' '}
        Only show products in stock
      </label>
    </form>
  );
}

function FilterableProductTable({ products }) {
  return (
    <div>
      <SearchBar />
      <ProductTable products={products} />
    </div>
  );
}

const PRODUCTS = [
  {category: "Fruits", price: "$1", stocked: true, name: "Apple"},
  {category: "Fruits", price: "$1", stocked: true, name: "Dragonfruit"},
  {category: "Fruits", price: "$2", stocked: false, name: "Passionfruit"},
  {category: "Vegetables", price: "$2", stocked: true, name: "Spinach"},
  {category: "Vegetables", price: "$4", stocked: false, name: "Pumpkin"},
  {category: "Vegetables", price: "$1", stocked: true, name: "Peas"}
];

export default function App() {
  return <FilterableProductTable products={PRODUCTS} />;
}
```

```css
body {
  padding: 5px
}
label {
  display: block;
  margin-top: 5px;
  margin-bottom: 5px;
}
th {
  padding-top: 10px;
}
td {
  padding: 2px;
  padding-right: 40px;
}
```

</Sandpack>

(Ikiwa msimbo huu unaonekana wa kuudhi, pitia [Anza Haraka](/learn/) kwanza!)

Baada ya kuunda vijenzi vyako, utakuwa na maktaba ya vijenzi vinavyoweza kutumika tena vinavyotoa kielelezo chako cha data. Kwa sababu hii ni programu tuli, vijenzi vitarejesha JSX pekee. Kijenzi kilicho juu ya daraja (`FilterableProductTable`) kitachukua kielelezo chako wa data kama mhimili. Hii inaitwa _one-way data flow_ kwa sababu data hutiririka kutoka kijenzi cha kiwango cha juu hadi kile kilicho chini ya mti.

<Pitfall>
Kufikia hapa, haupaswi kuwa unatumia thamani za hali zozote. Hiyo ni kwa hatua inayofuata!
</Pitfall>

## Hatua ya 3: Pata uwakilishi mdogo lakini kamili wa hali ya UI {/*step-3-find-the-minimal-but-complete-representation-of-ui-state*/}

Ili kufanya UI iwe na mwingiliano, unahitaji kuwaruhusu watumiaji kubadilisha kielelezo chako cha msingi wa data. Utatumia *hali (state)* kwa ajili ya hili.

Wazia hali kama seti ndogo ya kubadilisha data ambayo programu yako inahitaji kukumbuka. Kanuni muhimu zaidi ya uundaji wa hali ni kuitunza [KAUSHA (Usijirudie).](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) Tambua uwakilishi mdogo kabisa wa hali ambayo programu yako inahitaji na uhesabu kila kitu kingine unapohitaji. Kwa mfano, ikiwa unaunda orodha ya ununuzi, unaweza kuhifadhi bidhaa kama array katika hali. Ikiwa unataka pia kuonyesha idadi ya bidhaa kwenye orodha, usihifadhi idadi ya bidhaa kama thamani nyingine ya hali--badala yake, soma urefu wa array yako.

Sasa fikiria vipande vyote vya data katika programu hii ya mfano:

1. Orodha ya asili ya bidhaa
2. Maandishi ya utafutaji ambayo mtumiaji ameingiza
3. Value ya kisanduku cha kuteua
4. Orodha iliyochujwa ya bidhaa

Hali zipi kati ya hizi? Tambua zile ambazo sio:

* Je, **inabaki ilivyo bila kubadilika** baada ya muda? Ikiwa ni hivyo, hio sio hali.
* Je, **imepitishwa kutoka kwa mzazi** kupitia prop? Ikiwa ni hivyo, hio sio hali.
* **Je, unaweza kuihesabu** kulingana na hali iliyopo au vifaa katika kijenzi chako? Ikiwa ndivyo, *bila shaka* hio nayo sio hali!

Kilichobaki labda ni hali.

Wacha tupitie moja baada ya nyingine tena:

1. Orodha asili ya bidhaa **imepitishwa kama vifaa, kwa hivyo haijabainishwa.**
2. Maandishi ya utafutaji yanaonekana kuwa ya hali kwani yanabadilika kwa wakati na hayawezi kukokotwa kutoka kwa chochote.
3. Thamani ya kisanduku cha kuteua inaonekana kuwa hali kwani inabadilika kwa wakati na haiwezi kukokotwa kutoka kwa chochote.
4. Orodha iliyochujwa ya bidhaa **haijabainishwa kwa sababu inaweza kukokotwa** kwa kuchukua orodha asili ya bidhaa na kuichuja kulingana na maandishi ya utafutaji na thamani ya kisanduku cha kuteua.

Hii ina maana kwamba ni maandishi ya utafutaji pekee na thamani ya kisanduku cha kuteua ndizo zilizotajwa! Imefanywa vizuri!
<DeepDive>

#### Viunga dhidi ya Hali {/*props-vs-state*/}

Kuna aina mbili za data ya "kielelezo" katika React: viunga na hali. Hizi mbili ni tofauti sana:

* [**Viunga** ni kama hoja unazopitisha](/learn/passing-props-to-a-component) kwa kitendaji. Huruhusu kijenzi mzazi kupitisha data kwa kijenzi mtoto na kubinafsisha mwonekano wake. Kwa mfano, `Fomu` inaweza kupitisha kiunga cha `color` kwa `Button`.
* [**Hali** ni kama kumbukumbu ya kijenzi.](/learn/state-a-components-memory) Huruhusu kijenzi kufuatilia baadhi ya taarifa na kuibadilisha kulingana na mwingiliano. Kwa mfano, `Button` kinaweza kufuatilia hali ya `isHovered`.

Viunga na hali ni tofauti, lakini vinafanya kazi pamoja. Kijenzi mzazi mara nyingi kitaweka baadhi ya taarifa katika hali (ili kiweze kukibadilisha), na *kuipitisha* kwa vijenzi watoto kama viunga vyake. Ni sawa ikiwa tofauti bado inahisi kuwa ngumu katika usomaji wa kwanza. Inachukua mazoezi kidogo ili iweze kushikamana kabisa!

</DeepDive>

## Hatua ya 4: Tambua pale hali yako itadumu {/*step-4-identify-where-your-state-should-live*/}

Baada ya kutambua data ya hali ya chini zaidi ya programu yako, unahitaji kutambua ni kijenzi kipi kinawajibika kubadilisha hali hii, au *kinamiliki* hali. Kumbuka: React hutumia mtiririko wa data wa njia moja, kupitisha data chini ya daraja ya vijenzi kutoka kijenzi mzazi hadi cha mtoto. Huenda isieleweke mara moja ni kijenzi gani kinapaswa kumiliki hali gani. Hili linaweza kuwa gumu ikiwa wewe ni mgeni kwa dhana hii, lakini unaweza kuibaini kwa kufuata hatua hizi!

Kwa kila sehemu ya hali katika programu yako:

1. Tambua *kila* kijenzi kinachofanya kitu kulingana na hali hiyo.
2. Tafuta kijenzi chao cha karibu zaidi cha mzazi--kijenzi kilicho juu yao yote katika daraja.
3. Amua mahali ambapo hali inapaswa kudumu:
     1. Mara nyingi, unaweza kuweka hali moja kwa moja kwenye mzazi wao wa kawaida.
     2. Unaweza pia kuweka hali katika kijenzi fulani juu ya mzazi wao wa kawaida.
     3. Iwapo huwezi kupata kipengele ambapo inaeleweka kumiliki jimbo, unda kijenzi kipya kwa ajili ya kushikilia hali pekee na uiongeze mahali fulani katika daraja juu ya kijenzi mzazi cha kawaida.

In the previous step, you found two pieces of state in this application: the search input text, and the thamani of the checkbox. In this example, they always appear together, so it makes sense to put them into the same place.

Katika hatua ya awali, ulipata vipande viwili vya hali katika programu hii: maandishi ya ingizo ya utafutaji, na thamani ya kisanduku cha kuteua. Katika mfano huu, daima huonekana pamoja, kwa hiyo ni mantiki kuviweka katika sehemu moja.

<<<<<<< HEAD
Sasa wacha tupitie mkakati wetu kwao:
=======
1. **Identify components that use state:**
    * `ProductTable` needs to filter the product list based on that state (search text and checkbox value). 
    * `SearchBar` needs to display that state (search text and checkbox value).
2. **Find their common parent:** The first parent component both components share is `FilterableProductTable`.
3. **Decide where the state lives**: We'll keep the filter text and checked state values in `FilterableProductTable`.
>>>>>>> 1697ae89a3bbafd76998dd7496754e5358bc1e9a

1. **Tambua vijenzi vinavyotumia hali:**
     * `ProductTable` inahitaji kuchuja orodha ya bidhaa kulingana na hali hiyo (maandishi ya utafutaji na thamani ya kisanduku cha kuteua).
     * `SearchBar` inahitaji kuonyesha hali hiyo (maandishi ya utafutaji na thamani ya kisanduku cha kuteua).
1. **Tafuta mzazi wao wa kawaida:** Kijenzi mzazi cha kwanza ambacho vijenzi vyote viwili hushiriki ni `FilterableProductTable`.
2. **Amua mahali pa kuishi**: Tutaweka maandishi ya kichujio na thamani za hali zilizochaguliwa katika `FilterableProductTable`.

Kwa hivyo state thamani zitaishi katika `FilterableProductTable`.

Ongeza hali kwa kijenzi chenye [Kiunganishi (Hook) cha `useState()`.](/reference/react/useState) Hooks ni vitendaji maalum ambavyo hukuruhusu "kuunganisha" React. Ongeza vigezo viwili vya hali juu ya `FilterableProductTable` na ubainishe hali yao ya awali:

```js
function FilterableProductTable({ products }) {
  const [filterText, setFilterText] = useState('');
  const [inStockOnly, setInStockOnly] = useState(false);  
```

Kisha, pitisha `filterText` na `inStockOnly` kwa `ProductTable` na `SearchBar` kama kiunga:
```js
<div>
  <SearchBar 
    filterText={filterText} 
    inStockOnly={inStockOnly} />
  <ProductTable 
    products={products}
    filterText={filterText}
    inStockOnly={inStockOnly} />
</div>
```

Unaweza kuanza kuona jinsi programu yako itakavyofanya kazi. Hariri vakue ya awali ya `filterText` kutoka `useState('')` hadi `useState('fruit')` katika msimbo wa kisanduku hapa chini. Utaona maandishi ya utafutaji na sasisho la jedwali:
<Sandpack>

```jsx src/App.js
import { useState } from 'react';

function FilterableProductTable({ products }) {
  const [filterText, setFilterText] = useState('');
  const [inStockOnly, setInStockOnly] = useState(false);

  return (
    <div>
      <SearchBar 
        filterText={filterText} 
        inStockOnly={inStockOnly} />
      <ProductTable 
        products={products}
        filterText={filterText}
        inStockOnly={inStockOnly} />
    </div>
  );
}

function ProductCategoryRow({ category }) {
  return (
    <tr>
      <th colSpan="2">
        {category}
      </th>
    </tr>
  );
}

function ProductRow({ product }) {
  const name = product.stocked ? product.name :
    <span style={{ color: 'red' }}>
      {product.name}
    </span>;

  return (
    <tr>
      <td>{name}</td>
      <td>{product.price}</td>
    </tr>
  );
}

function ProductTable({ products, filterText, inStockOnly }) {
  const rows = [];
  let lastCategory = null;

  products.forEach((product) => {
    if (
      product.name.toLowerCase().indexOf(
        filterText.toLowerCase()
      ) === -1
    ) {
      return;
    }
    if (inStockOnly && !product.stocked) {
      return;
    }
    if (product.category !== lastCategory) {
      rows.push(
        <ProductCategoryRow
          category={product.category}
          key={product.category} />
      );
    }
    rows.push(
      <ProductRow
        product={product}
        key={product.name} />
    );
    lastCategory = product.category;
  });

  return (
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Price</th>
        </tr>
      </thead>
      <tbody>{rows}</tbody>
    </table>
  );
}

function SearchBar({ filterText, inStockOnly }) {
  return (
    <form>
      <input 
        type="text" 
        value={filterText} 
        placeholder="Search..."/>
      <label>
        <input 
          type="checkbox" 
          checked={inStockOnly} />
        {' '}
        Only show products in stock
      </label>
    </form>
  );
}

const PRODUCTS = [
  {category: "Fruits", price: "$1", stocked: true, name: "Apple"},
  {category: "Fruits", price: "$1", stocked: true, name: "Dragonfruit"},
  {category: "Fruits", price: "$2", stocked: false, name: "Passionfruit"},
  {category: "Vegetables", price: "$2", stocked: true, name: "Spinach"},
  {category: "Vegetables", price: "$4", stocked: false, name: "Pumpkin"},
  {category: "Vegetables", price: "$1", stocked: true, name: "Peas"}
];

export default function App() {
  return <FilterableProductTable products={PRODUCTS} />;
}
```

```css
body {
  padding: 5px
}
label {
  display: block;
  margin-top: 5px;
  margin-bottom: 5px;
}
th {
  padding-top: 5px;
}
td {
  padding: 2px;
}
```

</Sandpack>

Kumbuka kwamba kuhariri fomu bado haifanyi kazi. Kuna hitilafu ya kiweko kwenye sanduku la mchanga hapo juu kuelezea kwa nini:

<ConsoleBlock level="error">

Umepeana kiunga cha \`value\` cha uga wa fomu bila kidhibiti cha \`onChange\`. Hii itatoa uga wa kusoma pekee.

</ConsoleBlock>

Katika kisanduku cha mchanga hapo juu, viunga `ProductTable` na `SearchBar` soma `filterText` na `inStockOnly` ili kutoa jedwali, ingizo, na kisanduku cha kuteua. Kwa mfano, hivi ndivyo `SearchBar` inavyojaza thamani ya ingizo:

```js {1,6}
function SearchBar({ filterText, inStockOnly }) {
  return (
    <form>
      <input 
        type="text" 
        value={filterText} 
        placeholder="Search..."/>
```

Hata hivyo, bado hujaongeza msimbo wowote ili kujibu vitendo vya mtumiaji kama vile kuandika. Hii itakuwa hatua yako ya mwisho.

## Hatua ya 5: Ongeza mtiririko wa data kinyume {/*step-5-add-inverse-data-flow*/}

You want to make it so whenever the user changes the form inputs, the state updates to reflect those changes. The state is owned by `FilterableProductTable`, so only it can call `setFilterText` and `setInStockOnly`. To let `SearchBar` update the `FilterableProductTable`'s state, you need to pass these functions down to `SearchBar`:

Kwa sasa programu yako inatekelezwa kwa njia ipasavyo ikiwa na viunga na hali inayotiririka chini ya daraja. Lakini ili kubadilisha hali kulingana na ingizo la mtumiaji, utahitaji kuauni data inayotiririka kwa njia nyingine: vijenzi vya fomu vilivyo ndani kabisa ya daraja vinahitaji kusasisha hali katika `FilterableProductTable`.

React hufanya mtiririko huu wa data kuwa wazi, lakini unahitaji kuandika zaidi kuliko kuunganisha data kwa njia mbili. Ukijaribu kuandika au kuteua kisanduku katika mfano ulio hapo juu, utaona kuwa React inapuuza ingizo lako. Hii ni makusudi. Kwa kuandika `<input value={filterText} />`, umeweka `value` mhimili wa `input` kuwa sawa na `filterText` hali iliyopitishwa kutoka `FilterableProductTable`. Kwa kuwa hali ya `filterText` haijawekwa kamwe, ingizo halibadiliki.

Unataka kuifanya iwe hivyo wakati wowote mtumiaji anapobadilisha ingizo za fomu, hali husasisha ili kuonyesha mabadiliko hayo. Hali hiyo inamilikiwa na `FilterableProductTable`, kwa hivyo inaweza tu kuita `setFilterText` na `setInStockOnly`. Ili kuruhusu `SearchBar` kusasisha hali ya `FilterableProductTable`, unahitaji kupitisha vitendaji hivi hadi kwa `SearchBar`:

```js {2,3,10,11}
function FilterableProductTable({ products }) {
  const [filterText, setFilterText] = useState('');
  const [inStockOnly, setInStockOnly] = useState(false);

  return (
    <div>
      <SearchBar 
        filterText={filterText} 
        inStockOnly={inStockOnly}
        onFilterTextChange={setFilterText}
        onInStockOnlyChange={setInStockOnly} />
```

Ndani ya `SearchBar`, utaongeza vidhibiti vya tukio vya `onChange` na kuweka hali mzazi kutoka kwao:

```js {4,5,13,19}
function SearchBar({
  filterText,
  inStockOnly,
  onFilterTextChange,
  onInStockOnlyChange
}) {
  return (
    <form>
      <input
        type="text"
        value={filterText}
        placeholder="Search..."
        onChange={(e) => onFilterTextChange(e.target.value)}
      />
      <label>
        <input
          type="checkbox"
          checked={inStockOnly}
          onChange={(e) => onInStockOnlyChange(e.target.checked)}
```

Sasa programu inafanya kazi kikamilifu!

<Sandpack>

```jsx src/App.js
import { useState } from 'react';

function FilterableProductTable({ products }) {
  const [filterText, setFilterText] = useState('');
  const [inStockOnly, setInStockOnly] = useState(false);

  return (
    <div>
      <SearchBar 
        filterText={filterText} 
        inStockOnly={inStockOnly} 
        onFilterTextChange={setFilterText} 
        onInStockOnlyChange={setInStockOnly} />
      <ProductTable 
        products={products} 
        filterText={filterText}
        inStockOnly={inStockOnly} />
    </div>
  );
}

function ProductCategoryRow({ category }) {
  return (
    <tr>
      <th colSpan="2">
        {category}
      </th>
    </tr>
  );
}

function ProductRow({ product }) {
  const name = product.stocked ? product.name :
    <span style={{ color: 'red' }}>
      {product.name}
    </span>;

  return (
    <tr>
      <td>{name}</td>
      <td>{product.price}</td>
    </tr>
  );
}

function ProductTable({ products, filterText, inStockOnly }) {
  const rows = [];
  let lastCategory = null;

  products.forEach((product) => {
    if (
      product.name.toLowerCase().indexOf(
        filterText.toLowerCase()
      ) === -1
    ) {
      return;
    }
    if (inStockOnly && !product.stocked) {
      return;
    }
    if (product.category !== lastCategory) {
      rows.push(
        <ProductCategoryRow
          category={product.category}
          key={product.category} />
      );
    }
    rows.push(
      <ProductRow
        product={product}
        key={product.name} />
    );
    lastCategory = product.category;
  });

  return (
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Price</th>
        </tr>
      </thead>
      <tbody>{rows}</tbody>
    </table>
  );
}

function SearchBar({
  filterText,
  inStockOnly,
  onFilterTextChange,
  onInStockOnlyChange
}) {
  return (
    <form>
      <input 
        type="text" 
        value={filterText} placeholder="Search..." 
        onChange={(e) => onFilterTextChange(e.target.value)} />
      <label>
        <input 
          type="checkbox" 
          checked={inStockOnly} 
          onChange={(e) => onInStockOnlyChange(e.target.checked)} />
        {' '}
        Only show products in stock
      </label>
    </form>
  );
}

const PRODUCTS = [
  {category: "Fruits", price: "$1", stocked: true, name: "Apple"},
  {category: "Fruits", price: "$1", stocked: true, name: "Dragonfruit"},
  {category: "Fruits", price: "$2", stocked: false, name: "Passionfruit"},
  {category: "Vegetables", price: "$2", stocked: true, name: "Spinach"},
  {category: "Vegetables", price: "$4", stocked: false, name: "Pumpkin"},
  {category: "Vegetables", price: "$1", stocked: true, name: "Peas"}
];

export default function App() {
  return <FilterableProductTable products={PRODUCTS} />;
}
```

```css
body {
  padding: 5px
}
label {
  display: block;
  margin-top: 5px;
  margin-bottom: 5px;
}
th {
  padding: 4px;
}
td {
  padding: 2px;
}
```

</Sandpack>

Unaweza kujifunza yote kuhusu kushughulikia matukio na hali ya kusasisha katika sehemu ya [Kuongeza Mwingiliano](/learn/adding-interactivity).

## Uende wapi kutoka hapa {/*where-to-go-from-here*/}

Huu ulikuwa utangulizi mfupi sana wa jinsi ya kuwaza juu ya kuunda vijenzi na programu ukitumia React. Unaweza [kuanzisha mradi wa React](/learn/installation) sasa hivi au [kupiga mbizi zaidi kwenye sintaksia yote](/learn/describing-the-ui) iliyotumika katika funzo hili.
