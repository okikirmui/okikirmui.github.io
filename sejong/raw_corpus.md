---
layout: default
title: "文節の検索"
---

## 文節の検索

ここでは，検索プログラム한마루2.0（以下「한마루」とする）を用いて，21世紀世宗計画の平文コーパス（원시 말뭉치）を検索する方法について紹介します．

[検索のための準備：コーパスの読み込み](overview_sejong#検索のための準備コーパスの読み込み)を参考に，書きことばあるいは話しことばのコーパスを読み込みます．平文コーパスの場合，自分で作成したテキストファイルを読み込むことも可能です．その場合，テキストファイルはUTF-16LEエンコーディング（BOM付き）で作成しておく必要があります．

## 検索の基本

検索を行う際，文節を基本とした検索が行われます．検索式にある単語を入力した場合，その単語**だけ**で成り立つ文節が検索されます．例えば，

```text
제일
```

と入力して検索した場合，検索されるのは

* 예를 들어 교향악단을 보게 되면 누가 **제일** 중심에 앉아 있습니까?
* 뭐니뭐니 해도 집이 **제일**!

のように，제일だけで構成される文節あるいは記号が続く文節だけです．「제일기획」のように同じ文節内に他の要素が含まれる例は検索されません．

同様に，많다「多い」という語を調べるつもりで，

```text
많
```

とだけ入力して検索すると，

* 내가 입을 열면 **많**~은 사람이 다쳐요.
* 어간 형태 '**많**-'에 대해 [맘]의 혼동이 야기될 수 있으므로
* 공교육 환경 개선 등에 참여하고 싶지 않은 사람이 **많**"다고 전제한 뒤
* 향수를 이해하는 데 **많** 은 도움이 된다.

など，많に記号が続く例，または많に空白が続く例だけが検索されます．

## 音節を表す演算子による検索

検索式には，様々な役割を持った特殊文字＝演算子（연산자）を用いることができます．以下の演算子は，音節を表すのに用いられます（ガイドp.24「4)音節検索式　表3」）．

| 演算子|  役割・意味 |連続使用 |
|:---:|:----------:|:----:|
| ?  |  1つの音節  | 可 |
| %  |  0または1つの音節  | 可 |
| \*  |  0以上の音節  | 不可 |
| \+  | 1つ以上の音節  | 不可 |

### 「?」＝1つの音節

`?`は1つの音節を指します．連続して用いることができ，`?`の数だけ音節がある例を検索することができます．例えば，

```text
??그룹
```

は「그룹の前に2音節ある文節」を指し，

* 18일과 19일 국회현장에서 수시로 만나 **현대그룹** 정주영 명예회장의 기자회견에 대한 사후대응방안 등을 협의했다.
* \*자서전 펴내는 펄 시스터즈 멤버 배인순씨 최원석 **동아그룹** 전 회장과의 비화 등 털어놔
* 세무 공무원 출신인 정태수 **한보그룹** 총회장과 함께 근무한 인연이 있는 등 친밀한 사이다.
* 기아사태 이후에 **기아그룹** 임직원의 自救努力과 또 소비자와 업계가 보여준 기아살리기는 더 이상 우리 경제가
* 한때 10대 **재벌그룹** 중 하나의 최고 사모님이었던 카페 주인은
* 무대 뒤편은 빌딩과 숲으로 꾸미는 등 거대하기로 유명한 **외국그룹** 핑크 플로이드 공연에서나 볼 수 있는 장관을 연출할 계획이다.
* 이 같은 내용의 **lg그룹** 성장전략을 논의했다.
* 공정위, **6大그룹** 부당내부거래 조사
* 대검 중수부는 31일 지앤지(g&**g)그룹** 회장 이용호(李容湖)씨에게서

などの例が検索されます．そのため，

