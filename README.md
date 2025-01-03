# 電子質問票システム OLSPA-Q
**O**ff**L**ine-compatible **S**ingle**P**age**A**pplication-**Q**uestionnair

* Version : 1.1
* LastUpdate : 2024/12/23
* Copyright : えくせらぼソフト 2024-
* Depend-on : jQuery(v3.7.1, slim)

---

### はじめに

OLASPA-Q(以下本ソフト)はインターネット非接続下でも動作可能なアンケート集計ソフトです。
Webブラウザをもちいており、Operating Systemに依存せず使用することが可能です。
質問票の結果はシステムで設定されているダウンロードフォルダへ指定の様式で格納されます。


### 要件

* Operating System(OS) : 指定なし
* Webブラウザ : Chrome/Edge/Firefox/Internet Explorer/Safari/Opera/Stock browser on Android/Safari on iOS
   ※JavaScriptが動作する必要があります。


---


### 基本動作

HTMLファイルとconfigフォルダが動作に必要です。これらファイルを任意のフォルダで展開して下さい。
app.zipを任意のフォルダで展開することでも使用することが可能です。
開始するには付属しているHTMLファイル(デフォルトはindex.html)を起動することで開始できます。

事前設定として識別子(ID)を入力した状態で、回答者に端末を渡して下さい。
各種入力した後に回答ボタンが押されると、入力値をダウンロードフォルダに出力します。
後述方法で指定したファイル名で回答内容が保存されます。

質問内容などは同梱の「settings.txt」を編集することで変更が可能です。
なお、他のファイル名に変更するとシステムが読み取れませんので注意して下さい。


### 設定詳細 : フォーム設定

「settings_デモ.txt」を参考に適宜修正して下さい。
詳細はJavaScriptの知識があると比較的安全に調整することが可能です。

**! 表記方法に留意して下さい。項目名や書式が正しくない場合、正常に動作しなくなります。**


#### テンプレート(◇◇◇の部分は編集可)

```
const form_option = {
  "title": "◇◇◇",
  "savefilename": "◇◇◇",
  "saveforat": ["◇◇◇"],
  "question": [
    {
      "id": ◇◇◇,
      "caption": "◇◇◇",
      "type": "◇◇◇",
      "required": ◇◇◇,
      "detail": ◇◇◇
    },
  ]
}
```

#### 項目詳細

* title : 質問フォームのタイトルを入力します。
* savefilename : ファイルで保存する場合、そのファイル名を指定します。実際に保存されるファイル名は「(savefilenameの指定)_(質問票ID).(saveformatの指定)」となります。
* saveformat : 出力の保存形式を設定します。現在以下をサポートしています。
   * txt : テキスト形式(.txt)。"csv"を同時に指定することはできません。
   * csv : CSVファイル(.csv)。"txt"を同時に指定することはできません。
   * view : データは出力せず、ページ上に結果を出すことができます。
* question : 質問項目を定義する場所です。質問の個数に応じて、内部の波括弧範囲({"id": ...}で定義される範囲)を複写して下さい。
   * id : 通し番号です。1、2、3、4…と順番に指定して下さい。0は使用できません。後述の保存形式に影響しますので、数字を飛ばして指定することはできません。
   * caption : 質問文を入力します。
   * type : 回答形式を指定します。以下から適切なものを指定して下さい。
      ##### 一般
        * text : テキスト入力
        * password : テキスト入力されますが、入力値が表示されません(ファイル出力時は普通のテキストが取得されます。暗号化されていないので扱いは最大限に注意が必要であり、暗証番号など重要な個人情報の取得には使用しないで下さい。)
      ##### 数値系
        * number : テキストで任意の数字を入力できます
        * range : 数値を視覚的バーで入力できます
      ##### 選択式
        * checkbox : 選択肢に対してOn / Offを選びます
        * radio : 複数の選択肢から1つのみ選びます
      ##### 時間系
        * datetime-local : 日時入力
        * time : 時刻入力
        * date : 日付入力
        * month : 年月入力
   * required : 入力必須とするかどうかの設定が可能です。「true(必須)」/「false(任意)」で指定して下さい。
   * detail : 追加の設定を指定します。「type」に応じて以下の書式で設定できます。不要であれば省略も可能です。
      ##### 一般
      * `"detail": {"minlength": 最小文字数, "maxlength": 最大文字数}`
      ##### 数値系
      * `"detail": {"min": 最小値, "max": 最大値, "step": 変化単位(既定は1, 実数にしたい場合はany)}`
      ##### 選択式
      * `"detail": ["選択肢1", "選択肢2", "選択肢3",...]`
      ##### 時間系
      * `"detail": {"min": 最小値, "max": 最大値}`


### 設定詳細 : 結果の保存書式設定

「settings_デモ.txt」を参考に適宜修正して下さい。
詳細はJavaScriptの知識があると比較的安全に調整することが可能です。

**! 表記方法に留意して下さい。項目名や書式が正しくない場合、正常に動作しなくなります。**


#### テンプレート(◇◇◇の部分は編集可)

```
const res_temp = function(s, d) {return `◇◇◇`}
```

#### 項目詳細

テンプレートを適宜編集して下さい。改行はそのまま保存されます。一部の特殊文字(「`」「{」「}」「$」)は使用を控えて下さい。
テンプレートに以下の書式で記入することでアンケート結果を反映させることができます。
* `${s[◇◇◇]}` : 入力値もしくは文字列が取得されます。◇◇◇には質問のidを入力して下さい。選択式の質問では、選択肢の文言が反映されます。「`${s[0]}`」で質問票ID([指定の識別子]_[日時])が反映されます。
* `${d[◇◇◇]}` : 入力値もしくは番号が取得されます。◇◇◇には質問のidを入力して下さい。選択式の質問では、選択肢の番号が反映されます。


---

## 更新履歴
2024/12/22 Ver1.0
2024/12/23 Ver1.1 : 複数の出力形式に対応