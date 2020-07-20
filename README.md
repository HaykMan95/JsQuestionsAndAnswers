# porcenq haskanal js

Author [HaykMan95](https://github.com/HaykMan95)

### JS event loop

skzbum mi qani harc


`JavaScript inch e da?`

ev patasxan

`da miapatok, none-blocking (aranc xochndotneri), sinxron, zugaher lezu e`

`hmm...`

`na uni call stack ev heap, event loop, callback queue, ev ayl api-ner`


js-i dvijoky(or `v8`) irenic nerkayacnum e hetevyal tesqy

![alt text](http://dl3.joxi.net/drive/2019/02/21/0035/3004/2296764/64/7f336be90f.jpg)

arachin masy da `heap`-e `dinamik bashxvac hishoxutyun`

erkrordy `stack` kancheri bazmutyun ,ayntex kareli e tesnel
te vor function um e kanchel.

ete duq clon aneq `v8`-i repositorian ev ayntex man gaq `setTimeout` kam
`DOM(document)` kam `XMLHttpRequest` apa vochinch cheq gtni, ban ayne vor ayt ameny
chka `v8`-um. ev da shat zarmanali e qani vor `setTimeout`-y amena arachin
banne vor du ogtagorcum es vorpesi haskanas ansinxornnutyuny.


![alt text](http://dl4.joxi.net/drive/2019/02/21/0035/3004/2296764/64/607c5e0253.jpg)


uremn menq unenq `v8` ev dranic baci `webAPIs` -vory rashirenia brausera-a
ir mech parunakuma orinak `DOM(document)`, `ajax(XMLHttpRequest)`, `setTimeout`, `Geolocation`
ev ayln, baci ays amenic menq unenq `event loop` (цикл событий)


uremn JavaScript-y miapatok katarman michavayr e `runtime` da nshanakum e
vor `stack`-y nra hamar mek hat e, ev na karox e anel mi gorcoxutyun mi pahi da
nshanakum e `miapatok`


ditarkenq hetevyal cody

```
function multipay(a, b) {
  return a * b;
}

function square(n) {
  return multipay(n,n);
}

function printSquare(n) {
  var squared = square(n);
  console.log(squared);
}

printSquare(4);

```

unenq mi qani functioner
`multipay` -bazmapatkman
`square` - qarakusi barcracnelu
`printSquare` - ev function vory tpum e tvi qarakusin

ev ete kanchenq `printSquare` -y apa ekeq tesnenq te inch tesq kunena mer `stack`-y

qani vor mer amboxj cragiry mi mec functioan e apa skzbum `stack`-i mech kngni mer
functian `main`-y
hajordiv `printSquare` ,
aynuhetev `square`
heto `multipay`


![alt text](http://dl4.joxi.net/drive/2019/02/21/0035/3004/2296764/64/31a6e298d3.jpg)


heto verchacrecinq hashvarky `multipay`- da durs kga `stack`-ic
heto `square`-y
heto `printSquare`-y ev nra mech kstananq resultaty ev kykanchenq `console.log`
u da kngni `stack`-i mech


![alt text](http://dl3.joxi.net/drive/2019/02/22/0035/3004/2296764/64/17a2bf4187.jpg)

ktpi consolum, ev kjnjvi `stack`-ic
`printSquare`-y chuni `return` , na kveradarcni `undefined` ev kjnjvi `stack`-ic,

sa motavor visualisacian e `call stack`-i


ditarkenq mi ayspisi orinak

```
function foo() {
  throw new Error('Oops!');
}

functian bar() {
  foo();
}

functian baz() {
  bar();
}

baz();
```

ashxatacnelov ays kody ktesnenq hetevyaly

![alt text](http://dl4.joxi.net/drive/2019/02/22/0035/3004/2296764/64/5449e4ce8e.jpg)

ev aystex duq karox eq tesnel ayt nuyn `call stack`-y nra vijaky Error-i pahin



duq erevi te lsel eq `stack overflow`-i masin (haeren)

naneq hetevyal orinaky

```

function foo() {
  return foo();
}

foo();

```

ashxatacnelov mer `main()`-y tesnenq `stack`-y


![alt text](http://dl4.joxi.net/drive/2019/02/22/0035/3004/2296764/64/69612baa9a.jpg)

mer `foo()`-functian ayngan kngni `stack`-i mech, minchev ancni max .sahmany,
ev consolum kgri `Maximum call stack size exceeded`, voric heto `Chrome`-y uxaki
maqrum e `stack`-y vorpesi uxaki chtraqi,



## Blocking

myus harcy ayn e te inchu e cody karox ashxatel dandax vori masin xoseluc
petq e xosenq `blocking`-ic

entandrenq mer js ashxatuma sinxron
apa inch kliner ayt jamanak
ditarkenq hetevyal kody

```

var foo = $.getSync('//foo.com');
var boo = $.getSync('//boo.com');
var woo = $.getSync('//woo.com');

console.log(foo);
console.log(boo);
console.log(woo);

```

inch kliner ete liner sinxonr, apa mer `stack`-um kgrever `$.getSync('//foo.com');` es functiony
ev menq petq e stipvac spaenq vorpesi ayn avartvi ev mer `stack`-ic jnjvi ev nor
anci hajordin `$.getSync('//boo.com');` ev `$.getSync('//woo.com');` qani vor
dranq serverain harcumner en, dranq karox en tevel shat erkar,
hetevabar miapatok lezunerum orinak `Ruby` -um katarvum e hetevyal dzevov


aysinqn ete `steck` -um ka mi ban vory katarvum e hima, apa mer amboxj kayqy
chi ashxati, chenq karox entex anel voch mi urish click-er
ev ayl gorcoxutyunner qani vor `stack`-y zbaxvac e. isk inchpes payqarel dranc dem?


`payqar dranc dem klini ansinxron collBack-ery`

vonc e da ashxatum, miacnum enq cody , ev ashxatum e `callback` -y nra avartic heto


ditarkenq mi orinak

```

console.log('hi');

setTimeout(function () {
  console.log('here');
}, 5000);

console.log('Js where are you?');

```


tesnenq te inchpes en pahvum `stack` -um `callback`-nery

`stack`-um skzbum grvum e `console.log('hi');` tpum e consolum ev korum
heto `setTimeout(cb, 5000)` ev miangamic korum e
heto `console.log('Js where are you?')` tpum e ev korum

heto 5 varkyan anc `stack`-um haytnvum e `console.log('here')`;  inchpes????

ev aystex arach e galis `Concurrency & the Event Loop` zugaherutyun ev iradarcutyunneri sharnakakanutyun


skzbum asvec vor js karox e mi akntartum anel mi gorcoxutyun, texnikapes da aytpes e
ayo js chi karox zugaher anel mi qani gorcoxutyun , bayc bany nranum e vor
`Browser`-y avelin e qan uxaki kodi ashxatelu michavayr(sreda vipalnenie)

hishenq diagramy

![alt text](http://dl3.joxi.net/drive/2019/02/22/0035/3004/2296764/64/95c037eb2a.jpg)

js dvijoky karox e anel mi gorcoxutyun mi akntartum
bayc `browser`-y uni havelyal hnaravorutyunner `webAPIs`-ner vory irencic nerkayacnum en
`threads` hosqer, nrancov karox e uxarkel harcum anelu inch vor ban, ev zugaher
js-i dvijoki het na ani ir ashxatanqy
(ete duq nodi masnaget eq apa diagramy klini nuyny , uxaki `webAPIs`-i poxaren klini `api c++`)



hima ditarkenq amboxjovin tarberaky `browser`-um

![alt text](http://dl3.joxi.net/drive/2019/02/22/0035/3004/2296764/64/c5e2fcaaaa.jpg)

`webAPIs`-n tesnum e setTimeout ev browsery dzer hamar sksum e hethashvark, qani vor
setTimeout - browser-i methodne

ev ayt timery ashxatum e zugaher, isk menq karox enq maqrel stack-y  vorpesi
ashxati myus `console.log('Js where are you?')` -y


qani vor webAPIs chi karox inqniran avelacnel stack-um inch vor mi Event
dra hamar gorci en galis `event loop`-y ev `task queue`-n (handznararutyunneri hert)

![alt text](http://dl4.joxi.net/drive/2019/02/22/0035/3004/2296764/64/cb5da21493.jpg)

ev erb vor inch vor webAPIs-verchacnum e ir ashxatanqy, planavorc callback-y
texapoxvum e task queue

![alt text](http://dl3.joxi.net/drive/2019/02/22/0035/3004/2296764/64/2fb17ae4cf.jpg)

isk hima henc ayt `event loop`-y(mer temai anuny) na uni shat parz araqelutyun
na nayum e `stack`-in ev `task queue`, ete `stack`-y datark e, na vercnum e
arachin handznararutyuny herti mechic ev texapoxum e `stack` ev vori shnorhiv
cody kashxati


![alt text](http://dl4.joxi.net/drive/2019/02/22/0035/3004/2296764/64/62b61ca098.jpg)

tvyal depqum collBack-y ir hertin kanchum e `console.log('here');` vory ir hertin
katarvum e ev maqrvum stack-ic


hima menq patrast enq lselu te inchu en arvum mi qani tarorinak gorcoxutyunner annsinxronutyan hamar,
orinak duq erevi handipel eq nran vor xorhurd e trvum kanchel `setTimeout` զրոյական ուշացումով
ayytex harc,inchu kanchel code զրոյական ուշացումով, irakanum ka patjar da arvum e nra hamar
vor inch vor ban katarvi miangamic erb `stack`-y maqrvi;
ete tanq setTimeout(cb, 0) apa sa khaskacvi espes, cb kashxati զրոյական ուշացումով erb vor `stack`-y azat klini



inchu xorhurd chi trvum `cll stack`-y canr gorcoxutyunnerov axtotel qani vor
rendri hamar nuynpes anhrajesht e vorpeshi `stack`-y azat lini ev nor sksi
rendr anel, ev ete stack-y zbaxvac exav, voch mi render chi ashxati qani der chi maqrvi ayne

ev ayt isk patjarov xorhurd e trvum ogtagorcel callback-er qani vor nranq kspasen
`task queue`-um ev qani vor ka զրոյական ուշացում apa amen rendric heto event loop-y kgci stack-i mech
mi task , ev chi stacvi aynpes vor domy kykaxvi

ev ete nuynisk ayt callback-ery canr en, bayc mievnuyn e nranc katarman yndacqum ka
զրոյական ուշացում ev dra shnorhiv rendry chi sksi kaxvel/ dandaxel