* **대그룹** 활동 시간에는
* 세계랭킹 20위 주세혁은 9일 중국 장인에서 개막된 단식 예선 **3그룹** 리그전에서 세계 9위 칼리니코스 크레앙가(그리스)를 4-2로 꺾었다.
* 오는 7월 1일부터 p&**g그룹** 중동지역 총괄 부회장으로 떠나는 앨 라즈와니<사진> p&g코리아 사장은

など，그룹の前に1音節ある文節や，

* 미용사에서 **화장품그룹** 총수된 에스티로더
* 이웅렬 **코오롱그룹** 부회장
* **10대그룹** 회장이라는 중량감과
* ■후계는 **누가=그룹** 안에서 고인의 ‘크기’가 이처럼 컸기에,
* 삼백(**三白)그룹** 회장의 차남이라는 타고난 족보 덕에

など，그룹の前に3音節ある文節は検索されません．

> 記号の扱いはやや曖昧です．上記の「(g&g)그룹」を例として，検索式とその意味，検索されるかどうかについてまとめると，以下の通りです．
> 
> | 検索式  | 指し示す部分  |  検索されるか |
> |:-----:|:---------:|:-----------:|
> | `?그룹` | )그룹    | 検索されない |
> | `??그룹`|g)그룹    |検索される |
> |`???그룹`| &g)그룹 |検索されない |
> |`????그룹` | g&g)그룹 |検索される|
> 
> 以上の結果から，記号は文節の始まりとしては扱われないようです．文節の境界か，または1つの音節として扱われることが分かります．

### 「%」＝0または1つの音節

`%`は，「0または1つの音節」を表します．例えば

```text
최%
```

という検索式は，「최という文字の後ろに1音節あってもなくてもよい文節」を指すことになります．結果は

* '**최** 의원님이 사시겠습니까?'
* 수술 전 그녀의 상태는 **최악**.
* "그러나 **최근** 신약개발에서 보듯
* "정치 신뢰회복 **최선** 다하겠다"
* 오스트리아 극우 자유당의 연정 참여에 반대하는 **최대** 규모의 시위가 19일 오후 빈에서 벌어졌다.
* 앞으로 **최장** 2년 동안
* 미국의 **최대** 일간지 유에스에이투데이(usa today)는 지난 7월 22일
* 세계 **최고** 반도체 회사 인텔
* 고려시대의 구성(龜城) 지방에 구들 시설이 있었다고 **최자**(崔滋)는 그의 저서인 『보한집(補閒集)』에 기록하고 있다.

などとなります．

`%`を連続して用いることもできます．例えば

```text
경%%도에
```

は，「경という文字の後ろに0～2音節があり，さらに도에が続く文節」ということになります．以下のような例が検索されます．

* 여주의 利浦를 출발해 한강을 따라 **경도에** 도착하는 경로를 취했던 것이다.
* 지난 1월 포항시에 건축허가를 신청한데 이어 지난 11일 **경북도에** 사업승인 신청을 했다.
* 반대로 각 지방의 문화요소가 **경기도에** 통합되어 완벽한 조화를 이루었다고 볼 수도 있다.
* 시강원 심대(沈垈)를 충청, 전라, **경상도에** 파견하여 근왕군을 모집하도록 명령하였다.
* 2백 40억원 규모의 임대 아파트를 지어 **경남도에** 기증한 창원 성원토건의 세 공동대표의 미담도 이에 못지 않다.
* 46번 **경춘국도에** 접해 있으며,
* 건국동맹은 이어서 각도 대표의 책임위원으로 충청남북도에 신표성(愼杓晟)·김종우(金鍾宇)·유웅경(劉熊慶)·장준(張逡), **경상남도에** 명도환(明道煥), **경상북도에** 이상훈(李相薰)·정운해(鄭雲海),
* 고대국가들의 **경제제도에** 있어서 고대국가들 성립 이후의 생산력  발전,
* "그런 시각에, 왜 그 사람이 **경인가도에** 있었는지 이해가 가지 않아요.

### 「*」＝0以上の音節

