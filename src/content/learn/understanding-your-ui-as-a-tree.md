---
title: Kuelewa UI Yako kama Mti
---

<Intro>

Programu yako ya React inachukua sura huku components (vipengele) nyingi zikipachikwa ndani ya nyingine. Je, React huwezaje kufuatilia muundo wa components za programu yako?

React, na maktaba nyingine nyingi za UI, huiga UI kama mti (tree). Kuifikiria programu yako kama mti ni jambo la manufaa kwa kuelewa uhusiano kati ya components. Uelewa huu utakusaidia kutatua dhana za baadaye kama utendaji (performance) na usimamizi wa state (hali).

</Intro>

<YouWillLearn>

* Jinsi React "inavyoona" miundo ya components
* Render tree ni nini na ina manufaa gani
* Module dependency tree ni nini na ina manufaa gani

</YouWillLearn>

## UI yako kama mti {/*your-ui-as-a-tree*/}

Miti ni muundo wa uhusiano kati ya vitu. UI mara nyingi huwakilishwa kwa kutumia miundo ya mti. Kwa mfano, vivinjari hutumia miundo ya mti kuiga HTML ([DOM](https://developer.mozilla.org/docs/Web/API/Document_Object_Model/Introduction)) na CSS ([CSSOM](https://developer.mozilla.org/docs/Web/API/CSS_Object_Model)). Mifumo ya simu za mkononi pia hutumia miti kuwakilisha ngazi ya mionekano yao.

<Diagram name="preserving_state_dom_tree" height={193} width={864} alt="Diagram with three sections arranged horizontally. In the first section, there are three rectangles stacked vertically, with labels 'Component A', 'Component B', and 'Component C'. Transitioning to the next pane is an arrow with the React logo on top labeled 'React'. The middle section contains a tree of components, with the root labeled 'A' and two children labeled 'B' and 'C'. The next section is again transitioned using an arrow with the React logo on top labeled 'React DOM'. The third and final section is a wireframe of a browser, containing a tree of 8 nodes, which has only a subset highlighted (indicating the subtree from the middle section).">

React huunda mti wa UI kutoka kwa components zako. Katika mfano huu, mti wa UI kisha hutumika ku-render kwenye DOM.
</Diagram>

Kama vivinjari na mifumo ya simu za mkononi, React pia hutumia miundo ya mti kusimamia na kuiga uhusiano kati ya components katika programu ya React. Miti hii ni zana za manufaa za kuelewa jinsi data inavyopita katika programu ya React na jinsi ya kuboresha ku-render na ukubwa wa programu.

## Render Tree {/*the-render-tree*/}

Sifa kuu ya components ni uwezo wa kutunga components kutoka kwa components nyingine. Tunapo[pachika components](/learn/your-first-component#nesting-and-organizing-components), tunapata dhana ya components-mzazi na components-mtoto, ambapo kila component-mzazi yenyewe inaweza kuwa mtoto wa component nyingine.

Tunapo-render programu ya React, tunaweza kuiga uhusiano huu katika mti, ujulikanao kama render tree.

Hii hapa ni programu ya React inayo-render nukuu za kutia moyo.

<Sandpack>

```js src/App.js
import FancyText from './FancyText';
import InspirationGenerator from './InspirationGenerator';
import Copyright from './Copyright';

export default function App() {
  return (
    <>
      <FancyText title text="Get Inspired App" />
      <InspirationGenerator>
        <Copyright year={2004} />
      </InspirationGenerator>
    </>
  );
}

```

```js src/FancyText.js
export default function FancyText({title, text}) {
  return title
    ? <h1 className='fancy title'>{text}</h1>
    : <h3 className='fancy cursive'>{text}</h3>
}
```

```js src/InspirationGenerator.js
import * as React from 'react';
import quotes from './quotes';
import FancyText from './FancyText';

export default function InspirationGenerator({children}) {
  const [index, setIndex] = React.useState(0);
  const quote = quotes[index];
  const next = () => setIndex((index + 1) % quotes.length);

  return (
    <>
      <p>Your inspirational quote is:</p>
      <FancyText text={quote} />
      <button onClick={next}>Inspire me again</button>
      {children}
    </>
  );
}
```

```js src/Copyright.js
export default function Copyright({year}) {
  return <p className='small'>©️ {year}</p>;
}
```

```js src/quotes.js
export default [
  "Don’t let yesterday take up too much of today.” — Will Rogers",
  "Ambition is putting a ladder against the sky.",
  "A joy that's shared is a joy made double.",
  ];
```

```css
.fancy {
  font-family: 'Georgia';
}
.title {
  color: #007AA3;
  text-decoration: underline;
}
.cursive {
  font-style: italic;
}
.small {
  font-size: 10px;
}
```

</Sandpack>

<Diagram name="render_tree" height={250} width={500} alt="Tree graph with five nodes. Each node represents a component. The root of the tree is App, with two arrows extending from it to 'InspirationGenerator' and 'FancyText'. The arrows are labelled with the word 'renders'. 'InspirationGenerator' node also has two arrows pointing to nodes 'FancyText' and 'Copyright'.">

React huunda *render tree*, mti wa UI, uliotungwa na components zilizo-render.


</Diagram>

Kutoka kwa programu ya mfano, tunaweza kujenga render tree iliyo hapo juu.

Mti umetungwa na nodi, ambapo kila mmoja huwakilisha component. `App`, `FancyText`, `Copyright`, kwa kutaja chache, zote ni nodi katika mti wetu.

Nodi ya mzizi (root) katika render tree ya React ni [component-mzizi](/learn/importing-and-exporting-components#the-root-component-file) ya programu. Katika hali hii, component-mzizi ni `App` na ndiyo component ya kwanza ambayo React inai-render. Kila mshale katika mti unaelekeza kutoka component-mzazi hadi component-mtoto.

<DeepDive>

#### Tagi za HTML ziko wapi katika render tree? {/*where-are-the-html-elements-in-the-render-tree*/}

Utagundua kwamba katika render tree iliyo hapo juu, hakuna taja ya tagi za HTML ambazo kila component ina-render. Hii ni kwa sababu render tree imetungwa tu na [components](learn/your-first-component#components-ui-building-blocks) za React.

React, kama framework ya UI, haitegemei mfumo mahususi (platform agnostic). Kwenye react.dev, tunaonyesha mifano inayo-render kwenye wavuti, ambayo hutumia markup ya HTML kama vipengele vyake vya msingi vya UI. Lakini programu ya React ingeweza vilevile ku-render kwenye mfumo wa simu au wa kompyuta ya mezani, ambao unaweza kutumia vipengele tofauti vya msingi vya UI kama [UIView](https://developer.apple.com/documentation/uikit/uiview) au [FrameworkElement](https://learn.microsoft.com/en-us/dotnet/api/system.windows.frameworkelement?view=windowsdesktop-7.0).

Vipengele hivi vya msingi vya UI vya mfumo si sehemu ya React. Render trees za React zinaweza kutoa mwanga kuhusu programu yako ya React bila kujali ni mfumo gani programu yako inao-render.

</DeepDive>

Render tree huwakilisha pasi moja ya ku-render ya programu ya React. Kwa [ku-render kwa masharti](/learn/conditional-rendering), component-mzazi inaweza ku-render watoto tofauti kutegemeana na data iliyopitishwa.

Tunaweza kusasisha programu ili ku-render kwa masharti ama nukuu ya kutia moyo au rangi.

<Sandpack>

```js src/App.js
import FancyText from './FancyText';
import InspirationGenerator from './InspirationGenerator';
import Copyright from './Copyright';

export default function App() {
  return (
    <>
      <FancyText title text="Get Inspired App" />
      <InspirationGenerator>
        <Copyright year={2004} />
      </InspirationGenerator>
    </>
  );
}

```

```js src/FancyText.js
export default function FancyText({title, text}) {
  return title
    ? <h1 className='fancy title'>{text}</h1>
    : <h3 className='fancy cursive'>{text}</h3>
}
```

```js src/Color.js
export default function Color({value}) {
  return <div className="colorbox" style={{backgroundColor: value}} />
}
```

```js src/InspirationGenerator.js
import * as React from 'react';
import inspirations from './inspirations';
import FancyText from './FancyText';
import Color from './Color';

export default function InspirationGenerator({children}) {
  const [index, setIndex] = React.useState(0);
  const inspiration = inspirations[index];
  const next = () => setIndex((index + 1) % inspirations.length);

  return (
    <>
      <p>Your inspirational {inspiration.type} is:</p>
      {inspiration.type === 'quote'
      ? <FancyText text={inspiration.value} />
      : <Color value={inspiration.value} />}

      <button onClick={next}>Inspire me again</button>
      {children}
    </>
  );
}
```

```js src/Copyright.js
export default function Copyright({year}) {
  return <p className='small'>©️ {year}</p>;
}
```

```js src/inspirations.js
export default [
  {type: 'quote', value: "Don’t let yesterday take up too much of today.” — Will Rogers"},
  {type: 'color', value: "#B73636"},
  {type: 'quote', value: "Ambition is putting a ladder against the sky."},
  {type: 'color', value: "#256266"},
  {type: 'quote', value: "A joy that's shared is a joy made double."},
  {type: 'color', value: "#F9F2B4"},
];
```

```css
.fancy {
  font-family: 'Georgia';
}
.title {
  color: #007AA3;
  text-decoration: underline;
}
.cursive {
  font-style: italic;
}
.small {
  font-size: 10px;
}
.colorbox {
  height: 100px;
  width: 100px;
  margin: 8px;
}
```
</Sandpack>

<Diagram name="conditional_render_tree" height={250} width={561} alt="Tree graph with six nodes. The top node of the tree is labelled 'App' with two arrows extending to nodes labelled 'InspirationGenerator' and 'FancyText'. The arrows are solid lines and are labelled with the word 'renders'. 'InspirationGenerator' node also has three arrows. The arrows to nodes 'FancyText' and 'Color' are dashed and labelled with 'renders?'. The last arrow points to the node labelled 'Copyright' and is solid and labelled with 'renders'.">

Kwa ku-render kwa masharti, katika ku-render tofauti, render tree inaweza ku-render components tofauti.

</Diagram>

Katika mfano huu, kutegemeana na `inspiration.type` ni nini, tunaweza ku-render `<FancyText>` au `<Color>`. Render tree inaweza kuwa tofauti kwa kila pasi ya ku-render.

Ingawa render trees zinaweza kutofautiana katika pasi tofauti za ku-render, miti hii kwa ujumla ni ya manufaa kwa kutambua ni components zipi za *ngazi ya juu* na *components za majani (leaf)* katika programu ya React. Components za ngazi ya juu ni components zilizo karibu zaidi na component-mzizi na huathiri utendaji wa ku-render wa components zote zilizo chini yao na mara nyingi huwa na uchangamano mkubwa zaidi. Components za majani ziko karibu na chini ya mti na hazina components-watoto na mara nyingi hu-re-render mara kwa mara.

Kutambua makundi haya ya components ni ya manufaa kwa kuelewa mtiririko wa data na utendaji wa programu yako.

## Module Dependency Tree {/*the-module-dependency-tree*/}

Uhusiano mwingine katika programu ya React unaoweza kuigwa kwa mti ni utegemezi wa module za programu. Tunapo[gawanya components zetu](/learn/importing-and-exporting-components#exporting-and-importing-a-component) na mantiki katika faili tofauti, tunaunda [module za JS](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) ambapo tunaweza ku-export components, functions, au constants.

Kila nodi katika module dependency tree ni module na kila tawi huwakilisha kauli ya `import` katika module hiyo.

Tukichukua programu ya Inspirations iliyotangulia, tunaweza kujenga module dependency tree, au dependency tree kwa ufupi.

<Diagram name="module_dependency_tree" height={250} width={658} alt="A tree graph with seven nodes. Each node is labelled with a module name. The top level node of the tree is labelled 'App.js'. There are three arrows pointing to the modules 'InspirationGenerator.js', 'FancyText.js' and 'Copyright.js' and the arrows are labelled with 'imports'. From the 'InspirationGenerator.js' node, there are three arrows that extend to three modules: 'FancyText.js', 'Color.js', and 'inspirations.js'. The arrows are labelled with 'imports'.">

Module dependency tree ya programu ya Inspirations.

</Diagram>

Nodi ya mzizi (root) ya mti ni module ya mzizi, ijulikanayo pia kama faili ya kuingilia (entrypoint file). Mara nyingi ni module inayoshikilia component-mzizi.

Ukilinganisha na render tree ya programu ileile, kuna miundo inayofanana lakini kuna tofauti chache za kuzingatia:

* Nodi zinazounda mti huwakilisha module, si components.
* Module zisizo za components, kama `inspirations.js`, pia huwakilishwa katika mti huu. Render tree inashikilia components pekee.
* `Copyright.js` inaonekana chini ya `App.js` lakini katika render tree, `Copyright`, component yenyewe, inaonekana kama mtoto wa `InspirationGenerator`. Hii ni kwa sababu `InspirationGenerator` inakubali JSX kama [children props](/learn/passing-props-to-a-component#passing-jsx-as-children), hivyo inai-render `Copyright` kama component-mtoto lakini haii-import module.

Dependency trees ni za manufaa kubaini ni module zipi zinahitajika kuendesha programu yako ya React. Unapojenga programu ya React kwa ajili ya uzalishaji (production), kwa kawaida kuna hatua ya kujenga itakayounganisha JavaScript yote inayohitajika kupeleka kwa mteja. Zana inayohusika na hili inaitwa [bundler](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Understanding_client-side_tools/Overview#the_modern_tooling_ecosystem), na bundlers zitatumia dependency tree kubaini ni module zipi zinapaswa kujumuishwa.

Kadri programu yako inavyokua, mara nyingi ukubwa wa bundle pia hukua. Ukubwa mkubwa wa bundle ni gharama kwa mteja kupakua na kuendesha. Ukubwa mkubwa wa bundle unaweza kuchelewesha muda wa UI yako kuchorwa. Kupata mwelekeo wa dependency tree ya programu yako kunaweza kusaidia katika kutatua matatizo haya.

[comment]: <> (perhaps we should also deep dive on conditional imports)

<Recap>

* Miti ni njia ya kawaida ya kuwakilisha uhusiano kati ya vitu. Mara nyingi hutumika kuiga UI.
* Render trees huwakilisha uhusiano wa upachikaji kati ya components za React katika ku-render moja.
* Kwa ku-render kwa masharti, render tree inaweza kubadilika katika ku-render tofauti. Kwa thamani tofauti za props, components zinaweza ku-render components-watoto tofauti.
* Render trees husaidia kutambua ni components zipi za ngazi ya juu na za majani. Components za ngazi ya juu huathiri utendaji wa ku-render wa components zote zilizo chini yao na components za majani mara nyingi hu-re-render mara kwa mara. Kuzitambua ni ya manufaa kwa kuelewa na kutatua utendaji wa ku-render.
* Dependency trees huwakilisha utegemezi wa module katika programu ya React.
* Dependency trees hutumiwa na zana za kujenga kuunganisha msimbo unaohitajika kupeleka programu.
* Dependency trees ni za manufaa kwa kutatua ukubwa mkubwa wa bundle unaochelewesha muda wa kuchora na kufichua fursa za kuboresha ni msimbo upi unaounganishwa.

</Recap>

[TODO]: <> (Add challenges)
