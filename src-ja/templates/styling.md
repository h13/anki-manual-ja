# スタイリング＆HTML

<!-- toc -->

## カードのスタイリング

YouTubeで[カードのスタイリングに関する動画](http://www.youtube.com/watch?v=F1j1Zx0mXME&yt:cc=on)をご覧いただけます。動画はAnki 2.0のインターフェースを示していますが、概念はほぼ同じです。

カード画面のスタイリングセクションは、「裏テンプレート」ボタンの隣にある「スタイリング」ボタンをクリックしてアクセスできます。そのセクションでは、カードの背景色、デフォルトフォント、テキスト配置などを変更できます。

利用可能な標準オプションは次のとおりです：

**font-family**\
カードで使用するフォントの名前。フォントに「MS Unicode」のようにスペースが含まれている場合は、この文のようにフォント名を二重引用符で囲む必要があります。1枚のカードで複数のフォントを使用することも可能です。詳細については以下をご覧ください。

**font-size**\
ピクセル単位のフォントサイズ。変更する際は、末尾のpxを残すようにしてください。

**text-align**\
テキストを中央揃え、左揃え、右揃えのいずれにするか。

**color**\
テキストの色。「blue」、「lightyellow」などの単純な色名が機能するか、HTMLカラーコードを使用して任意の色を選択できます。詳細については[このウェブページ](https://htmlcolorcodes.com/)をご覧ください。

**background-color**\
カードの背景色。

スタイリングセクションには任意のCSSを配置できます。上級ユーザーは、背景画像やグラデーションの追加などを行うことができます。特定のフォーマットを取得する方法を知りたい場合は、CSSでの実装方法についてウェブを検索してください。多くのドキュメントが利用可能です。

スタイリングはすべてのカード間で共有されます。つまり、調整を行うと、そのノートタイプのすべてのカードに影響します。ただし、カード固有のスタイリングを指定することも可能です。次の例では、最初のカード以外のすべてのカードで黄色の背景を使用します：

```css
.card {
  background-color: yellow;
}
.card1 {
  background-color: blue;
}
```

## 画像のリサイズ

Ankiはデフォルトで画像を画面に合わせて縮小します。これを変更するには、スタイリングセクションの最下部（デフォルトの`.card { ... }`の外側）に以下を追加します：

```css
img {
  max-width: none;
  max-height: none;
}
```

AnkiDroidは時々[画像を画面に合わせてスケーリングするのに問題があります](https://github.com/ankidroid/Anki-Android/issues/3612)。CSSを使用して最大画像寸法を設定すると修正されるはずですが、AnkiDroid 2.9では無視されるようです。修正方法は、各スタイルディレクティブに`!important`を追加することです。例：

```css
img {
  max-width: 300px !important;
  max-height: 300px !important;
}
```

画像のスタイルを変更しようとして、マークされたカードに表示される星が影響を受ける場合（例えば、大きくなりすぎる場合）、次のようにターゲットできます：

```css
img#star {
  ...;
}
```

Chromeを使用してカードのスタイリングをインタラクティブに探索できます：

<https://addon-docs.ankiweb.net/porting2.0.html#webview-changes>

Anki 2.1.50以降は、エディタ内での画像リサイズをネイティブにサポートしています。

## フィールドのスタイリング

デフォルトのスタイリングはカード全体に適用されます。特定のフィールドやカードの一部に異なるフォント、色などを使用させることもできます。これは外国語を学習する際に特に重要です。適切なフォントが選択されていない限り、Ankiが文字を正しく表示できない場合があるからです。

「Expression」フィールドがあり、OSXタイフォント「Ayuthaya」を適用したいとします。テンプレートが既に次のようになっているとします：

    What is {{Expression}}?

    {{Notes}}

スタイルを適用したいテキストをHTMLでラップする必要があります。テキストの前に以下を配置します：

    <div class=mystyle1>

そして後ろに以下を配置します：

    </div>

上記のようにテキストをラップすることで、ラップされたテキストを「mystyle1」というカスタムスタイルでスタイリングするようAnkiに指示します。このスタイルは後で作成します。

したがって、「What is …​?」の式全体にタイフォントを使用したい場合は、次のようにします：

    <div class=mystyle1>What is {{Expression}}?</div>

    {{Notes}}

そして、expressionフィールド自体のみにタイフォントを使用したい場合は、次のようにします：

    What is <div class=mystyle1>{{Expression}}</div>?

    {{Notes}}

テンプレートを編集した後、テンプレート間のスタイリングセクションに移動する必要があります。編集前は次のような外観になっているはずです：

```css
.card {
  font-family: arial;
  font-size: 20px;
  text-align: center;
  color: black;
  background-color: white;
}
```

新しいスタイルを最下部に追加して、次のようにします：

```css
.card {
  font-family: arial;
  font-size: 20px;
  text-align: center;
  color: black;
  background-color: white;
}

.mystyle1 {
  font-family: ayuthaya;
}
```

スタイルには任意のスタイリングを含めることができます。フォントサイズも大きくしたい場合は、mystyle1セクションを次のように変更します：

```css
.mystyle1 {
  font-family: ayuthaya;
  font-size: 30px;
}
```

デッキにカスタムフォントをバンドルすることも可能です。そうすれば、コンピューターやモバイルデバイスにインストールする必要がありません。詳細については[フォントのインストール](#フォントのインストール)セクションをご覧ください。

## 音声再生ボタン

カードに音声やテキスト読み上げが含まれている場合、Ankiは音声を再生するためにクリックできるボタンを表示します。

ボタンを表示したくない場合は、設定画面で非表示にできます。

カードのスタイリングで外観をカスタマイズできます。例えば、小さくして色を付けるには、次のようにします：

```css
.replay-button svg {
  width: 20px;
  height: 20px;
}
.replay-button svg circle {
  fill: blue;
}
.replay-button svg path {
  stroke: white;
  fill: green;
}
```

## テキストの方向

アラビア語やヘブライ語など、右から左に書かれる言語を使用する場合、復習中に正しく表示するために.cardセクションにCSSの`direction`プロパティを追加できます：

```css
.card {
  direction: rtl;
}
```

これにより、カード全体の方向が変更されます。特定のフィールドのみの方向を変更するには、参照をHTMLでラップします：

    <div dir="rtl">{{Front}}</div>

エディタでフィールドの方向を変更するには、[編集](../editing.md#customizing-fields)セクションをご覧ください。

## その他のHTML

テンプレートには任意のHTMLを含めることができます。つまり、インターネットのウェブページで使用されるすべてのレイアウトの可能性をカードでも使用できます。テーブル、リスト、画像、外部ページへのリンクなどがすべてサポートされています。例えばテーブルを使用すると、カードの表面と裏面が上下ではなく左右に表示されるようにレイアウトを変更できます。

HTMLのすべての機能をカバーすることはこのマニュアルの範囲外ですが、詳細を学びたい場合は、ウェブ上にHTMLの優れた入門ガイドがたくさんあります。

## ブラウザの外観

カードテンプレートが複雑な場合、[カードリスト](../browsing.md#cardnote-table)の質問と回答の列（「表」と「裏」と呼ばれる）が読みにくくなる可能性があります。「ブラウザの外観」オプションを使用すると、ブラウザでのみ使用されるカスタムテンプレートを定義できるため、重要なフィールドのみを含めたり、必要に応じて順序を変更したりできます。構文は標準のカードテンプレートと同じです。

このオプションを使用する場合、質問列のテキストが回答列の先頭で繰り返されている場合、Ankiは質問列にのみテキストを表示します。例えば、質問列のテキストが「People in Ladakh speak」で、回答が「People in Ladakh speak Ladakhi」の場合、回答列には「Ladakhi」のみが表示され、残りは省略されます。

## プラットフォーム固有のCSS

Ankiは、異なるプラットフォームで異なるスタイリングを定義できる特別なCSSクラスを定義しています。以下の例は、復習する場所に応じてフォントを変える方法を示しています：

```css
/* Windows */
.win .example {
  font-family: "Example1";
}
/* macOS */
.mac .example {
  font-family: "Example2";
}
/* Linuxデスクトップ */
.linux:not(.android) .example {
  font-family: "Example3";
}
/* LinuxデスクトップとAndroidデバイスの両方 */
.linux .example {
  font-family: "Example4";
}
/* AndroidとiOSの両方 */
.mobile .example {
  font-family: "Example5";
}
/* iOS */
.iphone .example,
.ipad .example {
  font-family: "Example6";
}
/* Android */
.android .example {
  font-family: "Example7";
}
```

そしてテンプレートでは：

```html
<div class="example">{{Field}}</div>
```

AnkiWebを使用する場合は、.gecko、.opera、.ieなどのプロパティを使用して特定のブラウザを選択することもできます。オプションの完全なリストについては、<http://rafael.adm.br/css_browser_selector/>をご覧ください。

## フォントのインストール

Ankiに直接フォントをインストールできます。これは、新しいフォントをインストールする権限がない職場や学校のコンピューターでAnkiを使用している場合や、モバイルデバイスでAnkiを使用している場合に便利です。

Ankiは、TrueType（.ttf）、OpenType（.otf）、Web Open Font Format（.woff）など、最も広く使用されているフォント形式をサポートしています。

### メディアフォルダにフォントを追加

「Arial.ttf」などのサポートされているフォントをダウンロードしたら、メディアフォルダに追加する必要があります。

1. ファイル名を変更し、先頭にアンダースコアを追加して「\_arial.ttf」のようにします。アンダースコアを追加することで、このファイルがテンプレートで使用され、未使用メディアをチェックする際に削除されないことをAnkiに伝えます。

2. コンピューターのファイルブラウザで、[Ankiフォルダ](../files.md)に移動し、「User 1」というフォルダ（またはプロファイルの名前を変更/追加した場合はプロファイル名）に移動します。

3. フォルダ内に、collection.mediaというフォルダが表示されるはずです。名前を変更したファイルをそのフォルダにドラッグします。

### テンプレートを更新してそのフォントを使用

フォントがメディアフォルダに追加された後、テンプレートを更新する必要があります。

1. メイン画面の上部にある**追加**をクリックし、左上のボタンで変更したいノートタイプを選択します。

2. **カード**をクリックします。

3. スタイリングセクションで、以下のテキストを最下部（最後の「}」文字の後）に追加し、「\_arial.ttf」をメディアフォルダにコピーしたファイル名に置き換えます：

```css
@font-face {
  font-family: myfont;
  src: url("_arial.ttf");
}
```

「arial」の部分のみを変更し、「myfont」の部分は変更しないでください。

その後、カード全体のフォントを変更するか、個々のフィールドのフォントを変更できます。カード全体のフォントを変更するには、.cardセクションのfont-family:行を見つけて、フォントを「myfont」に変更します。特定のフィールドのみのフォントを変更するには、上記の[フィールドのスタイリング](#フィールドのスタイリング)の手順をご覧ください。

ファイル名が正確に一致することを確認してください。ファイルがarial.TTFと呼ばれていて、カードテンプレートでarial.ttfと書いた場合、機能しません。

## ナイトモード

設定画面でナイトモードが有効になっているときのテンプレートの表示方法をカスタマイズできます。

より明るいグレーの背景が必要な場合は、次のようなものを使用できます：

```css
.card.nightMode {
  background-color: #555;
}
```

「myclass」スタイルがある場合、次のようにするとナイトモードが有効なときにテキストが黄色で表示されます：

```css
.nightMode .myclass {
  color: yellow;
}
```

## フェードとスクロール

Ankiはデフォルトで自動的に回答にスクロールします。id=answerを持つHTML要素を探し、そこまでスクロールします。スクロール位置を調整するために別の要素にidを配置したり、スクロールをオフにするためにid=answerを削除したりできます。

カードの質問側はデフォルトでフェードインします。この遅延を調整したい場合は、表カードテンプレートの先頭に以下を配置できます：

```html
<script>
  qFade = 100;
  if (typeof anki !== "undefined") anki.qFade = qFade;
</script>
```

100（ミリ秒）がデフォルトです。フェードを無効にするには0に設定します。

## Javascript

Ankiカードはウェブページとして扱われるため、カードテンプレートを介してカードにJavascriptを埋め込むことが可能です。良いリファレンスについては、フォーラムの[この投稿](https://forums.ankiweb.net/t/card-templates-user-input-101-buttons-keyboard-shortcuts-etc-guide/13756)をお読みください。

Javascriptは高度な機能であり、多くの問題が発生する可能性があるため、**Javascript機能はサポートや保証なしで提供されています**。Javascriptの記述に関する支援は提供できず、記述したコードが将来のAnkiアップデートで変更なしに動作し続けることを保証できません。遭遇した問題に自分で対処することに慣れていない場合は、Javascriptの使用を避けてください。

各Ankiクライアントはカード表示を異なる方法で実装する可能性があるため、プラットフォーム間で動作をテストする必要があります。多くのクライアントは、長時間実行されるウェブページを維持し、カードが復習されるときに動的に部分を更新することで実装されているため、Javascriptはdocument.write()のようなことをするのではなく、document.getElementById()のようなものを使用してドキュメントのセクションを更新する必要があります。

window.alertのような関数は利用できない場合があります。AnkiはJavascriptエラーをターミナルに書き込むため、それらを見るには[コンソールを表示](https://addon-docs.ankiweb.net/console-output.html#console-output)する必要があります。JavaScriptの問題をデバッグするには、Chromeの[インスペクタ](https://addon-docs.ankiweb.net/debugging.html#webviews)を使用できます。