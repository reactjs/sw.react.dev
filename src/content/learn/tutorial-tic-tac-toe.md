---
title: 'Mafunzo: Tic-Tac-Toe'
---

<Intro>

Utaunda mchezo mdogo wa tic-tac-toe wakati wa mafunzo haya. Mafunzo haya hayachukui maarifa yoyote yaliyopo ya React. Mbinu utakazojifunza katika mafunzo ni muhimu ili kuunda programu yoyote ya React, na kuielewa kikamilifu kutakupa uelewa wa kina wa React.

</Intro>

<Note>

Mafunzo haya yameundwa kwa ajili ya watu wanaopendelea **kujifunza kwa kufanya** na wanataka kujaribu haraka kufanya kitu kinachoonekana. Ikiwa unapendelea kujifunza kila dhana hatua kwa hatua, anza na [Kuelezea UI.](/learn/describing-the-ui)

</Note>

Mafunzo haya yamegawanywa katika sehemu kadhaa:

- [Mpanglio wa mafunzo](#setup-for-the-tutorial) utakupa **sehemu ya kuanzia** kufuata mafunzo.
- [Muhtasari](#overview) utakufundisha **misingi** ya React: vipengele, props, na hali.
- [Kukamilisha mchezo](#completing-the-game) itakufundisha **mbinu za kawaida zaidi** katika maendeleo ya React.
- [Kuongeza safari ya muda](#adding-time-travel) itakupa **ufahamu wa kina zaidi** wa nguvu za kipekee za React.

### Unajenga nini? {/*what-are-you-building*/}

Katika mafunzo haya, utatengeneza mchezo wa tic-tac-toe unaoshirikiana na React.

Unaweza kuona jinsi utakavyokamilisha mradi wako hapa:

<Sandpack>

```js src/App.js
import { useState } from 'react';

function Square({ value, onSquareClick }) {
  return (
    <button className="square" onClick={onSquareClick}>
      {value}
    </button>
  );
}

function Board({ xIsNext, squares, onPlay }) {
  function handleClick(i) {
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    const nextSquares = squares.slice();
    if (xIsNext) {
      nextSquares[i] = 'X';
    } else {
      nextSquares[i] = 'O';
    }
    onPlay(nextSquares);
  }

  const winner = calculateWinner(squares);
  let status;
  if (winner) {
    status = 'Winner: ' + winner;
  } else {
    status = 'Next player: ' + (xIsNext ? 'X' : 'O');
  }

  return (
    <>
      <div className="status">{status}</div>
      <div className="board-row">
        <Square value={squares[0]} onSquareClick={() => handleClick(0)} />
        <Square value={squares[1]} onSquareClick={() => handleClick(1)} />
        <Square value={squares[2]} onSquareClick={() => handleClick(2)} />
      </div>
      <div className="board-row">
        <Square value={squares[3]} onSquareClick={() => handleClick(3)} />
        <Square value={squares[4]} onSquareClick={() => handleClick(4)} />
        <Square value={squares[5]} onSquareClick={() => handleClick(5)} />
      </div>
      <div className="board-row">
        <Square value={squares[6]} onSquareClick={() => handleClick(6)} />
        <Square value={squares[7]} onSquareClick={() => handleClick(7)} />
        <Square value={squares[8]} onSquareClick={() => handleClick(8)} />
      </div>
    </>
  );
}

export default function Game() {
  const [history, setHistory] = useState([Array(9).fill(null)]);
  const [currentMove, setCurrentMove] = useState(0);
  const xIsNext = currentMove % 2 === 0;
  const currentSquares = history[currentMove];

  function handlePlay(nextSquares) {
    const nextHistory = [...history.slice(0, currentMove + 1), nextSquares];
    setHistory(nextHistory);
    setCurrentMove(nextHistory.length - 1);
  }

  function jumpTo(nextMove) {
    setCurrentMove(nextMove);
  }

  const moves = history.map((squares, move) => {
    let description;
    if (move > 0) {
      description = 'Go to move #' + move;
    } else {
      description = 'Go to game start';
    }
    return (
      <li key={move}>
        <button onClick={() => jumpTo(move)}>{description}</button>
      </li>
    );
  });

  return (
    <div className="game">
      <div className="game-board">
        <Board xIsNext={xIsNext} squares={currentSquares} onPlay={handlePlay} />
      </div>
      <div className="game-info">
        <ol>{moves}</ol>
      </div>
    </div>
  );
}

function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}
```

```css src/styles.css
* {
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
  margin: 20px;
  padding: 0;
}

.square {
  background: #fff;
  border: 1px solid #999;
  float: left;
  font-size: 24px;
  font-weight: bold;
  line-height: 34px;
  height: 34px;
  margin-right: -1px;
  margin-top: -1px;
  padding: 0;
  text-align: center;
  width: 34px;
}

.board-row:after {
  clear: both;
  content: '';
  display: table;
}

.status {
  margin-bottom: 10px;
}
.game {
  display: flex;
  flex-direction: row;
}

.game-info {
  margin-left: 20px;
}
```

</Sandpack>

Ikiwa msimbo bado haukueleweki kwako, au kama haufahamu sintaksia ya msimbo huo, usijali! Lengo la mafunzo haya ni kukusaidia kuelewa React na sintaksia yake.

Tunapendekeza uangalie mchezo wa tic-tac-toe hapo juu kabla ya kuendelea na mafunzo. Moja ya vipengele utakavyogundua ni kwamba kuna orodha ya namba upande wa kulia wa ubao wa mchezo. Orodha hii inakupa historia ya hatua zote zilizotokea kwenye mchezo, na inasasishwa kadri mchezo unavyoendelea.

Baada ya kucheza na mchezo wa tic-tac-toe uliokamilika, endelea kusoma. Utaanza na kiolezo rahisi zaidi katika mafunzo haya. Hatua yetu inayofuata ni kukuweka tayari ili uanze kujenga mchezo.

## Mpangilio wa mafunzo {/*setup-for-the-tutorial*/}

Katika mhariri wa msimbo wa moja kwa moja hapa chini, bonyeza **Fork** kwenye kona ya juu kulia ili kufungua kihariri katika kichupo kipya ukitumia tovuti ya CodeSandbox. CodeSandbox inakuwezesha kuandika msimbo kwenye kivinjari chako na kuona jinsi watumiaji wako watakavyoona programu uliyounda. Kichupo kipya kinapaswa kuonyesha mraba tupu na msimbo wa mwanzo kwa mafunzo haya.

<Sandpack>

```js src/App.js
export default function Square() {
  return <button className="square">X</button>;
}
```

```css src/styles.css
* {
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
  margin: 20px;
  padding: 0;
}

.square {
  background: #fff;
  border: 1px solid #999;
  float: left;
  font-size: 24px;
  font-weight: bold;
  line-height: 34px;
  height: 34px;
  margin-right: -1px;
  margin-top: -1px;
  padding: 0;
  text-align: center;
  width: 34px;
}

.board-row:after {
  clear: both;
  content: '';
  display: table;
}

.status {
  margin-bottom: 10px;
}
.game {
  display: flex;
  flex-direction: row;
}

.game-info {
  margin-left: 20px;
}
```

</Sandpack>

<Note>

Unaweza pia kufuata mafunzo haya kwa kutumia mazingira yako ya maendeleo ya ndani. Kufanya hivyo, unahitaji:

1. Sakinisha [Node.js](https://nodejs.org/en/)
1. Katika kichupo cha CodeSandbox ulichokifungua awali, bonyeza kitufe kilichopo kona ya juu kushoto kufungua menyu, kisha chagua **Download Sandbox** katika menyu hiyo ili kupakua faili za archive kwenye kompyuta yako
1. Fungua archive, kisha fungua terminal na `cd` hadi directory ulipozipa
1. Sakinisha utegemezi kwa kutumia `npm install`
1. Endesha `npm start` kuanzisha seva ya ndani na fuata maelekezo kuangalia jinsi msimbo unavyotekelezwa kwenye kivinjari

Ikiwa utakwama, usiruhusu hili likusumbue! Fuata mtandaoni badala yake na jaribu kuanzisha tena setup ya ndani baadaye.

</Note>

## Muhtasari {/*overview*/}

Sasa kwamba umejiandaa, hebu tupate muhtasari wa React!

### Kagua msimbo wa mwanzo {/*inspecting-the-starter-code*/}

Katika CodeSandbox utaona sehemu tatu kuu:

![CodeSandbox na msimbo wa mwanzo](../images/tutorial/react-starter-code-codesandbox.png)

1. Sehemu ya _Files_ na orodha ya mafaili kama `App.js`, `index.js`, `styles.css` na folda inayoitwa `public`
1. _Kihariri cha msimbo_ ambapo utaona msimbo wa chanzo wa faili uliyouchagua
1. Sehemu ya _kivinjari_ ambapo utaona jinsi msimbo ulioandika utakavyonyeshwa

Faili ya `App.js` inapaswa kuchaguliwa katika sehemu ya _Files_. Maudhui ya faili hiyo katika _mhariri wa msimbo_ yanapaswa kuwa:

```jsx
export default function Square() {
  return <button className="square">X</button>;
}
```

Sehemu ya _kivinjari_ inapaswa kuonyesha mraba na X ndani yake kama hii:

![mraba wenye X](../images/tutorial/x-filled-square.png)

Sasa hebu tuangalie faili katika msimbo wa mwanzo.

#### `App.js` {/*appjs*/}

Msimbo katika `App.js` unaunda _kiungo (component)_. Katika React, kiungo ni kipande cha msimbo kinachoweza kutumika tena kinachoonyeshea sehemu ya kiolesura cha mtumiaji. Viungo hutumika kutayarisha, kusimamia, na kuboresha vipengele vya UI katika programu yako. Hebu tuangalie kiungo mstari kwa mstari ili kuona kinachotokea:

```js {1}
export default function Square() {
  return <button className="square">X</button>;
}
```

Mstari wa kwanza unafafanua kazi inayoitwa `Square`. Neno la JavaScript la `export` linaufanya kazi huu kupatikana nje ya faili hii. Neno la `default` linawambia faili zingine zinazotumia msimbo wako kwamba hii ndiyo kazi kuu katika faili yako.

```js {2}
export default function Square() {
  return <button className="square">X</button>;
}
```

Mstari wa pili unarudisha kitufe. Neno la JavaScript la `return` linamaanisha chochote kinachokuja baada ya hili kinarudishwa kama thamani kwa mtoaji wa kazi. `<button>` ni *elementi ya JSX*. Elementi ya JSX ni mchanganyiko wa msimbo wa JavaScript na lebo za HTML zinazofafanua kile ungependa kuonyesha. `className="square"` ni sifa ya kitufe au *prop* inayosema CSS jinsi ya kupamba kitufe hicho. `X` ni maandishi yanayoonyeshwa ndani ya kitufe na `</button>` inafunga elementi ya JSX ili kuonyesha kwamba yaliyomo mengine hayapaswi kuwekwa ndani ya kitufe hicho.

#### `styles.css` {/*stylescss*/}

Bonyeza faili inayoitwa `styles.css` katika sehemu ya _Files_ ya CodeSandbox. Faili hii inafafanua mitindo kwa programu yako ya React. Wateuzi wawili wa _CSS_ wa kwanza (`*` na `body`) wanadhibiti mtindo wa sehemu kubwa za programu yako wakati mteuzi wa `.square` unadhibiti mtindo wa kiungo chochote ambapo sifa ya `className` imesetiwa kuwa `square`. Katika msimbo wako, hiyo italingana na kitufe kutoka kwa kiungo cha Square katika faili ya `App.js`.

#### `index.js` {/*indexjs*/}

Bonyeza faili inayoitwa `index.js` katika sehemu ya _Files_ ya CodeSandbox. Hutaedit faili hii wakati wa mafunzo, lakini ni daraja kati ya kiungo ulichokiumba katika faili ya `App.js` na kivinjari cha wavuti.

```jsx
import { StrictMode } from 'react';
import { createRoot } from 'react-dom/client';
import './styles.css';

import App from './App';
```

Mstari wa 1-5 unaleta vipande vyote muhimu pamoja:

* React
* Maktaba ya React ya kuwasiliana na kivinjari cha wavuti (React DOM)
* mitindo ya viungo vyako
* kiungo ulichokiumba katika `App.js`.

Sehemu inayofuata ya faili inaleta vipengele vyote pamoja na kuingiza bidhaa ya mwisho katika `index.html` katika folda ya `public`.

### Kujenga bodi {/*building-the-board*/}

Tunarudi kwenye `App.js`. Hapa ndipo utakapokuwa ukitumia sehemu kubwa ya mafunzo haya.

Kwa sasa bodi ni mraba mmoja tu, lakini unahitaji tisa! Ikiwa utajaribu kunakili na kubandika mraba wako ili kutengeneza mraba mbili kama hii:

```js {2}
export default function Square() {
  return <button className="square">X</button><button className="square">X</button>;
}
```

Utapata makosa haya:

<ConsoleBlock level="error">

/src/App.js: Adjacent JSX elements must be wrapped in an enclosing tag. Did you want a JSX Fragment `<>...</>`?

</ConsoleBlock>

Viungo vya React vinahitaji kurudisha elementi moja ya JSX na sio elementi nyingi za JSX zilizo karibu kama vitufe viwili. Ili kutatua hili, unaweza kutumia *Fragments* (`<>` na `</>`) ili kuzunguka elementi nyingi za JSX zilizo karibu kama hii:

```js {3-6}
export default function Square() {
  return (
    <>
      <button className="square">X</button>
      <button className="square">X</button>
    </>
  );
}
```

Sasa unapaswa kuona:

![vitufe viwili vyenye X](../images/tutorial/two-x-filled-squares.png)

Vizuri! Sasa unahitaji tu kunakili na kubandika mara kadhaa ili kuongeza mraba tisa na...

![miraba tisa yenye X katika mstari](../images/tutorial/nine-x-filled-squares.png)

Hapana! Miraba yote ziko katika mstari mmoja tu, si katika gridi kama inavyohitajika kwa bodi yetu. Ili kutatua hili, utahitaji kundi la mraba zako katika safu na `div`s na kuongeza baadhi ya madarasa ya CSS. Wakati huo huo, utampa kila mraba nambari ili kuhakikisha unajua kila mraba unapoonyeshwa.

Katika faili ya `App.js`, sasisha kiungo cha `Square` ili kiwe kama hiki:

```js {3-19}
export default function Square() {
  return (
    <>
      <div className="board-row">
        <button className="square">1</button>
        <button className="square">2</button>
        <button className="square">3</button>
      </div>
      <div className="board-row">
        <button className="square">4</button>
        <button className="square">5</button>
        <button className="square">6</button>
      </div>
      <div className="board-row">
        <button className="square">7</button>
        <button className="square">8</button>
        <button className="square">9</button>
      </div>
    </>
  );
}
```

CSS iliyoainishwa katika `styles.css` inapamba `div`s zenye `className` ya `board-row`. Sasa kwamba umejumuisha viungo vyako katika safu kwa kutumia `div`s zilizopambwa, sasa una bodi yako ya tic-tac-toe:

![bodi ya tic-tac-toe iliyojaa nambari kutoka 1 hadi 9](../images/tutorial/number-filled-board.png)

Lakini sasa una tatizo. Kiungo chako kiitwacho `Square`, kweli hakiko tena kama mraba. Hebu tufanye mabadiliko kwa kubadili jina na kuwa `Board`:

```js {1}
export default function Board() {
  //...
}
```

Katika hatua hii, msimbo wako unapaswa kuonekana namna hii:

<Sandpack>

```js
export default function Board() {
  return (
    <>
      <div className="board-row">
        <button className="square">1</button>
        <button className="square">2</button>
        <button className="square">3</button>
      </div>
      <div className="board-row">
        <button className="square">4</button>
        <button className="square">5</button>
        <button className="square">6</button>
      </div>
      <div className="board-row">
        <button className="square">7</button>
        <button className="square">8</button>
        <button className="square">9</button>
      </div>
    </>
  );
}
```

```css src/styles.css
* {
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
  margin: 20px;
  padding: 0;
}

.square {
  background: #fff;
  border: 1px solid #999;
  float: left;
  font-size: 24px;
  font-weight: bold;
  line-height: 34px;
  height: 34px;
  margin-right: -1px;
  margin-top: -1px;
  padding: 0;
  text-align: center;
  width: 34px;
}

.board-row:after {
  clear: both;
  content: '';
  display: table;
}

.status {
  margin-bottom: 10px;
}
.game {
  display: flex;
  flex-direction: row;
}

.game-info {
  margin-left: 20px;
}
```

</Sandpack>

<Note>

Psssst... Kuna ni mengi ya kuandika! Ni sawa kunakili na kubandika msimbo kutoka ukurasa huu. Hata hivyo, ikiwa uko tayari kwa changamoto kidogo, tunapendekeza kunakili tu msimbo ambao umekuandika mwenyewe angalau mara moja.

</Note>

### Kupitisha data kupitia props {/*passing-data-through-props*/}

Ifuatayo, utataka kubadili thamani ya mraba kutoka tupu hadi "X" wakati mtumiaji anabofya kwenye mraba. Kwa jinsi ulivyounda bodi hadi sasa, itabidi unakili na kubandika msimbo unaosasisha mraba mara tisa (moja kwa kila mraba ulionao)! Badala ya kunakili na kubandika, usanifu wa viungo vya React unakuwezesha kuunda kiungo kinachoweza kutumika tena ili kuepuka msimbo unaojirudia.

Kwanza, utachukua mstari unaofafanua mraba wako wa kwanza (`<button className="square">1</button>`) kutoka kwa kiungo chako cha `Board` na kuhamisha katika kiungo kipya cha `Square`:

```js {1-3}
function Square() {
  return <button className="square">1</button>;
}

export default function Board() {
  // ...
}
```

Kisha utasasisha kiungo cha Board ili kionyeshe kiungo hicho cha `Square` kwa kutumia sintaksia ya JSX:

```js {5-19}
// ...
export default function Board() {
  return (
    <>
      <div className="board-row">
        <Square />
        <Square />
        <Square />
      </div>
      <div className="board-row">
        <Square />
        <Square />
        <Square />
      </div>
      <div className="board-row">
        <Square />
        <Square />
        <Square />
      </div>
    </>
  );
}
```

Angalia jinsi, kinyume na `div`s za kivinjari, viungo vyako `Board` na `Square` lazima vianze na herufi kubwa.

Hebu tuangalie:

![bodi moja iliyojaa](../images/tutorial/board-filled-with-ones.png)

Hapana! Ulipoteza mraba yenye nambari ulizokuwa nazo awali. Sasa kila mraba inasema "1". Ili kutatua hili, utatumia *props* kupitisha thamani ambayo kila mraba inapaswa kuwa nayo kutoka kwa kiungo cha mzazi (`Board`) hadi kwa mtoto wake (`Square`).

Sasasisha kiungo cha `Square` ili kisome prop ya `value` ambayo utapitia kutoka kwa `Board`:

```js {1}
function Square({ value }) {
  return <button className="square">1</button>;
}
```

`function Square({ value })` inaonyesha kuwa kiungo cha Square kinaweza kupitishiwa prop iitwayo `value`.

Sasa unataka kuonyesha hiyo `value` badala ya `1` ndani ya kila mraba. Jaribu kufanya hivyo kama hivi:

```js {2}
function Square({ value }) {
  return <button className="square">value</button>;
}
```

Ooh, hii siyo unachotaka:

![bodi yenye value](../images/tutorial/board-filled-with-value.png)

Ulitaka kutekeleza variable ya JavaScript iitwayo `value` kutoka kwa kiungo chako, sio neno "value". Ili "kutoroka kwenda JavaScript" kutoka kwa JSX, unahitaji mabano ya curly. Ongeza mabano ya curly kuzunguka `value` katika JSX kama hivi:

```js {2}
function Square({ value }) {
  return <button className="square">{value}</button>;
}
```

Kwa sasa, unapaswa kuona bodi tupu:

![bodi tupu](../images/tutorial/empty-board.png)

Hii ni kwa sababu kiungo cha `Board` hakijapitisha prop ya `value` kwa kila kiungo cha `Square` kinachochora bado. Ili kutatua hili, utaongeza prop ya `value` kwa kila kiungo cha `Square` kinachochora na `Board`:

```js {5-7,10-12,15-17}
export default function Board() {
  return (
    <>
      <div className="board-row">
        <Square value="1" />
        <Square value="2" />
        <Square value="3" />
      </div>
      <div className="board-row">
        <Square value="4" />
        <Square value="5" />
        <Square value="6" />
      </div>
      <div className="board-row">
        <Square value="7" />
        <Square value="8" />
        <Square value="9" />
      </div>
    </>
  );
}
```

Sasa unapaswa kuona gridi ya nambari tena:

![bodi ya tic-tac-toe iliyojaa nambari kutoka 1 hadi 9](../images/tutorial/number-filled-board.png)

Msimbo wako wa sasisho unapaswa kuonekana kama huu:

<Sandpack>

```js src/App.js
function Square({ value }) {
  return <button className="square">{value}</button>;
}

export default function Board() {
  return (
    <>
      <div className="board-row">
        <Square value="1" />
        <Square value="2" />
        <Square value="3" />
      </div>
      <div className="board-row">
        <Square value="4" />
        <Square value="5" />
        <Square value="6" />
      </div>
      <div className="board-row">
        <Square value="7" />
        <Square value="8" />
        <Square value="9" />
      </div>
    </>
  );
}
```

```css src/styles.css
* {
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
  margin: 20px;
  padding: 0;
}

.square {
  background: #fff;
  border: 1px solid #999;
  float: left;
  font-size: 24px;
  font-weight: bold;
  line-height: 34px;
  height: 34px;
  margin-right: -1px;
  margin-top: -1px;
  padding: 0;
  text-align: center;
  width: 34px;
}

.board-row:after {
  clear: both;
  content: '';
  display: table;
}

.status {
  margin-bottom: 10px;
}
.game {
  display: flex;
  flex-direction: row;
}

.game-info {
  margin-left: 20px;
}
```

</Sandpack>

### Kutengeneza kipengele kinachoshirishi {/*making-an-interactive-component*/}

Hebu tujaze kipengele cha `Square` na `X` unapoibofya. Tangaza kazi (function) iitwayo `handleClick` ndani ya `Square`. Kisha, ongeza `onClick` kwenye props za kipengele cha button kilichorejeshwa kutoka kwa `Square`:

```js {2-4,9}
function Square({ value }) {
  function handleClick() {
    console.log('clicked!');
  }

  return (
    <button
      className="square"
      onClick={handleClick}
    >
      {value}
    </button>
  );
}
```

Ukibofya mraba sasa, utaona ujumbe wa log `"clicked!"` kwenye tabo ya _Console_ chini ya sehemu ya _Browser_ katika CodeSandbox. Kubofya mraba zaidi ya mara moja kutaonyesha tena `"clicked!"`. Log zinazojirudia zenye ujumbe sawa hazitaongeza mistari zaidi kwenye console. Badala yake, utaona kaunta inayoongezeka karibu na log yako ya kwanza `"clicked!"`.

<Note>

Ikiwa unafuata mafunzo haya kwa kutumia mazingira ya maendeleo ya ndani, unahitaji kufungua Console ya kivinjari chako. Kwa mfano, ikiwa unatumia kivinjari cha Chrome, unaweza kuona Console kwa njia ya mkato ya kibodi **Shift + Ctrl + J** (kwenye Windows/Linux) au **Option + ⌘ + J** (kwenye macOS).

</Note>

Hatua inayofuata, unataka kipengele cha Square "kumbuka" kuwa kilibofya, na kujaza kwa alama ya "X". Ili "kukumbuka" mambo, vipengele hutumia *hali* (*state*).

React inatoa kazi maalum iitwayo `useState` ambayo unaweza kuiita kutoka kwa kipengele chako ili kuifanya "ikumbuke" mambo. Hebu tuweke thamani ya sasa ya `Square` katika hali, na tuiibadilishe mraba inapobofya.

Ingiza `useState` juu ya faili. Ondoa prop ya `value` kutoka kwenye kipengele cha `Square`. Badala yake, ongeza mstari mpya mwanzoni mwa `Square` unaoita `useState`. Iwe irudishe hali inayoitwa `value`:

```js {1,3,4}
import { useState } from 'react';

function Square() {
  const [value, setValue] = useState(null);

  function handleClick() {
    //...
```

`value` huhifadhi thamani na `setValue` ni kazi inayoweza kutumika kubadilisha thamani. `null` inayotumwa kwa `useState` inatumika kama thamani ya awali ya hali hii, kwa hivyo `value` hapa inaanza ikiwa sawa na `null`.

Kwa kuwa kipengele cha `Square` hakikubali tena props, utaondoa prop ya `value` kutoka kwa vipengele vyote tisa vya `Square` vilivyoundwa na kipengele cha Board:

```js {6-8,11-13,16-18}
// ...
export default function Board() {
  return (
    <>
      <div className="board-row">
        <Square />
        <Square />
        <Square />
      </div>
      <div className="board-row">
        <Square />
        <Square />
        <Square />
      </div>
      <div className="board-row">
        <Square />
        <Square />
        <Square />
      </div>
    </>
  );
}
```

Sasa utabadilisha `Square` ili kuonyesha "X" inapobofya. Badilisha mshughulikaji wa tukio `console.log("clicked!");` na `setValue('X');`. Sasa kipengele chako cha `Square` kinaonekana hivi:

```js {5}
function Square() {
  const [value, setValue] = useState(null);

  function handleClick() {
    setValue('X');
  }

  return (
    <button
      className="square"
      onClick={handleClick}
    >
      {value}
    </button>
  );
}
```

Kwa kuwaita kazi hii ya `set` kutoka kwa mshughulikaji wa `onClick`, unaiambia React ifanye upya utengenezaji wa `Square` kila wakati `<button>` yake inapobofya. Baada ya sasisho, `value` ya `Square` itakuwa `'X'`, kwa hivyo utaona "X" kwenye ubao wa mchezo. Bofya Square yoyote, na "X" itaonekana:

![Kuongeza X kwenye ubao](../images/tutorial/tictac-adding-x-s.gif)

Kila Square ina hali yake: thamani ya `value` iliyohifadhiwa kwenye kila Square ni huru kabisa kwa nyinginezo. Unapobadilisha hali kwa kuita kazi ya `set`, React huboresha pia vipengele vya watoto vilivyo ndani. 

Baada ya mabadiliko haya, msimbo wako utaonekana hivi:

<Sandpack>

```js src/App.js
import { useState } from 'react';

function Square() {
  const [value, setValue] = useState(null);

  function handleClick() {
    setValue('X');
  }

  return (
    <button
      className="square"
      onClick={handleClick}
    >
      {value}
    </button>
  );
}

export default function Board() {
  return (
    <>
      <div className="board-row">
        <Square />
        <Square />
        <Square />
      </div>
      <div className="board-row">
        <Square />
        <Square />
        <Square />
      </div>
      <div className="board-row">
        <Square />
        <Square />
        <Square />
      </div>
    </>
  );
}
```

```css src/styles.css
* {
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
  margin: 20px;
  padding: 0;
}

.square {
  background: #fff;
  border: 1px solid #999;
  float: left;
  font-size: 24px;
  font-weight: bold;
  line-height: 34px;
  height: 34px;
  margin-right: -1px;
  margin-top: -1px;
  padding: 0;
  text-align: center;
  width: 34px;
}

.board-row:after {
  clear: both;
  content: '';
  display: table;
}

.status {
  margin-bottom: 10px;
}
.game {
  display: flex;
  flex-direction: row;
}

.game-info {
  margin-left: 20px;
}
```

</Sandpack>

### Zana za Watengenezaji wa React {/*react-developer-tools*/}

React DevTools hukuruhusu kuangalia *props* na *state* za vipengele vyako vya React. Unaweza kupata kichupo cha React DevTools chini ya sehemu ya *browser* katika CodeSandbox:

![React DevTools katika CodeSandbox](../images/tutorial/codesandbox-devtools.png)

Ili kuchunguza kipengele fulani kwenye skrini, tumia kitufe kilicho kwenye kona ya juu kushoto ya React DevTools:

![Kuchagua vipengele kwenye ukurasa na React DevTools](../images/tutorial/devtools-select.gif)

<Note>

Kwa maendeleo ya ndani (*local development*), React DevTools inapatikana kama [Chrome](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en), [Firefox](https://addons.mozilla.org/en-US/firefox/addon/react-devtools/), na [Edge](https://microsoftedge.microsoft.com/addons/detail/react-developer-tools/gpphkfbcpidddadnkolkpfckpihlkkil) *browser extension*. Isakinishe, na kichupo cha *Components* kitaonekana kwenye Zana za Watengenezaji wa kivinjari chako kwa tovuti zinazotumia React.

</Note>

## Kukamilisha Mchezo {/*completing-the-game*/}

Hadi kufikia sasa, unayo sehemu zote za msingi za mchezo wako wa *tic-tac-toe*. Ili kukamilisha mchezo, unahitaji kubadilisha kuweka "X" na "O" kwenye ubao, na pia unahitaji njia ya kuamua mshindi.

### Kuinua *Hali* Juu {/*lifting-state-up*/}

Kwa sasa, kila kipengele cha `Square` kinahifadhi sehemu ya *state* ya mchezo. Ili kuangalia mshindi kwenye mchezo wa *tic-tac-toe*, kipengele cha `Board` kitalazimika kujua hali ya kila moja ya vipengele 9 vya `Square`.

Ungeweza kushughulikia hili vipi? Kwanza, unaweza kudhani kwamba `Board` inahitaji "kuuliza" kila `Square` kuhusu hali ya `Square` hiyo. Ingawa mbinu hii inawezekana kimitambo katika React, haipendekezwi kwa sababu msimbo unakuwa mgumu kuelewa, unakabiliwa na hitilafu, na ni vigumu kurekebisha. Badala yake, njia bora ni kuhifadhi *state* ya mchezo katika kipengele cha mzazi `Board` badala ya kila `Square`. Kipengele cha `Board` kinaweza kuambia kila `Square` nini cha kuonyesha kwa kupitisha *prop*, kama ulivyofanya ulipopitisha namba kwa kila `Square`.

**Ili kukusanya data kutoka kwa watoto wengi, au kufanya vipengele viwili vya watoto kuwasiliana, tangaza *hali* iliyoshirikiwa kwenye kipengele cha mzazi badala yake. Kipengele cha mzazi kinaweza kupitisha *hali* hiyo kurudi chini kwa watoto kupitia *props*. Hii husaidia vipengele vya watoto kuwa katika usawa na kila mmoja na mzazi wao.**

Kuinua *state* kwenye kipengele cha mzazi ni jambo la kawaida linapokuja suala la kufanyia marekebisho vipengele vya React.

Hebu tujaribu hili. Hariri kipengele cha `Board` ili kitangaze *state variable* iitwayo `squares` ambayo chaguo-msingi lake ni safu ya null 9 inayolingana na mraba 9:

```js {3}
// ...
export default function Board() {
  const [squares, setSquares] = useState(Array(9).fill(null));
  return (
    // ...
  );
}
```

`Array(9).fill(null)` huunda safu yenye vipengele tisa na huweka kila kimoja sawa na `null`. Wito wa `useState()` unaozunguka hiyo hutangaza *state variable* ya `squares` ambayo awali imewekwa kuwa safu hiyo. Kila ingizo kwenye safu linahusiana na thamani ya mraba. Unapojaza ubao baadaye, safu ya `squares` itaonekana hivi:

```jsx
['O', null, 'X', 'X', 'X', 'O', 'O', null, null]
```

Sasa kipengele chako cha `Board` kinahitaji kupitisha *prop* ya `value` kwenda kwa kila `Square` kinachotengeneza:

```js {6-8,11-13,16-18}
export default function Board() {
  const [squares, setSquares] = useState(Array(9).fill(null));
  return (
    <>
      <div className="board-row">
        <Square value={squares[0]} />
        <Square value={squares[1]} />
        <Square value={squares[2]} />
      </div>
      <div className="board-row">
        <Square value={squares[3]} />
        <Square value={squares[4]} />
        <Square value={squares[5]} />
      </div>
      <div className="board-row">
        <Square value={squares[6]} />
        <Square value={squares[7]} />
        <Square value={squares[8]} />
      </div>
    </>
  );
}
```

Kisha, utahariri kipengele cha `Square` ili kipokee *prop* ya `value` kutoka kwa kipengele cha `Board`. Hii itahitaji kuondoa ufuatiliaji wa hali ya `value` wa ndani wa `Square` na *prop* ya `onClick` ya kitufe:

```js {1,2}
function Square({value}) {
  return <button className="square">{value}</button>;
}
```

Kwa wakati huu, unapaswa kuona ubao wa *tic-tac-toe* tupu:

![Ubao tupu](../images/tutorial/empty-board.png)

Na msimbo wako unapaswa kuonekana hivi:

<Sandpack>

```js src/App.js
import { useState } from 'react';

function Square({ value }) {
  return <button className="square">{value}</button>;
}

export default function Board() {
  const [squares, setSquares] = useState(Array(9).fill(null));
  return (
    <>
      <div className="board-row">
        <Square value={squares[0]} />
        <Square value={squares[1]} />
        <Square value={squares[2]} />
      </div>
      <div className="board-row">
        <Square value={squares[3]} />
        <Square value={squares[4]} />
        <Square value={squares[5]} />
      </div>
      <div className="board-row">
        <Square value={squares[6]} />
        <Square value={squares[7]} />
        <Square value={squares[8]} />
      </div>
    </>
  );
}
```

```css src/styles.css
* {
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
  margin: 20px;
  padding: 0;
}

.square {
  background: #fff;
  border: 1px solid #999;
  float: left;
  font-size: 24px;
  font-weight: bold;
  line-height: 34px;
  height: 34px;
  margin-right: -1px;
  margin-top: -1px;
  padding: 0;
  text-align: center;
  width: 34px;
}

.board-row:after {
  clear: both;
  content: '';
  display: table;
}

.status {
  margin-bottom: 10px;
}
.game {
  display: flex;
  flex-direction: row;
}

.game-info {
  margin-left: 20px;
}
```

</Sandpack>

Kila Square sasa itapokea kipengele cha `value` ambacho kitakuwa aidha `'X'`, `'O'`, au `null` kwa Square ambazo hazijajazwa.

Sasa, unahitaji kubadilisha kile kinachotokea wakati Square inabonyezwa. Sehemu ya `Board` sasa inasimamia ni Square gani zimejazwa. Utahitaji kuunda njia kwa Square kuboresha hali ya `Board`. Kwa kuwa hali ni ya ndani kwa sehemu inayofafanua, huwezi kusasisha hali ya `Board` moja kwa moja kutoka Square.

Badala yake, utapunguza chini kazi kutoka kwa sehemu ya `Board` kwenda kwa sehemu ya `Square`, na utakuwa na `Square` kuita kazi hiyo wakati Square inapobonyezwa. Utaanza na kazi ambayo sehemu ya `Square` itaita inapobonyezwa. Utaita kazi hiyo `onSquareClick`:

```js
function Square({ value }) {
  return (
    <button className="square" onClick={onSquareClick}>
      {value}
    </button>
  );
}
```

Baadaye, utaongeza kazi ya `onSquareClick` kwenye sifa za sehemu ya `Square`:

```js
function Square({ value, onSquareClick }) {
  return (
    <button className="square" onClick={onSquareClick}>
      {value}
    </button>
  );
}
```

Sasa utaunganisha kipengele cha `onSquareClick` na kazi katika sehemu ya `Board` utakayoita `handleClick`. Ili kuunganisha `onSquareClick` na `handleClick`, utapita kazi kwa kipengele cha `onSquareClick` cha sehemu ya kwanza ya `Square`:

```js
export default function Board() {
  const [squares, setSquares] = useState(Array(9).fill(null));

  return (
    <>
      <div className="board-row">
        <Square value={squares[0]} onSquareClick={handleClick} />
        //...
  );
}
```

Hatimaye, utafafanua kazi ya `handleClick` ndani ya sehemu ya Board ili kusasisha safu ya `squares` inayoshikilia hali ya ubao wako:

```js
export default function Board() {
  const [squares, setSquares] = useState(Array(9).fill(null));

  function handleClick() {
    const nextSquares = squares.slice();
    nextSquares[0] = "X";
    setSquares(nextSquares);
  }

  return (
    // ...
  )
}
```

Kazi ya `handleClick` inaunda nakala ya safu ya `squares` (`nextSquares`) kwa kutumia mbinu ya `slice()` ya JavaScript. Kisha, `handleClick` inasasisha safu ya `nextSquares` ili kuongeza `X` kwenye Square ya kwanza (`[0]` index).

Kuita kazi ya `setSquares` kunamwambia React kwamba hali ya sehemu imebadilika. Hii itasababisha urejeshaji wa vipengele vinavyotumia hali ya `squares` (`Board`) pamoja na vipengele vya mtoto wake (vipengele vya `Square` vinavyounda ubao).

<Note>

JavaScript inaunga mkono [closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures) ambayo inamaanisha kazi ya ndani (mfano, `handleClick`) ina ufikiaji wa vigezo na kazi zilizofafanuliwa katika kazi ya nje (mfano, `Board`). Kazi ya `handleClick` inaweza kusoma hali ya `squares` na kuita mbinu ya `setSquares` kwa sababu zote zipo ndani ya kazi ya `Board`.

</Note>

Sasa unaweza kuongeza alama za X kwenye ubao... lakini tu kwenye Square ya juu kushoto. Kazi yako ya `handleClick` imewekewa mpangilio wa kuboresha index ya Square ya juu kushoto (`0`). Hebu tuiboreshe kazi ya `handleClick` ili iweze kuboresha Square yoyote. Ongeza hoja `i` kwenye kazi ya `handleClick` inayochukua index ya Square inayosasishwa:

```js
export default function Board() {
  const [squares, setSquares] = useState(Array(9).fill(null));

  function handleClick(i) {
    const nextSquares = squares.slice();
    nextSquares[i] = "X";
    setSquares(nextSquares);
  }

  return (
    // ...
  )
}
```

Sasa, utahitaji kupitisha `i` kwenda kwa `handleClick`. Unaweza kujaribu kuweka kipengele cha `onSquareClick` cha Square kuwa `handleClick(0)` moja kwa moja ndani ya JSX kama hii, lakini haitafanya kazi:

```jsx
<Square value={squares[0]} onSquareClick={handleClick(0)} />
```

Hii ndiyo sababu haifanyi kazi. Wito wa `handleClick(0)` utakuwa sehemu ya kurender sehemu ya Board. Kwa kuwa `handleClick(0)` hubadilisha hali ya sehemu ya Board kwa kuita `setSquares`, sehemu yako yote ya Board itarender tena. Lakini hii inaendesha tena `handleClick(0)`, ikisababisha mzunguko usio na mwisho:

<ConsoleBlock level="error">

Renders nyingi mno. React inaweka mipaka ya idadi ya renders ili kuzuia mzunguko usio na mwisho.

</ConsoleBlock>

Kwa nini tatizo hili halikutokea awali?

Ulipoitisha `onSquareClick={handleClick}`, ulikuwa unapitisha kazi ya `handleClick` chini kama sifa. Hukuitia! Lakini sasa unaiita kazi hiyo moja kwa moja--angalia mabano katika `handleClick(0)`--na ndiyo maana inaendeshwa mapema. Hutaki *kuiita* `handleClick` mpaka mtumiaji atakapobonyeza!

Ungeweza kutatua hili kwa kuunda kazi kama `handleFirstSquareClick` inayotumia `handleClick(0)`, kazi kama `handleSecondSquareClick` inayotumia `handleClick(1)`, na kadhalika. Ungepitisha (badala ya kuitisha) kazi hizi chini kama sifa kama `onSquareClick={handleFirstSquareClick}`. Hili lingetatuliwa.

Hata hivyo, kufafanua kazi tisa tofauti na kuzipa kila moja jina ni njia ndefu. Badala yake, hebu tufanye hivi:

```js
export default function Board() {
  // ...
  return (
    <>
      <div className="board-row">
        <Square value={squares[0]} onSquareClick={() => handleClick(0)} />
        // ...
  );
}
```

Angalia syntax mpya ya `() =>`. Hapa, `() => handleClick(0)` ni *arrow function,* ambayo ni njia fupi ya kufafanua kazi. Square inapobonyezwa, msimbo baada ya mshale `=>` utaendeshwa, ukipiga simu `handleClick(0)`.

Sasa unahitaji kusasisha Square zingine nane kuziita `handleClick` kutoka kwenye kazi za arrow unazopitisha. Hakikisha kwamba hoja kwa kila wito wa `handleClick` inalingana na index ya Square sahihi:

```js
export default function Board() {
  // ...
  return (
    <>
      <div className="board-row">
        <Square value={squares[0]} onSquareClick={() => handleClick(0)} />
        <Square value={squares[1]} onSquareClick={() => handleClick(1)} />
        <Square value={squares[2]} onSquareClick={() => handleClick(2)} />
      </div>
      <div className="board-row">
        <Square value={squares[3]} onSquareClick={() => handleClick(3)} />
        <Square value={squares[4]} onSquareClick={() => handleClick(4)} />
        <Square value={squares[5]} onSquareClick={() => handleClick(5)} />
      </div>
      <div className="board-row">
        <Square value={squares[6]} onSquareClick={() => handleClick(6)} />
        <Square value={squares[7]} onSquareClick={() => handleClick(7)} />
        <Square value={squares[8]} onSquareClick={() => handleClick(8)} />
      </div>
    </>
  );
};
```

Sasa unaweza tena kuongeza alama za `X` kwenye Square yoyote kwenye ubao kwa kubonyeza juu yake:

![kujaza ubao na X](../images/tutorial/tictac-adding-x-s.gif)

Lakini wakati huu usimamizi wote wa hali unafanywa na sehemu ya `Board`!

Hivi ndivyo msimbo wako unavyopaswa kuonekana:

<Sandpack>

```js src/App.js
import { useState } from 'react';

function Square({ value, onSquareClick }) {
  return (
    <button className="square" onClick={onSquareClick}>
      {value}
    </button>
  );
}

export default function Board() {
  const [squares, setSquares] = useState(Array(9).fill(null));

  function handleClick(i) {
    const nextSquares = squares.slice();
    nextSquares[i] = 'X';
    setSquares(nextSquares);
  }

  return (
    <>
      <div className="board-row">
        <Square value={squares[0]} onSquareClick={() => handleClick(0)} />
        <Square value={squares[1]} onSquareClick={() => handleClick(1)} />
        <Square value={squares[2]} onSquareClick={() => handleClick(2)} />
      </div>
      <div className="board-row">
        <Square value={squares[3]} onSquareClick={() => handleClick(3)} />
        <Square value={squares[4]} onSquareClick={() => handleClick(4)} />
        <Square value={squares[5]} onSquareClick={() => handleClick(5)} />
      </div>
      <div className="board-row">
        <Square value={squares[6]} onSquareClick={() => handleClick(6)} />
        <Square value={squares[7]} onSquareClick={() => handleClick(7)} />
        <Square value={squares[8]} onSquareClick={() => handleClick(8)} />
      </div>
    </>
  );
}
```

```css src/styles.css
* {
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
  margin: 20px;
  padding: 0;
}

.square {
  background: #fff;
  border: 1px solid #999;
  float: left;
  font-size: 24px;
  font-weight: bold;
  line-height: 34px;
  height: 34px;
  margin-right: -1px;
  margin-top: -1px;
  padding: 0;
  text-align: center;
  width: 34px;
}

.board-row:after {
  clear: both;
  content: '';
  display: table;
}

.status {
  margin-bottom: 10px;
}
.game {
  display: flex;
  flex-direction: row;
}

.game-info {
  margin-left: 20px;
}
```

</Sandpack>

Sasa kwamba usimamizi wa hali yako uko kwenye sehemu ya `Board`, sehemu ya mzazi `Board` hupitisha sifa kwa sehemu za mtoto `Square` ili ziweze kuonyeshwa ipasavyo. Unapobonyeza `Square`, sehemu ya mtoto `Square` sasa huomba sehemu ya mzazi `Board` kuboresha hali ya ubao. Hali ya `Board` inaposasishwa, sehemu zote mbili (`Board` na kila mtoto `Square`) hujirejesha moja kwa moja. Kuhifadhi hali ya squares zote kwenye sehemu ya `Board` kutaiwezesha kuamua mshindi baadaye.

Hebu tuangalie tena kinachotokea wakati mtumiaji anapobonyeza square ya juu kushoto kwenye ubao wako ili kuongeza `X`:

1. Kubonyeza square ya juu kushoto huendesha kazi ambayo `button` ilipokea kama sifa yake ya `onClick` kutoka kwa `Square`. Sehemu ya `Square` ilipokea kazi hiyo kama sifa yake ya `onSquareClick` kutoka kwa `Board`. Sehemu ya `Board` ilifafanua kazi hiyo moja kwa moja ndani ya JSX. Inaita `handleClick` ikiwa na hoja ya `0`.
2. `handleClick` hutumia hoja (`0`) kuboresha kipengele cha kwanza cha safu ya `squares` kutoka `null` hadi `X`.
3. Hali ya `squares` ya sehemu ya `Board` ilisasishwa, hivyo `Board` na watoto wake wote hujirejesha. Hii husababisha sifa ya `value` ya sehemu ya `Square` yenye index `0` kubadilika kutoka `null` hadi `X`.

Hatimaye mtumiaji ataona kwamba square ya juu kushoto imebadilika kutoka tupu hadi kuwa na `X` baada ya kubonyeza.

<Note>

Sifa ya `onClick` ya kipengele cha DOM `<button>` ina maana maalum kwa React kwa sababu ni kipengele kilichojengwa ndani. Kwa vipengele vilivyobuniwa kama `Square`, jina ni uamuzi wako. Unaweza kutoa jina lolote kwa sifa ya `onSquareClick` ya `Square` au kazi ya `handleClick` ya `Board`, na msimbo utaendelea kufanya kazi sawa. Katika React, ni kawaida kutumia majina kama `onSomething` kwa sifa zinazowakilisha matukio na `handleSomething` kwa kazi zinazoshughulikia matukio hayo.

</Note>

### Kwa nini kutobadilika ni muhimu {/*why-immutability-is-important*/}

Angalia jinsi `handleClick` inavyoita `.slice()` kuunda nakala ya safu ya `squares` badala ya kubadilisha safu iliyopo. Ili kueleza kwa nini, tunahitaji kujadili kutobadilika na umuhimu wake.

Kuna njia mbili kuu za kubadilisha data. Njia ya kwanza ni *kubadilisha* data kwa kubadilisha maadili ya data moja kwa moja. Njia ya pili ni kuchukua nafasi ya data kwa nakala mpya ambayo ina mabadiliko yaliyotakiwa. Hivi ndivyo ingekuwa kama ungebadilisha safu ya `squares`:

```jsx
const squares = [null, null, null, null, null, null, null, null, null];
squares[0] = 'X';
// Sasa `squares` ni ["X", null, null, null, null, null, null, null, null];
```

Na hivi ndivyo ingekuwa kama ungebadilisha data bila kubadilisha safu ya `squares`:

```jsx
const squares = [null, null, null, null, null, null, null, null, null];
const nextSquares = ['X', null, null, null, null, null, null, null, null];
// Sasa `squares` haijabadilika, lakini kipengele cha kwanza cha `nextSquares` ni 'X' badala ya `null`
```

Matokeo ni sawa lakini kwa kutobadilisha (kutobadilisha data ya msingi) moja kwa moja, unapata faida kadhaa.

Kutobadilika kunafanya vipengele changamani kuwa rahisi kutekeleza. Baadaye katika mafunzo haya, utatekeleza kipengele cha "kusafiri muda" kinachokuruhusu kukagua historia ya mchezo na "kuruka nyuma" kwenye hatua za nyuma. Kipengele hiki si cha michezo tu--uwezo wa kufanya "undo" na "redo" ni hitaji la kawaida kwa programu. Kuepuka kubadilisha data moja kwa moja hukuruhusu kuweka matoleo ya awali ya data bila kuguswa, na kuyatumia tena baadaye.

Pia kuna faida nyingine ya kutobadilika. Kwa chaguo-msingi, vipengele vyote vya mtoto hujirejesha moja kwa moja hali ya sehemu ya mzazi inaposasishwa. Hii ni pamoja na vipengele vya mtoto ambavyo havikuathiriwa na mabadiliko. Ingawa kurejesha si jambo linaloonekana moja kwa moja kwa mtumiaji (hutaki kujaribu kuliepuka kwa bidii!), unaweza kutaka kuzuia kurejesha sehemu ya mti ambayo wazi haikuathiriwa kwa sababu za utendaji. Kutobadilika hufanya iwe rahisi kwa vipengele kulinganisha kama data yao imebadilika au la. Unaweza kujifunza zaidi kuhusu jinsi React inavyochagua wakati wa kurejesha kipengele katika [marejeleo ya API ya `memo`](/reference/react/memo).

### Kuchukua zamu {/*taking-turns*/}

Sasa ni wakati wa kurekebisha hitilafu kubwa katika mchezo huu wa tic-tac-toe: alama za "O" haziwezi kuwekewa alama kwenye ubao.

Utaweka hatua ya kwanza kuwa "X" kwa chaguo-msingi. Wacha tuweke rekodi ya hii kwa kuongeza kipengele kingine cha hali kwenye sehemu ya `Board`:

```js {2}
function Board() {
  const [xIsNext, setXIsNext] = useState(true);
  const [squares, setSquares] = useState(Array(9).fill(null));

  // ...
}
```

Kila wakati mchezaji anapohamia, `xIsNext` (boolean) itabadilishwa ili kuamua ni mchezaji gani atakayefuata na hali ya mchezo itaokolewa. Utaboresha kazi ya `handleClick` ya `Board` ili kubadilisha thamani ya `xIsNext`:

```js {7,8,9,10,11,13}
export default function Board() {
  const [xIsNext, setXIsNext] = useState(true);
  const [squares, setSquares] = useState(Array(9).fill(null));

  function handleClick(i) {
    const nextSquares = squares.slice();
    if (xIsNext) {
      nextSquares[i] = "X";
    } else {
      nextSquares[i] = "O";
    }
    setSquares(nextSquares);
    setXIsNext(!xIsNext);
  }

  return (
    //...
  );
}
```

Sasa, unapobonyeza squares tofauti, zitabadilisha kati ya `X` na `O`, kama inavyopaswa!

Lakini subiri, kuna tatizo. Jaribu kubonyeza square moja mara kadhaa:

![O ikibadilisha X](../images/tutorial/o-replaces-x.gif)

`X` inabadilishwa na `O`! Ingawa hii ingeongeza mabadiliko ya kufurahisha kwenye mchezo, kwa sasa tutafuata sheria za awali.

Unapoweka square na `X` au `O` huchunguzi kwanza ikiwa square tayari ina thamani ya `X` au `O`. Unaweza kurekebisha hili kwa *kurejesha mapema*. Utachunguza ikiwa square tayari ina `X` au `O`. Ikiwa square tayari imejazwa, utarejesha ndani ya kazi ya `handleClick` mapema--kabla haijajaribu kusasisha hali ya ubao.

```js {2,3,4}
function handleClick(i) {
  if (squares[i]) {
    return;
  }
  const nextSquares = squares.slice();
  //...
}
```

Sasa unaweza tu kuongeza `X` au `O` kwenye squares tupu! Hivi ndivyo msimbo wako unavyopaswa kuonekana kwa sasa:

<Sandpack>

```js src/App.js
import { useState } from 'react';

function Square({value, onSquareClick}) {
  return (
    <button className="square" onClick={onSquareClick}>
      {value}
    </button>
  );
}

export default function Board() {
  const [xIsNext, setXIsNext] = useState(true);
  const [squares, setSquares] = useState(Array(9).fill(null));

  function handleClick(i) {
    if (squares[i]) {
      return;
    }
    const nextSquares = squares.slice();
    if (xIsNext) {
      nextSquares[i] = 'X';
    } else {
      nextSquares[i] = 'O';
    }
    setSquares(nextSquares);
    setXIsNext(!xIsNext);
  }

  return (
    <>
      <div className="board-row">
        <Square value={squares[0]} onSquareClick={() => handleClick(0)} />
        <Square value={squares[1]} onSquareClick={() => handleClick(1)} />
        <Square value={squares[2]} onSquareClick={() => handleClick(2)} />
      </div>
      <div className="board-row">
        <Square value={squares[3]} onSquareClick={() => handleClick(3)} />
        <Square value={squares[4]} onSquareClick={() => handleClick(4)} />
        <Square value={squares[5]} onSquareClick={() => handleClick(5)} />
      </div>
      <div className="board-row">
        <Square value={squares[6]} onSquareClick={() => handleClick(6)} />
        <Square value={squares[7]} onSquareClick={() => handleClick(7)} />
        <Square value={squares[8]} onSquareClick={() => handleClick(8)} />
      </div>
    </>
  );
}
```

```css src/styles.css
* {
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
  margin: 20px;
  padding: 0;
}

.square {
  background: #fff;
  border: 1px solid #999;
  float: left;
  font-size: 24px;
  font-weight: bold;
  line-height: 34px;
  height: 34px;
  margin-right: -1px;
  margin-top: -1px;
  padding: 0;
  text-align: center;
  width: 34px;
}

.board-row:after {
  clear: both;
  content: '';
  display: table;
}

.status {
  margin-bottom: 10px;
}
.game {
  display: flex;
  flex-direction: row;
}

.game-info {
  margin-left: 20px;
}
```

</Sandpack>

### Kutangaza mshindi {/*declaring-a-winner*/}

Sasa kwa kuwa wachezaji wanaweza kubadilishana zamu, utataka kuonyesha wakati mchezo umeshinda na hakuna zamu zaidi za kufanya. Ili kufanya hivyo, utaongeza kazi msaidizi inayoitwa `calculateWinner` ambayo inachukua safu ya squares 9, inachunguza mshindi na inarudisha `'X'`, `'O'`, au `null` kulingana na inavyostahili. Usijali sana kuhusu kazi ya `calculateWinner`; sio maalum kwa React:

```js src/App.js
export default function Board() {
  //...
}

function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6]
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}
```

<Note>

Haijalishi kama unafafanua `calculateWinner` kabla au baada ya `Board`. Wacha tuiweke mwisho ili usilazimike kusogeza chini kila mara unapohariri sehemu zako.

</Note>

Utaita `calculateWinner(squares)` ndani ya kazi ya `handleClick` ya sehemu ya `Board` ili kuchunguza ikiwa mchezaji ameshinda. Unaweza kufanya ukaguzi huu kwa wakati mmoja unapotathmini ikiwa mtumiaji amebofya square ambayo tayari ina `X` au `O`. Tunapenda kurudi mapema katika matukio yote mawili:

```js {2}
function handleClick(i) {
  if (squares[i] || calculateWinner(squares)) {
    return;
  }
  const nextSquares = squares.slice();
  //...
}
```

Ili kuwajulisha wachezaji wakati mchezo umekwisha, unaweza kuonyesha maandishi kama "Winner: X" au "Winner: O". Ili kufanya hivyo, utaongeza sehemu ya `status` kwa sehemu ya `Board`. Hali itaonyesha mshindi ikiwa mchezo umekwisha, na ikiwa mchezo unaendelea utaonyesha ni mchezaji gani atakayefuata:

```js {3-9,13}
export default function Board() {
  // ...
  const winner = calculateWinner(squares);
  let status;
  if (winner) {
    status = "Winner: " + winner;
  } else {
    status = "Next player: " + (xIsNext ? "X" : "O");
  }

  return (
    <>
      <div className="status">{status}</div>
      <div className="board-row">
        // ...
  )
}
```

Hongera! Sasa una mchezo wa tic-tac-toe unaofanya kazi. Na pia umejifunza misingi ya React. Kwa hivyo _wewe_ ndiye mshindi halisi hapa. Hivi ndivyo msimbo unavyopaswa kuonekana:

<Sandpack>

```js src/App.js
import { useState } from 'react';

function Square({value, onSquareClick}) {
  return (
    <button className="square" onClick={onSquareClick}>
      {value}
    </button>
  );
}

export default function Board() {
  const [xIsNext, setXIsNext] = useState(true);
  const [squares, setSquares] = useState(Array(9).fill(null));

  function handleClick(i) {
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    const nextSquares = squares.slice();
    if (xIsNext) {
      nextSquares[i] = 'X';
    } else {
      nextSquares[i] = 'O';
    }
    setSquares(nextSquares);
    setXIsNext(!xIsNext);
  }

  const winner = calculateWinner(squares);
  let status;
  if (winner) {
    status = 'Winner: ' + winner;
  } else {
    status = 'Next player: ' + (xIsNext ? 'X' : 'O');
  }

  return (
    <>
      <div className="status">{status}</div>
      <div className="board-row">
        <Square value={squares[0]} onSquareClick={() => handleClick(0)} />
        <Square value={squares[1]} onSquareClick={() => handleClick(1)} />
        <Square value={squares[2]} onSquareClick={() => handleClick(2)} />
      </div>
      <div className="board-row">
        <Square value={squares[3]} onSquareClick={() => handleClick(3)} />
        <Square value={squares[4]} onSquareClick={() => handleClick(4)} />
        <Square value={squares[5]} onSquareClick={() => handleClick(5)} />
      </div>
      <div className="board-row">
        <Square value={squares[6]} onSquareClick={() => handleClick(6)} />
        <Square value={squares[7]} onSquareClick={() => handleClick(7)} />
        <Square value={squares[8]} onSquareClick={() => handleClick(8)} />
      </div>
    </>
  );
}

function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}
```

```css src/styles.css
* {
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
  margin: 20px;
  padding: 0;
}

.square {
  background: #fff;
  border: 1px solid #999;
  float: left;
  font-size: 24px;
  font-weight: bold;
  line-height: 34px;
  height: 34px;
  margin-right: -1px;
  margin-top: -1px;
  padding: 0;
  text-align: center;
  width: 34px;
}

.board-row:after {
  clear: both;
  content: '';
  display: table;
}

.status {
  margin-bottom: 10px;
}
.game {
  display: flex;
  flex-direction: row;
}

.game-info {
  margin-left: 20px;
}
```

</Sandpack>

## Kuongeza usafiri wa wakati {/*adding-time-travel*/}

Kama mazoezi ya mwisho, wacha tufanye iwezekane "kurudi nyuma kwa wakati" kwa hatua zilizopita kwenye mchezo.

### Kuhifadhi historia ya hatua {/*storing-a-history-of-moves*/}

Ikiwa ulirekebisha safu ya `squares`, kutekeleza usafiri wa wakati kungekuwa ngumu sana.

Walakini, ulitumia `slice()` kuunda nakala mpya ya safu ya `squares` baada ya kila hatua, na kuichukulia kama isiyobadilika. Hii itakuruhusu kuhifadhi kila toleo la zamani la safu ya `squares`, na kuvinjari zamu ambazo tayari zimetokea.

Utahifadhi safu za zamani za `squares` kwenye safu nyingine iitwayo `history`, ambayo utahifadhi kama variable mpya ya hali. Safu ya `history` inawakilisha hali zote za ubao, kuanzia hatua ya kwanza hadi ya mwisho, na ina umbo kama hili:

```jsx
[
  // Kabla ya hatua ya kwanza
  [null, null, null, null, null, null, null, null, null],
  // Baada ya hatua ya kwanza
  [null, null, null, null, 'X', null, null, null, null],
  // Baada ya hatua ya pili
  [null, null, null, null, 'X', null, null, null, 'O'],
  // ...
]
```

### Kupandisha hali juu, tena {/*lifting-state-up-again*/}

Sasa utaandika sehemu mpya ya kiwango cha juu inayoitwa `Game` ili kuonyesha orodha ya hatua za zamani. Hapo ndipo utaweka hali ya `history` ambayo inajumuisha historia nzima ya mchezo.

Kuweka hali ya `history` kwenye sehemu ya `Game` itakuruhusu kuondoa hali ya `squares` kutoka kwa sehemu yake ya mtoto, `Board`. Kama ulivyopandisha hali kutoka kwa sehemu ya `Square` hadi kwenye `Board`, sasa utaipandisha kutoka kwa `Board` hadi kwenye sehemu kuu ya `Game`. Hii inampa `Game` udhibiti kamili wa data ya `Board` na kumruhusu kuelekeza `Board` kuchora zamu za awali kutoka kwa `history`.

Kwanza, ongeza sehemu ya `Game` na `export default`. Ifanye ichore sehemu ya `Board` na markup fulani:

```js {1,5-16}
function Board() {
  // ...
}

export default function Game() {
  return (
    <div className="game">
      <div className="game-board">
        <Board />
      </div>
      <div className="game-info">
        <ol>{/*TODO*/}</ol>
      </div>
    </div>
  );
}
```

Kumbuka kuwa unaondoa maneno ya `export default` kabla ya tamko la `function Board() {` na kuyaongeza kabla ya tamko la `function Game() {`. Hii inaeleza faili yako ya `index.js` kutumia sehemu ya `Game` kama sehemu kuu badala ya sehemu yako ya `Board`. `div` za ziada zilizorejeshwa na sehemu ya `Game` zinatoa nafasi kwa taarifa za mchezo utakazoongeza baadaye kwenye ubao.

Ongeza hali fulani kwenye sehemu ya `Game` ili kufuatilia mchezaji anayefuata na historia ya hatua:

```js {2-3}
export default function Game() {
  const [xIsNext, setXIsNext] = useState(true);
  const [history, setHistory] = useState([Array(9).fill(null)]);
  // ...
```

Kumbuka jinsi `[Array(9).fill(null)]` ilivyo safu yenye kipengele kimoja, ambacho chenyewe ni safu ya `null` tisa.

Ili kuchora squares za hatua ya sasa, utataka kusoma safu ya mwisho ya squares kutoka kwa `history`. Huhitaji `useState` kwa hili--tayari una taarifa ya kutosha kuihesabu wakati wa kuchora:

```js {4}
export default function Game() {
  const [xIsNext, setXIsNext] = useState(true);
  const [history, setHistory] = useState([Array(9).fill(null)]);
  const currentSquares = history[history.length - 1];
  // ...
```

Endelea na utengeneze kazi ya `handlePlay` ndani ya sehemu ya `Game` ambayo itaitwa na sehemu ya `Board` ili kusasisha mchezo. Pitisha `xIsNext`, `currentSquares`, na `handlePlay` kama props kwa sehemu ya `Board`:

```js {6-8,13}
export default function Game() {
  const [xIsNext, setXIsNext] = useState(true);
  const [history, setHistory] = useState([Array(9).fill(null)]);
  const currentSquares = history[history.length - 1];

  function handlePlay(nextSquares) {
    // TODO
  }

  return (
    <div className="game">
      <div className="game-board">
        <Board xIsNext={xIsNext} squares={currentSquares} onPlay={handlePlay} />
        //...
  )
}
```

Hapa, `[...history, nextSquares]` inaunda orodha mpya inayojumuisha vitu vyote katika `history`, ikifuatiwa na `nextSquares`. (Unaweza kusoma `...history` kama "orodhesha vitu vyote vilivyomo ndani ya `history`".)

Kwa mfano, ikiwa `history` ni `[[null,null,null], ["X",null,null]]` na `nextSquares` ni `["X",null,"O"]`, basi orodha mpya `[...history, nextSquares]` itakuwa `[[null,null,null], ["X",null,null], ["X",null,"O"]]`.

Katika hatua hii, umehamisha hali (state) kuishi ndani ya kipengele cha `Game`, na UI inapaswa kuwa inafanya kazi kikamilifu, kama ilivyokuwa kabla ya mabadiliko. Hapa kuna vile nambari inavyopaswa kuonekana kwa wakati huu:

<Sandpack>

```js src/App.js
import { useState } from 'react';

function Square({ value, onSquareClick }) {
  return (
    <button className="square" onClick={onSquareClick}>
      {value}
    </button>
  );
}

function Board({ xIsNext, squares, onPlay }) {
  function handleClick(i) {
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    const nextSquares = squares.slice();
    if (xIsNext) {
      nextSquares[i] = 'X';
    } else {
      nextSquares[i] = 'O';
    }
    onPlay(nextSquares);
  }

  const winner = calculateWinner(squares);
  let status;
  if (winner) {
    status = 'Winner: ' + winner;
  } else {
    status = 'Next player: ' + (xIsNext ? 'X' : 'O');
  }

  return (
    <>
      <div className="status">{status}</div>
      <div className="board-row">
        <Square value={squares[0]} onSquareClick={() => handleClick(0)} />
        <Square value={squares[1]} onSquareClick={() => handleClick(1)} />
        <Square value={squares[2]} onSquareClick={() => handleClick(2)} />
      </div>
      <div className="board-row">
        <Square value={squares[3]} onSquareClick={() => handleClick(3)} />
        <Square value={squares[4]} onSquareClick={() => handleClick(4)} />
        <Square value={squares[5]} onSquareClick={() => handleClick(5)} />
      </div>
      <div className="board-row">
        <Square value={squares[6]} onSquareClick={() => handleClick(6)} />
        <Square value={squares[7]} onSquareClick={() => handleClick(7)} />
        <Square value={squares[8]} onSquareClick={() => handleClick(8)} />
      </div>
    </>
  );
}

export default function Game() {
  const [xIsNext, setXIsNext] = useState(true);
  const [history, setHistory] = useState([Array(9).fill(null)]);
  const currentSquares = history[history.length - 1];

  function handlePlay(nextSquares) {
    setHistory([...history, nextSquares]);
    setXIsNext(!xIsNext);
  }

  return (
    <div className="game">
      <div className="game-board">
        <Board xIsNext={xIsNext} squares={currentSquares} onPlay={handlePlay} />
      </div>
      <div className="game-info">
        <ol>{/*TODO*/}</ol>
      </div>
    </div>
  );
}

function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}
```

```css src/styles.css
* {
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
  margin: 20px;
  padding: 0;
}

.square {
  background: #fff;
  border: 1px solid #999;
  float: left;
  font-size: 24px;
  font-weight: bold;
  line-height: 34px;
  height: 34px;
  margin-right: -1px;
  margin-top: -1px;
  padding: 0;
  text-align: center;
  width: 34px;
}

.board-row:after {
  clear: both;
  content: '';
  display: table;
}

.status {
  margin-bottom: 10px;
}
.game {
  display: flex;
  flex-direction: row;
}

.game-info {
  margin-left: 20px;
}
```

</Sandpack>

### Kuonyesha harakati za zamani {/*kuonyesha-harakati-za-zamani*/}

Kwa sababu unarekodi historia ya mchezo wa tic-tac-toe, sasa unaweza kuonyesha orodha ya harakati za zamani kwa mchezaji.

Vipengele vya React kama `<button>` ni vitu vya kawaida vya JavaScript; unaweza kuvitumia katika programu yako. Ili kutengeneza vitu vingi kwenye React, unaweza kutumia orodha ya vipengele vya React.

Tayari una orodha ya harakati za `history` katika hali, hivyo sasa unahitaji kubadilisha kuwa orodha ya vipengele vya React. Katika JavaScript, ili kubadilisha orodha moja kuwa nyingine, unaweza kutumia mbinu ya [array `map`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

```jsx
[1, 2, 3].map((x) => x * 2) // [2, 4, 6]
```

Utatumia `map` kubadilisha `history` ya harakati zako kuwa vipengele vya React vinavyowakilisha vitufe kwenye skrini, na kuonyesha orodha ya vitufe ili "kuruka" hadi kwenye harakati za zamani. Hapa chini ni jinsi unavyoweza kutumia `map` kwenye historia katika kipengele cha Mchezo:

```js {11-13,15-27,35}
export default function Game() {
  const [xIsNext, setXIsNext] = useState(true);
  const [history, setHistory] = useState([Array(9).fill(null)]);
  const currentSquares = history[history.length - 1];

  function handlePlay(nextSquares) {
    setHistory([...history, nextSquares]);
    setXIsNext(!xIsNext);
  }

  function jumpTo(nextMove) {
    // TODO
  }

  const moves = history.map((squares, move) => {
    let description;
    if (move > 0) {
      description = 'Endelea hadi harakati #' + move;
    } else {
      description = 'Rudi mwanzo wa mchezo';
    }
    return (
      <li>
        <button onClick={() => jumpTo(move)}>{description}</button>
      </li>
    );
  });

  return (
    <div className="game">
      <div className="game-board">
        <Board xIsNext={xIsNext} squares={currentSquares} onPlay={handlePlay} />
      </div>
      <div className="game-info">
        <ol>{moves}</ol>
      </div>
    </div>
  );
}
```
Unaweza kuona jinsi kanuni yako inavyopaswa kuonekana hapa chini. Kumbuka kwamba unapaswa kuona hitilafu katika koni ya zana za mende inayosema:

<ConsoleBlock level="warning">
Warning: Each child in an array or iterator should have a unique "key" prop. Check the render method of &#96;Game&#96;.
</ConsoleBlock>
  
Utarekebisha hitilafu hii katika sehemu inayofuata.

<Sandpack>

```js src/App.js
import { useState } from 'react';

function Square({ value, onSquareClick }) {
  return (
    <button className="square" onClick={onSquareClick}>
      {value}
    </button>
  );
}

function Board({ xIsNext, squares, onPlay }) {
  function handleClick(i) {
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    const nextSquares = squares.slice();
    if (xIsNext) {
      nextSquares[i] = 'X';
    } else {
      nextSquares[i] = 'O';
    }
    onPlay(nextSquares);
  }

  const winner = calculateWinner(squares);
  let status;
  if (winner) {
    status = 'Winner: ' + winner;
  } else {
    status = 'Next player: ' + (xIsNext ? 'X' : 'O');
  }

  return (
    <>
      <div className="status">{status}</div>
      <div className="board-row">
        <Square value={squares[0]} onSquareClick={() => handleClick(0)} />
        <Square value={squares[1]} onSquareClick={() => handleClick(1)} />
        <Square value={squares[2]} onSquareClick={() => handleClick(2)} />
      </div>
      <div className="board-row">
        <Square value={squares[3]} onSquareClick={() => handleClick(3)} />
        <Square value={squares[4]} onSquareClick={() => handleClick(4)} />
        <Square value={squares[5]} onSquareClick={() => handleClick(5)} />
      </div>
      <div className="board-row">
        <Square value={squares[6]} onSquareClick={() => handleClick(6)} />
        <Square value={squares[7]} onSquareClick={() => handleClick(7)} />
        <Square value={squares[8]} onSquareClick={() => handleClick(8)} />
      </div>
    </>
  );
}

export default function Game() {
  const [xIsNext, setXIsNext] = useState(true);
  const [history, setHistory] = useState([Array(9).fill(null)]);
  const currentSquares = history[history.length - 1];

  function handlePlay(nextSquares) {
    setHistory([...history, nextSquares]);
    setXIsNext(!xIsNext);
  }

  function jumpTo(nextMove) {
    // TODO
  }

  const moves = history.map((squares, move) => {
    let description;
    if (move > 0) {
      description = 'Go to move #' + move;
    } else {
      description = 'Go to game start';
    }
    return (
      <li>
        <button onClick={() => jumpTo(move)}>{description}</button>
      </li>
    );
  });

  return (
    <div className="game">
      <div className="game-board">
        <Board xIsNext={xIsNext} squares={currentSquares} onPlay={handlePlay} />
      </div>
      <div className="game-info">
        <ol>{moves}</ol>
      </div>
    </div>
  );
}

function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}
```

```css src/styles.css
* {
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
  margin: 20px;
  padding: 0;
}

.square {
  background: #fff;
  border: 1px solid #999;
  float: left;
  font-size: 24px;
  font-weight: bold;
  line-height: 34px;
  height: 34px;
  margin-right: -1px;
  margin-top: -1px;
  padding: 0;
  text-align: center;
  width: 34px;
}

.board-row:after {
  clear: both;
  content: '';
  display: table;
}

.status {
  margin-bottom: 10px;
}

.game {
  display: flex;
  flex-direction: row;
}

.game-info {
  margin-left: 20px;
}
```

</Sandpack>

Unapozunguka kupitia orodha ya `history` ndani ya kazi unayopitisha kwa `map`, hoja ya `squares` inapitia kila kipengele cha `history`, na hoja ya `move` inapitia kila kiashiria cha orodha: `0`, `1`, `2`, …. (Katika hali nyingi, ungehitaji vipengele halisi vya orodha, lakini ili kuonyesha orodha ya harakati utahitaji tu viashiria.)

Kwa kila harakati katika historia ya mchezo wa tic-tac-toe, unaunda kipengele cha orodha `<li>` kinachojumuisha kitufe `<button>`. Kitufe kinakuwa na kihariri cha `onClick` kinachoitwa kazi inayoitwa `jumpTo` (ambayo huja kuwa umeitekeleza bado).

Kwa sasa, unapaswa kuona orodha ya harakati zilizotokea katika mchezo na hitilafu katika koni ya zana za mende. Hebu tujadili maana ya hitilafu ya "key."

### Kuchagua kipeperushi `{/*picking-a-key*/}` {/*kuchagua-kipeperushi-picking-a-key*/}

Unapohonyesha orodha, React huhifadhi baadhi ya habari kuhusu kila kipengele cha orodha kilichohonyeshwa. Unaposasisha orodha, React inahitaji kubaini kile kilichobadilika. Huenda umeongeza, kuondoa, kubadilisha, au kupanga upya vipengele vya orodha.

Fikiria unapohama kutoka

```html
<li>Alexa: 7 tasks left</li>
<li>Ben: 5 tasks left</li>
```

hadi

```html
<li>Ben: 9 tasks left</li>
<li>Claudia: 8 tasks left</li>
<li>Alexa: 5 tasks left</li>
```

Mbali na idadi za sasa, mtu akiyasoma hili angeweza kusema kuwa umehamisha mpangilio wa Alexa na Ben na kuweka Claudia kati ya Alexa na Ben. Hata hivyo, React ni programu ya kompyuta na haiwezi kujua unachokusudia, hivyo unahitaji kutoa sifa ya _key_ kwa kila kipengele cha orodha ili kutofautisha kila kipengele cha orodha kutoka kwa wenzao. Ikiwa data yako ingeletwa kutoka kwa hifadhidata, IDs za Alexa, Ben, na Claudia za hifadhidata zinaweza kutumika kama funguo.

```js {1}
<li key={user.id}>
  {user.name}: {user.taskCount} tasks left
</li>
```

Wakati orodha inapojengwa tena, React hutumia kila funguo ya kipengele cha orodha na kutafuta vipengele vya orodha vilivyopita kwa funguo zinazofanana. Ikiwa orodha ya sasa ina funguo ambazo hazikuwapo hapo awali, React inaunda kipengele. Ikiwa orodha ya sasa inakosa funguo ambazo zilikuwepo kwenye orodha iliyopita, React inaharibu kipengele kilichopita. Ikiwa funguo mbili zinalingana, kipengele kinahama.

Funguo zinamwambia React kuhusu utambulisho wa kila kipengele, jambo linalomwezesha React kudumisha hali kati ya uchoraji wa orodha. Ikiwa funguo ya kipengele inabadilika, kipengele kitaharibiwa na kuundwa upya na hali mpya.

`key` ni sifa maalum na iliyohifadhiwa katika React. Wakati kipengele kinaundwa, React hutolewa sifa ya `key` na kuihifadhi moja kwa moja kwenye kipengele kilichorejeshwa. Ingawa `key` inaweza kuonekana kama inapitishwa kama props, React hutumia kiotomatiki `key` kuamua ni vipengele vipi vya kusasishwa. Hakuna njia kwa kipengele kuuliza ni sifa gani ya `key` mzazi wake aliyoainisha.

**Inapendekezwa sana kwamba utoe funguo sahihi kila unapojenga orodha za mabadiliko.** Ikiwa huna kipeperushi sahihi, unaweza kutaka kufikiria tena muundo wa data yako ili kupata moja.

Ikiwa hakuna kipeperushi kilichotolewa, React itaripoti hitilafu na kutumia kiashiria cha orodha kama kipeperushi kwa kutumika kwa default. Kutumia kiashiria cha orodha kama kipeperushi ni tatizo unapojaribu kupanga upya vipengele vya orodha au kuingiza/kuondoa vipengele vya orodha. Kupitia kipeperushi cha kipeperushi `key={i}` kunazuia hitilafu lakini kuna matatizo yale yale kama kiashiria cha orodha na halipendekezwi katika hali nyingi.

Funguo hazihitaji kuwa za kipekee duniani; zinahitaji tu kuwa za kipekee kati ya vipengele na ndugu zao.

### Kutekeleza safari ya wakati `{/*implementing-time-travel*/}` {/*kutekeleza-safari-ya-wakati-implementing-time-travel*/}

Katika historia ya mchezo wa tic-tac-toe, kila harakati ya zamani ina ID ya kipekee inayohusishwa nayo: ni nambari mfuatano ya harakati. Harakati hazitabadilishwa, kuondolewa, au kuingizwa katikati, hivyo ni salama kutumia kiashiria cha harakati kama kipeperushi.

Katika kazi ya `Game`, unaweza kuongeza kipeperushi kama `<li key={move}>`, na ikiwa utaongeza upya mchezo ulioonyeshwa, hitilafu ya "key" ya React inapaswa kuondoka.

```js {4}
const moves = history.map((squares, move) => {
  //...
  return (
    <li key={move}>
      <button onClick={() => jumpTo(move)}>{description}</button>
    </li>
  );
});
```

<Sandpack>

```js src/App.js
import { useState } from 'react';

function Square({ value, onSquareClick }) {
  return (
    <button className="square" onClick={onSquareClick}>
      {value}
    </button>
  );
}

function Board({ xIsNext, squares, onPlay }) {
  function handleClick(i) {
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    const nextSquares = squares.slice();
    if (xIsNext) {
      nextSquares[i] = 'X';
    } else {
      nextSquares[i] = 'O';
    }
    onPlay(nextSquares);
  }

  const winner = calculateWinner(squares);
  let status;
  if (winner) {
    status = 'Winner: ' + winner;
  } else {
    status = 'Next player: ' + (xIsNext ? 'X' : 'O');
  }

  return (
    <>
      <div className="status">{status}</div>
      <div className="board-row">
        <Square value={squares[0]} onSquareClick={() => handleClick(0)} />
        <Square value={squares[1]} onSquareClick={() => handleClick(1)} />
        <Square value={squares[2]} onSquareClick={() => handleClick(2)} />
      </div>
      <div className="board-row">
        <Square value={squares[3]} onSquareClick={() => handleClick(3)} />
        <Square value={squares[4]} onSquareClick={() => handleClick(4)} />
        <Square value={squares[5]} onSquareClick={() => handleClick(5)} />
      </div>
      <div className="board-row">
        <Square value={squares[6]} onSquareClick={() => handleClick(6)} />
        <Square value={squares[7]} onSquareClick={() => handleClick(7)} />
        <Square value={squares[8]} onSquareClick={() => handleClick(8)} />
      </div>
    </>
  );
}

export default function Game() {
  const [xIsNext, setXIsNext] = useState(true);
  const [history, setHistory] = useState([Array(9).fill(null)]);
  const currentSquares = history[history.length - 1];

  function handlePlay(nextSquares) {
    setHistory([...history, nextSquares]);
    setXIsNext(!xIsNext);
  }

  function jumpTo(nextMove) {
    // TODO
  }

  const moves = history.map((squares, move) => {
    let description;
    if (move > 0) {
      description = 'Go to move #' + move;
    } else {
      description = 'Go to game start';
    }
    return (
      <li key={move}>
        <button onClick={() => jumpTo(move)}>{description}</button>
      </li>
    );
  });

  return (
    <div className="game">
      <div className="game-board">
        <Board xIsNext={xIsNext} squares={currentSquares} onPlay={handlePlay} />
      </div>
      <div className="game-info">
        <ol>{moves}</ol>
      </div>
    </div>
  );
}

function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}

```

```css src/styles.css
* {
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
  margin: 20px;
  padding: 0;
}

.square {
  background: #fff;
  border: 1px solid #999;
  float: left;
  font-size: 24px;
  font-weight: bold;
  line-height: 34px;
  height: 34px;
  margin-right: -1px;
  margin-top: -1px;
  padding: 0;
  text-align: center;
  width: 34px;
}

.board-row:after {
  clear: both;
  content: '';
  display: table;
}

.status {
  margin-bottom: 10px;
}

.game {
  display: flex;
  flex-direction: row;
}

.game-info {
  margin-left: 20px;
}
```

</Sandpack>

Kabla hujatekeleza `jumpTo`, unahitaji kipengele cha `Game` kufuatilia ni hatua gani mchezaji anayoangalia kwa sasa. Ili kufanya hivyo, tengeneza kipengele kipya cha hali kinachoitwa `currentMove`, kilichoelea kuwa `0`:

```js {4}
export default function Game() {
  const [xIsNext, setXIsNext] = useState(true);
  const [history, setHistory] = useState([Array(9).fill(null)]);
  const [currentMove, setCurrentMove] = useState(0);
  const currentSquares = history[history.length - 1];
  //...
}
```

Kisha, sasisha kazi ya `jumpTo` ndani ya `Game` ili kusasisha `currentMove`. Pia utaweka `xIsNext` kuwa `true` ikiwa nambari unayobadilisha `currentMove` kuwa ni nambari yenye uwiano wa mbili.

```js {4-5}
export default function Game() {
  // ...
  function jumpTo(nextMove) {
    setCurrentMove(nextMove);
    setXIsNext(nextMove % 2 === 0);
  }
  //...
}
```

Sasa, utaleta mabadiliko mawili kwa kazi ya `handlePlay` ya `Game` inayoitwa unapobofya kwenye mraba.

- Ikiwa "unarudi nyuma katika wakati" kisha kufanya harakati mpya kutoka kwa hatua hiyo, unataka kuhifadhi historia hadi hatua hiyo pekee. Badala ya kuongeza `nextSquares` baada ya vitu vyote (`...` sintaksia ya kueneza) katika `history`, utaongeza baada ya vitu vyote katika `history.slice(0, currentMove + 1)` ili kuhakikisha unahifadhi sehemu hiyo tu ya historia ya zamani.
- Kila wakati harakati inapotokea, unahitaji kusasisha `currentMove` kuelekeza kwenye kipengele cha hivi karibuni cha historia.

```js {2-4}
function handlePlay(nextSquares) {
  const nextHistory = [...history.slice(0, currentMove + 1), nextSquares];
  setHistory(nextHistory);
  setCurrentMove(nextHistory.length - 1);
  setXIsNext(!xIsNext);
}
```

Mwishowe, utabadilisha kipengele cha `Game` ili kuonyesha harakati iliyochaguliwa kwa sasa, badala ya kuonyesha kila mara harakati ya mwisho:

```js {5}
export default function Game() {
  const [xIsNext, setXIsNext] = useState(true);
  const [history, setHistory] = useState([Array(9).fill(null)]);
  const [currentMove, setCurrentMove] = useState(0);
  const currentSquares = history[currentMove];

  // ...
}
```

Ikiwa uta-bofya kwenye hatua yoyote katika historia ya mchezo, bodi ya tic-tac-toe inapaswa sasishwa mara moja kuonyesha kile kilichokuwa kwenye bodi baada ya hatua hiyo kutokea.

<Sandpack>

```js src/App.js
import { useState } from 'react';

function Square({value, onSquareClick}) {
  return (
    <button className="square" onClick={onSquareClick}>
      {value}
    </button>
  );
}

function Board({ xIsNext, squares, onPlay }) {
  function handleClick(i) {
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    const nextSquares = squares.slice();
    if (xIsNext) {
      nextSquares[i] = 'X';
    } else {
      nextSquares[i] = 'O';
    }
    onPlay(nextSquares);
  }

  const winner = calculateWinner(squares);
  let status;
  if (winner) {
    status = 'Winner: ' + winner;
  } else {
    status = 'Next player: ' + (xIsNext ? 'X' : 'O');
  }

  return (
    <>
      <div className="status">{status}</div>
      <div className="board-row">
        <Square value={squares[0]} onSquareClick={() => handleClick(0)} />
        <Square value={squares[1]} onSquareClick={() => handleClick(1)} />
        <Square value={squares[2]} onSquareClick={() => handleClick(2)} />
      </div>
      <div className="board-row">
        <Square value={squares[3]} onSquareClick={() => handleClick(3)} />
        <Square value={squares[4]} onSquareClick={() => handleClick(4)} />
        <Square value={squares[5]} onSquareClick={() => handleClick(5)} />
      </div>
      <div className="board-row">
        <Square value={squares[6]} onSquareClick={() => handleClick(6)} />
        <Square value={squares[7]} onSquareClick={() => handleClick(7)} />
        <Square value={squares[8]} onSquareClick={() => handleClick(8)} />
      </div>
    </>
  );
}

export default function Game() {
  const [xIsNext, setXIsNext] = useState(true);
  const [history, setHistory] = useState([Array(9).fill(null)]);
  const [currentMove, setCurrentMove] = useState(0);
  const currentSquares = history[currentMove];

  function handlePlay(nextSquares) {
    const nextHistory = [...history.slice(0, currentMove + 1), nextSquares];
    setHistory(nextHistory);
    setCurrentMove(nextHistory.length - 1);
    setXIsNext(!xIsNext);
  }

  function jumpTo(nextMove) {
    setCurrentMove(nextMove);
    setXIsNext(nextMove % 2 === 0);
  }

  const moves = history.map((squares, move) => {
    let description;
    if (move > 0) {
      description = 'Go to move #' + move;
    } else {
      description = 'Go to game start';
    }
    return (
      <li key={move}>
        <button onClick={() => jumpTo(move)}>{description}</button>
      </li>
    );
  });

  return (
    <div className="game">
      <div className="game-board">
        <Board xIsNext={xIsNext} squares={currentSquares} onPlay={handlePlay} />
      </div>
      <div className="game-info">
        <ol>{moves}</ol>
      </div>
    </div>
  );
}

function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}
```

```css src/styles.css
* {
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
  margin: 20px;
  padding: 0;
}

.square {
  background: #fff;
  border: 1px solid #999;
  float: left;
  font-size: 24px;
  font-weight: bold;
  line-height: 34px;
  height: 34px;
  margin-right: -1px;
  margin-top: -1px;
  padding: 0;
  text-align: center;
  width: 34px;
}

.board-row:after {
  clear: both;
  content: '';
  display: table;
}

.status {
  margin-bottom: 10px;
}
.game {
  display: flex;
  flex-direction: row;
}

.game-info {
  margin-left: 20px;
}
```

</Sandpack>

### Kusafisha Mwisho `{/*final-cleanup*/}` {/*kusafisha-mwisho-final-cleanup*/}

Ukichunguza kwa makini kanuni, utaona kuwa `xIsNext === true` wakati `currentMove` ni nambari yenye uwiano wa mbili na `xIsNext === false` wakati `currentMove` ni nambari isiyo na uwiano wa mbili. Kwa maneno mengine, ikiwa unajua thamani ya `currentMove`, basi unaweza daima kujua kile kinachopaswa kuwa `xIsNext`.

Hakuna sababu ya kuhifadhi hizi mbili katika hali. Kwa kweli, daima jaribu kuepuka hali zinazojirudia. Kupunguza kile unachohifadhi katika hali kunapunguza hitilafu na kufanya kanuni yako kuwa rahisi kueleweka. Badilisha `Game` ili isihifadhi `xIsNext` kama kipengele cha hali kilichotenganishwa na badala yake ihesabu kwa kutegemea `currentMove`:

```js {4,11,15}
export default function Game() {
  const [history, setHistory] = useState([Array(9).fill(null)]);
  const [currentMove, setCurrentMove] = useState(0);
  const xIsNext = currentMove % 2 === 0;
  const currentSquares = history[currentMove];

  function handlePlay(nextSquares) {
    const nextHistory = [...history.slice(0, currentMove + 1), nextSquares];
    setHistory(nextHistory);
    setCurrentMove(nextHistory.length - 1);
  }

  function jumpTo(nextMove) {
    setCurrentMove(nextMove);
  }
  // ...
}
```

Sasa, huna tena haja ya tangazo la hali ya `xIsNext` au miito ya `setXIsNext`. Sasa, hakuna nafasi kwa `xIsNext` kutoka nje ya mfululizo na `currentMove`, hata kama utatengeneza makosa wakati wa kuandika vipengele.

### Kumalizia `{/*wrapping-up*/}` {/*kumalizia-wrapping-up*/}

Hongera! Umeunda mchezo wa tic-tac-toe ambao:

- Unakuruhusu kucheza tic-tac-toe,
- Unaponyesha wakati mchezaji ameshinda mchezo,
- Unahifadhi historia ya mchezo kadri unavyosonga mbele,
- Unaruhusu wachezaji kupitia historia ya mchezo na kuona matoleo ya zamani ya bodi ya mchezo.

Kazi nzuri! Tunatumai sasa unahisi kama unaelewa vizuri jinsi React inavyofanya kazi.

Angalia matokeo ya mwisho hapa:

<Sandpack>

```js src/App.js
import { useState } from 'react';

function Square({ value, onSquareClick }) {
  return (
    <button className="square" onClick={onSquareClick}>
      {value}
    </button>
  );
}

function Board({ xIsNext, squares, onPlay }) {
  function handleClick(i) {
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    const nextSquares = squares.slice();
    if (xIsNext) {
      nextSquares[i] = 'X';
    } else {
      nextSquares[i] = 'O';
    }
    onPlay(nextSquares);
  }

  const winner = calculateWinner(squares);
  let status;
  if (winner) {
    status = 'Winner: ' + winner;
  } else {
    status = 'Next player: ' + (xIsNext ? 'X' : 'O');
  }

  return (
    <>
      <div className="status">{status}</div>
      <div className="board-row">
        <Square value={squares[0]} onSquareClick={() => handleClick(0)} />
        <Square value={squares[1]} onSquareClick={() => handleClick(1)} />
        <Square value={squares[2]} onSquareClick={() => handleClick(2)} />
      </div>
      <div className="board-row">
        <Square value={squares[3]} onSquareClick={() => handleClick(3)} />
        <Square value={squares[4]} onSquareClick={() => handleClick(4)} />
        <Square value={squares[5]} onSquareClick={() => handleClick(5)} />
      </div>
      <div className="board-row">
        <Square value={squares[6]} onSquareClick={() => handleClick(6)} />
        <Square value={squares[7]} onSquareClick={() => handleClick(7)} />
        <Square value={squares[8]} onSquareClick={() => handleClick(8)} />
      </div>
    </>
  );
}

export default function Game() {
  const [history, setHistory] = useState([Array(9).fill(null)]);
  const [currentMove, setCurrentMove] = useState(0);
  const xIsNext = currentMove % 2 === 0;
  const currentSquares = history[currentMove];

  function handlePlay(nextSquares) {
    const nextHistory = [...history.slice(0, currentMove + 1), nextSquares];
    setHistory(nextHistory);
    setCurrentMove(nextHistory.length - 1);
  }

  function jumpTo(nextMove) {
    setCurrentMove(nextMove);
  }

  const moves = history.map((squares, move) => {
    let description;
    if (move > 0) {
      description = 'Go to move #' + move;
    } else {
      description = 'Go to game start';
    }
    return (
      <li key={move}>
        <button onClick={() => jumpTo(move)}>{description}</button>
      </li>
    );
  });

  return (
    <div className="game">
      <div className="game-board">
        <Board xIsNext={xIsNext} squares={currentSquares} onPlay={handlePlay} />
      </div>
      <div className="game-info">
        <ol>{moves}</ol>
      </div>
    </div>
  );
}

function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}
```

```css src/styles.css
* {
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
  margin: 20px;
  padding: 0;
}

.square {
  background: #fff;
  border: 1px solid #999;
  float: left;
  font-size: 24px;
  font-weight: bold;
  line-height: 34px;
  height: 34px;
  margin-right: -1px;
  margin-top: -1px;
  padding: 0;
  text-align: center;
  width: 34px;
}

.board-row:after {
  clear: both;
  content: '';
  display: table;
}

.status {
  margin-bottom: 10px;
}
.game {
  display: flex;
  flex-direction: row;
}

.game-info {
  margin-left: 20px;
}
```

</Sandpack>

Ikiwa una muda wa ziada au unataka kujitolea kuboresha ujuzi wako wa React, hapa kuna baadhi ya mawazo ya maboresho unayoweza kufanya kwa mchezo wa tic-tac-toe, yakiwa orodheshwa kwa mpangilio wa ugumu unaoongezeka:

1. Kwa harakati ya sasa pekee, onyesha "Upo kwenye harakati #..." badala ya kitufe.
1. Andika upya `Board` kutumia mizunguko miwili kutengeneza viwanja badala ya kuviandika kwa mikono.
1. Ongeza kitufe cha kubadili kinachokuruhusu kupanga harakati kwa mpangilio wa kupanda au kushuka.
1. Wakati mtu anashinda, angazia viwanja vitatu vilivyopunguza ushindi (na wakati hakuna anayeshinda, onyesha ujumbe kuhusu matokeo kuwa sare).
1. Onyesha eneo la kila harakati kwa muundo (mstari, nguzo) kwenye orodha ya historia ya harakati.

Katika mafunzo haya, umejifunza dhana za React ikiwemo vipengele, sehemu, props, na hali. Sasa kwamba umeona jinsi dhana hizi zinavyofanya kazi wakati wa kujenga mchezo, angalia [Thinking in React](/learn/thinking-in-react) kuona jinsi dhana hizo za React zinavyofanya kazi wakati wa kujenga UI ya programu.