`*`は0以上の音節を表します．そのため，探したい語で終わるか，あるいは何らかの音節が続く例を検索したい場合に用います．例として，

```text
근처*
```

を検索してみると，

* 나는 **근처** 가게에서 담배 한 갑과 조그만 위스키 한 병을 사서
* 지금의 강릉여고 **근처** 용지촌에서 태어났다.

といった근처だけで成り立つ文節と，

* 관졸들이 이 **근처까지** 들이닥쳤습니다.
* 우리 집은 커녕 이 아파트 **근처에도** 얼씬거리지마.
* 첫날 배운 후 교통사고를 당하여 학교 **근처병원에** 입원한 동료교사의 문병을 갔다가,
* 국민학교가 있는 **근처에서부터** 거리로 먹혀 들어가는데 제방 둑 이편은 야채밭이었다.

など，근처に他の音節が何音節でも続く文節が検索されます．

ターゲットとする音節の前に`*`をつけた場合，ターゲットの前に他の音節がある場合とない場合とを検索してくれます．

```text
*가까이
```

を検索すると，가까이だけで成り立つ文節だけでなく，

* 강경대군의 죽음 이후 재야는 수만 또는 수십만의 군중동원과 결집된 힘으로 현정권을 **한달가까이** 숨가쁘게 몰아붙였다.
* **남자2:\<stage\>(가까이** 다가오며)\</stage\>그 사람은 이 과장.
* 호기심에 끌린 트래비는 **물체에가까이** 접근하고 그 순간 푸른 광선에 맞아 실신, ufo에 납치된다.
* 1982년 7월 개관, **20년가까이** 되다보니 건물들은 낡았고 페인트칠마저 빛바래져 있었다.
* **대원군:이리가까이** 와라.
* 시멘트의 경우는 성신양회 아시아시멘트등이 침수피해를 입어 **80만t가까이** 공급이 줄어들 것으로 예상돼
* 작년 한해 동안의 순매수(1905억원)의 **3배가까이** 넘어섰다.
* 자원봉사를 하게 된 동기에 대한 중복응답에서 **절반가까이**(47.5%)가

などのように，가까이の前に他の音節または記号を含む文節が検索されます．

> 가까이は助詞ではないため，上記の例の多くは分かち書きの誤りを含んでいることになりますが，実際はこうしたケースが多く含まれています．そのため，例えば가까이の用例を網羅したい場合，`*가까이*`のように，前後に`*`をつけておき，多めに用例を検索してから不必要な例を除くのが現実的です．

### 「+」＝1つ以上の音節

`+`は1つ以上の音節を表します．ターゲットとする音節に他の音節が**必ず**先行／後行する例だけが検索されます．例えば

```text
+근처+
```

という検索式では，

* 만화방의 최고 장소는 만화 애독자 층인 학생들이 많은 **학교근처나** 주택가.
* 가판대판매원은 역구내 **화장실근처에서** 20대 후반쯤 돼 보이는 청년을 소개해줬다.
* 따라서 백패킹을 즐길 경우 당일이나 1박2일코스의 단기 산행보다는 **사흘쯤계곡근처에서** 야영을 계획하고 떠나는 것이 보다 효과적이라 하겠다.
* 임금투쟁을 앞둔 지난 4월말, 노조측이 개최한 조합원총회에 참석한 노조원들을 **결근처리하겠다고** 나선 것이다.
* 몸이 **천근처럼** 무거웠다.
* 무 **당근처럼** 수세미로 표면의 흙을 닦는다.
* **등산(근처에** 유명한 산이 있다)도 하곤 하는 모습을 보았는데,

などの例が検索され，근처だけで成り立つ文節や근처で終わる文節，근처に助詞などが続く文節は検索されません．また，결**근처**리や천**근처**럼など，名詞の근처以外の例も検索される点に注意しましょう．

## 文節をまたいだ検索

ここまで説明した検索の方法は，その範囲が1つの文節内に限定されたものでした．以下では，複数の文節にまたがる検索について説明します．

### 隣り合う文節を検索する

検索式に

```text
문화 교육
```

のように，2つの項目をスペースで区切って並べると，2つの検索対象項目がこの順序で隣り合って出現する文節が検索されます．上記の場合，「문화」「교육」だけで成り立つ文節が，この順序で隣り合って出現するという例が検索されます．

* {한겨레신문}의 김선주 논설 위원은 우리 나라의 대중 **문화 교육** 기관의 실태에 대해 다음과 같이 말한다.
* 이달 중에 시의회 **문화 교육** 위원 회의실에서 감사를 재기키로 하고 해산했다.
* "정치 군사 이념적 차원이 아닌 사회 **문화 교육** 학술분야에서 전문인뿐 아니라 시민들의 교류를 추진할 계획"이라고 밝혔다.

そのため，2つの項目が逆の順で現れる，以下のような例は検索から外れます．

* 1970 년에는 대통령 **교육 문화** 담당 특별 보좌관에 임명되었으며, 문화 훈장 대통령장을 받았다.

また，「문화」「교육」だけで成り立つ文節が検索対象になるので，

* 결국 사람들의 대중 문화에 대한 생각을 바꾸어야 하므로 대중 **문화 교육이** 절대적으로 중요하다.
* 대중 **문화 교육은** 어떻게 해야 하나
* 학생들의 학습 발달 단계와 **문화 교육적** 표현 감각에 맞춰 대중 문화를 도입, 고급 예술 문화에 접목해 나가는 식으로 청소년 문화 활동에 대한 짜임새 있는 학교 교육 활동을 펴 나가자는 것이다.

のような文節は検索されません．

検索項目の順序については，項目が3つ以上の場合でも同様です．

```text
미술관 옆 동물원
```

という検索式では，

* \<간첩 리철진\> \<**미술관 옆 동물원**\> \<정사\> \<내 마음의 풍금\> \<파란 대문\> \<노랑머리\> 등이 소개되며,
* 일례로 영화 '아름다운 시절'을 보고 감동하기보다는 '**미술관 옆 동물원**'에서 프렌치 키스를 해야 할 때 뺨에 가볍게 키스하는 것에서 재미를 느낀다.

は検索されますが，もし仮に「동물원 옆 미술관」という例があっても，検索されません．

なお，それぞれの項目には[音節を表す演算子](#音節を表す演算子による検索)を用いることができます．

```text
*을 수% 있*
```

という検索式は「을だけか，을の前に1つ以上の音節がある文節」と「수だけか，수の後ろに1つの音節がある文節」と「있だけか，있の後ろに1つ以上の音節がある文節」がこの順序で隣り合っている例が検索されます．例は以下の通り．

* 한의원에서도 교통사고 환자들이 맘놓고 **치료받을 수 있는** 법적 제도적 체계가 하루빨리 마련됐으면 한다.
* 그는 아마도 부왕의 유골을 고국땅으로 옮겨 **묻을 수도 있었을** 것입니다.
* 여관일을 봐주는 아주머니에게 물어 그가 묵는 방을 쉽게 **찾을 수가 있었다**.
* 한마디로 말해서 만약을 위해 벽장 속에 총 한자루쯤 가지고 **있을 수는 있어도** 이 총은 반드시 자기방어를 위한 수단으로만 사용해야 한다는 것이다.
* 저 저녁 노을에 빛나는 가을 바윗산을 카메라에다 **담을 수야 있겠오**?

### ブーリアン演算子による検索

ブーリアン（불리언）演算子には`&`（AND）と`|`（OR）があり，項目のいずれかが成り立つ，あるいは全て成り立つ，といった条件を表すことができます．

> どちらの演算子も，前後に空白（スペース）を入れてはいけません．

#### 「&」＝AND・論理積

前後の項目が同時に現れる例を検索します．例えば

```text
가장&많은
```

という検索式は，

* 소설이 **가장 많은** 독자를 확보하고 있는 것은,

のように「가장」に続いて「많은」が出現する例だけでなく，

* 다임러 벤츠(daimler benz) 그룹은 독일에서 **_가장_ 규모가 큰 제조업체다. 이같은 강점 외에도 다임러 벤츠는 세계 최고급 자동차 브랜드인 메르세데스 벤츠를 가지고 있다. 이쯤 되면 _많은_** 사람들은 다임러가 돈을 긁어모으고 있다고 생각할 법하다.
* 그러나 **_가장_ 중요한 것은 - 그리고 안데스의 농민을 소문난 존재로 만든 것은 - 그들이 활용하고 가꾸는 식물들의 엄청나게 다양한 종류이다. 농부들은 식물들의 다양성과 다채로움을 수단으로 하여 다양하고 변화무쌍한 기후와 대화를 나누고 선물을 주고받는다. 예를 들어, 농부들이 건조한 해에 대비하여 좀더 고지대의 밭에 농작물을 심을 때 그들은 그 차크라속에서 어떤 식물들이 예상되는 가뭄에 특히 잘 적응하는 것인지를 안다. 이러한 식물들 덕분에 그 해의 수확은 보다 비가 _많은_** 계절의 수확과 거의 맞먹는 것으로 된다.

など，文をまたがった場合も含めて，「가장」の後に「많은」が出現する例を検索します．検索される項目の順序は，並べる順序に準じます．そのため，上記の検索式では「많은」の後に「가장」が出現する例は該当しません．

> 検索の範囲は，元のファイルの`<p>`タグ内に限定されるようです．

以下のように，3つ以上の項目を並べることもできます．

```text
가장&많이&나타나는
```

この検索式では，

* 통계 자료를 정리할 때 **가장 많이 나타나는** 변량의 값.

といった例だけでなく，

* 현재 우울증은 뇌에서 분비되는 신경전달물질의 일종인 세로토닌의 저하가 **_가장_ 직접적인 원인이라고 알려져 있습니다. 우리의 감정은 뇌에서 나오는 다양한 호르몬에 의해 좌우됩니다. 예를 들어, 도파민은 흥분을, 엔케팔린류는 행복과 극치감을 느끼게 해주고, 아드레날린은 긴장과 날카로운 느낌을 가져옵니다. 세로토닌도 비슷한 작용을 하며 뇌에서 그 분비가 적어지면, 사람들은 우울한 기분을 느끼게 되죠. 앞에서 언급한 여러 종류의 스트레스들은 세로토닌의 분비를 억제합니다. 뿐만 아니라, 세로토닌의 분비 이상은 세로토닌 신경 전달 체계 중 5-ht1b 자가수용체(5-ht1b autoreceptor)라는 세포 단백질이 너무 _많이_ 생겨서 _나타나는_** 것으로 알려져 있습니다.

のように，「가장」「많이」「나타나는」の順序でそれぞれの文節が出現する例も検索されます．

なお，`A&B`という検索式で，文節Aの後に文節Bが複数現れる場合，最初に出現する文節Bまでが検索に該当します．例えば，

```text
*한테%&미스
```

という検索式（「한테の前に0以上の音節があり，後ろに1つの音節がある文節」の後ろに「미스」という文節が現れる）で検索される以下の例

* **_아무한테나_ 살살 눈웃음치는 _미스_** 리? 아니면, 호젓한 카페에서 시집 읽을 때가 제일 행복해요, 하는 _미스_ 박? 시집이나 갈 일이지, 그 여자 시라는 게 뭔지나 알고 그럴까, 참. 어쨌거나 그런 폼 잡는 _미스_ 박은 아무래도 이런 건 우습달 거고, 그렇다고 화장이라면 몰라도 이런 고상한 취밀 _미스_ 리가 가지고 있을 법하지도 않은데, 더구나 _미스_ 리 타자 솜씨라고는 상상이 안 되거든.

では，最初の「아무한테나」以降，「미스」という文節が複数現れますが，検索にマッチするのは太字で示している，最初の「미스」までです．

#### 「|」＝OR・論理和

前後の項目のどちらかが出現する例を検索します．単独の文節における複数の候補を検索する，という点では，「文節をまたいだ検索」ではないかもしれません．

```text
한국어|우리말
```

という検索式では，「한국어」もしくは「우리말」という文節を検索します．

* 영어와 달리, 띄어쓰기 등 철자법 규정이 엄격하지 않은 **한국어** 텍스트는 그 교정을 위해 많은 시간이 필요하였다.
* 처음 출판되고 40년이 지나 **우리말** 번역본이 나왔으니 어찌 보면 약간 뒤늦은 감이 없지 않지만 이 책은 우리에게도 대단히 큰 영감을 줄 것이 분명하다.

この演算子も，複数の項目を並べることができます．

```text
한국어|한국말|우리말
```

といった検索が可能です．いずれかの項目が出現する例を検索するため，項目の並び順は問いません．

#### ブーリアン演算子の組み合わせ

`&`と`|`を組み合わせて用いることができます．その場合は，優先順位に応じて`()`でくくりますが，`()`でくくった部分が優先されます．例えば，

```text
(가장|제일)&많은
```

という検索式では，「가장か제일という文節の後に，많은という文節が現れる」例（2,050例）が検索されます．

* "초콜릿을주고받는 밸런타인 데이 때는 난리법석인데 전래 민속놀이가 **가장 많은** 명절과 정월 대보름날 축제는 점차 밀려나고 있습니다.
* 이렇게 한 장씩 빼어던지는 지편은 일정한 규칙에 따라, 그 중 **제일 많은** 끗수를 낸 사람이 다른 석  장을 먹게 되는데, 그 4매 1조를  ‘한(一)수’라 하고, 네 사람 중 가장 여러 수를 먹은 사람이 이기게 된다.
* **_가장_ 두드러진 것은 핸드폰. 폴더형이 보편화하면서 삼성·엘지·현대 등 각사가 출시하는 제품들이 하루가 다르게 무게와 부피가 줄고 있다. 최소형은 지난 3월 삼성전자가 내놓은 `워치폰'이다. _많은_** 업체들이 배터리 용량 문제로 상용화하지 못했으나 삼성이 처음으로 판매를 시작했다.
* 인민재판에 회부돼서 당장 목숨을 잃었거나 모진 벌을 받고 있을 줄 알았는데 인민 총궐기대회에서 **_제일_ 먼저 의용군을 지원해서 _많은_** 젊은이들로 하여금 감격해서 동조케 했다는 소식이었다.

カッコの位置を変えて，

```text
가장|(제일&많은)
```

とすると，「가장という文節」あるいは「제일という文節の後に많은という文節が現れる」例が検索されることになります．この検索式は「가장」だけで成り立つ例を含むので，検索される例は非常に多くなります（35,360例）．

> カッコを外して，`가장|제일&많은`という検索を行ったところ，`가장|(제일&많은)`と同じ結果になりました．`가장&제일|많은`も`(가장&제일)|많은`と同じ結果になったので，カッコがない場合，`|`よりも`&`が優先され，先に処理されるようです．

また，

```text
(아주|매우)&(좋은|나쁜)
```

という検索式は，「아주か매우という文節の後に，좋은か나쁜という文節が現れる」，以下のような例が検索されます．

* 그믐께 쯤해서 마음에 둔 어른을 뵙고 세배를 하는 것은 **매우 좋은** 생각이라 하겠다.
* 심장병을 앓고 있는 자나, 불면증 환자, 잘 놀라는 자, 무서움을 타는 자에게 **아주 좋은** 약주가 된다.
* ▴구학서 신세계 사장=지표상 소비가 **매우 나쁜** 건 틀림없다.
* 알고 보니 선생님은 **아주 나쁜** 사람이군요.
* 향미:노름해서 노름빚 왕창왕창 빚지고 해서 지금 **_아주_ 안 _좋은_** 쪽으로 팔려 다닐 거예요.
* 젊은 남녀가 일생을 좌우할 혼인을 전제로 하여 맞선을 본다는 것은 **_매우_ 조심스러운 일이요, 신중을 기해야 하는 일이다. 쉽게 말해서 이 세상에서 가장 _좋은_** 사람, 가장 마음에 드는 사람을 찾아내는 일이다.
* **_아주_ 냉철하게 논리적으로 북한과 김정일을 기술했기 때문에 앞으로의 남북관계 일을 하는데 _좋은_** 참고서가 될 것이다.

> 上述の通り，ブーリアン演算子のうち`&`が優先されるので，この検索式からカッコを外した`아주|매우&좋은|나쁜`は，`아주|(매우&좋은)|나쁜`を検索するのと同じことになります．

さらに，

```text
(아주|매우)&(좋은|나쁜)&(사람|놈)
```

という検索式は，「아주か매우という文節」，「좋은か나쁜という文節」，「사람か놈という文節」がこの順序で並ぶ例を指します．

* 노인을 이번 일에서 손 떼게 하려면 '박학수 **아주 나쁜 놈**'이란 데서 더이상 얘기를 진전시키지 말아야 했다.
* 우승 상금과 맞먹는 돈을 하루에 벌 수 있으니까 한턱 쓰는 사람이나, 공짜 음식을 먹는 사람이나 부담이 안 가는 **_아주_ 기분좋은 광경이다. 인터뷰에서도 박지은은 아주 능숙하고 거침이 없다. 본인의 능력과 노력도 뛰어났겠지만 일찍이 외국에 나가 학업과 운동을 병행할 수 있도록 뒷받침을 해준 부모가 있었기 때문에 부자의 여유로움과 _좋은_ 환경에서 자란 _사람_** 특유의 자신감이 배어났다.
* 물론 다른 사람이 당신의 기술이나 외모, 재산 등을 평가하는 것은 **_매우_ 정확할 수도 있다. 하지만 당신의 기술이나 외모, 재산 등을 다져서 당신을 '_좋은_ _사람_**' 혹은 '나쁜 사람'이라고 판단하는 것은 결코 옳은 일이 아니다.
* 올바른 手順이나 합당한 행마법에서 벗어난, **_아주_ _나쁜_ 수를 악수라 한다. 상대방이 올바르게 대응할 경우, 당연히 악수는 그것을 둔 _사람_** 쪽에 매우 불리한 결과를 가져온다.

### 文節の範囲を指定した検索

ある項目Aを基準として，その前後の範囲を文節数で指定し，指定した範囲内に項目Bが現れる，というような検索を行うことができます．範囲の指定には`@`を用い，`@`の前後に，検索の範囲として指定する文節数を数字で記述します．例えば，項目Aの前（＝左側）3文節以内か，後ろ（＝右側）2文節以内に項目Bが現れる，という検索を行う場合は，

```text
項目A 3@2 項目B
```

のような検索式を書きます．項目Aと範囲指定の式，項目Bの間には，それぞれスペースが必要です．範囲指定の数に0を指定すると，そちら側の範囲は除外されます．例えば`3@0`であれば，基準とする項目の前3文節だけが範囲として指定されます．0の代わりに数字を省略して，`3@`としても同じです．

#### 他の演算子との組み合わせ

範囲を指定した検索においても，音節を表す演算子やブーリアン演算子を用いることができます．例えば，

```text
?거나 3@ +거나
```

という検索式は，「거나の前に1音節ある文節」（＝`?거나`）の「左側3文節以内」（＝`3@`）に，「거나の前に1音節以上ある文節」（＝`+거나`）があるという，以下のような例を検索することができます．

* 이 말의 뜻은, 현재 네가 받고 있는 정치는 **좋거나 궂거나** 다 너 스스로가 벌어얻은 것이라 함에 있다.
* 그들은 오염을 **유발하거나 쓰레기가 되거나** 또는 어떤 식으로든 생태계를 손상시키는 제품은 우수한 제품이 아니라는 것을 알아차리고, 환경관리 프로그램을 품질관리 프로그램과 통합했습니다.
* 세간에 **있거나 세간을 떠나 있거나** 인욕(人慾)을 따르는 것도 고통이요, 인욕을 끊는 것도  고통이라 하였다.

また，ブーリアン演算子も使用した

```text
절대 @5 (않+|없+|안*)
```

という検索式は，「절대だけで成り立つ文節」（＝`절대`）の「右側5文節以内」（＝`5@`）に，「않に1音節以上続く文節か없に1音節以上続く文節か안に0以上の音節が続く文節」（＝`(않+|없+|안*)`）があるという例を検索することになります．検索される例は以下の通り．

* "누가 만일 날 찾거든 내가 여기 있다고 말해선 **절대 안** 된다."
* 거기서 있었던 일은 **절대 말하지 않는다는** 내용의 각서에 지장을 찍고.
* **절대 진리가 없다면** 진리는 절대로 없다는 이 무섭고도 한심한 이분법!
* 터키식 커피는 **절대 주문하면 안된다는** 점!
* 지금도 생생하게 억양과 음색이 기억이 나고 있는데, 그날 당신은 내게, 천만에요, **절대 그럴 리가 없죠,** 라는 말을 두 번이나 했었어.
* **절대 흥분해서 앞으로 나가면 안** 돼.
* 그녀는 **절대 그보다 더 빠를 수가 없었다.**

## 字素の検索

1つの音節内での字母を指定して，検索を行うことができます（ガイドp.25「5. 가. 어절 검색 5) 자소 검색식」）．1つの音節を`[  ]`でくくり，`[初声,中声,終声]`のように，字母をコンマで区切って指定します．`[  ]`内，コンマの前後にスペースが入らないようにしてください．ただし，「終声がない」例を検索する場合に限り，「終声」として` `（スペース）を記述することが可能です．

なお，字母の代わりに以下のような演算子を用いることができます．

| 演算子 | 意味・役割 | 使用可能な箇所 |
|:---:|:--------:|:------------:|
| ? | 何らかの字母が必ずあり，その字母は何でもよい | 初声・中声・終声 |
| % | 字母があってもなくてもよい | 終声のみ |

要するに，`?`は「1つの字母」を表し，`%`は「0または1つの終声字母」を表すということになります．なお，初声・中声・終声の全てに1つずつ字母を記述することはできません．少なくとも一つは，上記の演算子を含む必要があります．

いくつか例を挙げます：

* `[ㄱ,?,ㄴ]`：初声はㄱ，中声は何でもよい，終声はㄴ＝간, 갠, 갼, 근, ...
* `[?,ㅗ,ㄹ]`：初声は何でもよい，中声はㅗ，終声はㄹ＝골, 꼴, 놀, 돌, 똘, ...
* `[ㅂ,?, ]`：初声はㅂ，中声は何でもよい，終声はない＝바, 뱌, 배, 뱨, 베, ...
* `[ㅂ,?,%]`：初声はㅂ，中声は何でもよい，終声はないか，何でもよい＝바, 박, 밖, 보, 봄, 부, 불, ...
* `[ㅂ,?,?]`：初声はㅂ，中声は何でもよい，終声は必ずあるが，何でもよい＝박, 밖, 봄, 불, 뱀, ...

複数を列挙することも可能です：

* `[ㄱ,?,ㅇ][?,?,ㅇ]`：강정, 경영, 강장, 강령, 공용, 긍정, ...

> 日本語版のWindows 7上で上記の検索を行ったところ，いずれも検索結果がありませんでした．字母による検索は，韓国語版のWindowsでのみ実行可能なようです．
