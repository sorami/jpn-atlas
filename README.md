# Japan Atlas TopoJSON / 日本アトラスTopoJSON

便利な日本の国、都道府県、地方公共団体のレベルで堺データをもらい方です。
このデータの元は[国土地理院の地球地図ー2016](http://www.gsi.go.jp/kankyochiri/gm_jpn.html).

An easy way to access Japanese geospatial boundary data at the national, prefectural, and municipal levels.
The data is taken from the [Geospatial Information Authority of Japan's "Global Map Japan" published in 2016](http://www.gsi.go.jp/kankyochiri/gm_japan_e.html).

## Usage / 用いること

jpn-atlasは[TopoJSON](https://github.com/topojson/topojson)フォーマットで届けられます。SVGとかCanvasなどの座標系のために作られました。このため、ダウンロード、変換、簡略、投影、または掃除しなくて便利に使える。
データは一番簡単なもらう方法が[unpkg](https://unpkg.com/jpn-atlas/)からです。そろとも、[npm](https://www.npmjs.com/package/jpn-atlas)から自分でインストールできます。

jpn-atlas is delivered in [TopoJSON](https://github.com/topojson/topojson) file format and made for SVG and Canvas coordinate systems (with the 'y' axis being reversed).
Because of this, it is convenient to use jpn-atlas to load Japanese geospatial data into a browser application without having to download, convert, simplify, and clean the data yourself, jpn-atlas has already done this for you.
The easiest way to get the jpn-atlas data is via [unpkg](https://unpkg.com/jpn-atlas/), but you can also install it via [npm](https://www.npmjs.com/package/jpn-atlas) for yourself.
Either of these methods, among others, will allow you to access the boundary data:

### Example / 例

[unpkg](https://unpkg.com/jpn-atlas@1.0.0/)からブラウザのSVGで、[d3-geo](https://github.com/d3/d3-geo)でデータが表示される：

In-browser SVG via [unpkg](https://unpkg.com/jpn-atlas@1.0.0/), displayed using [d3-geo](https://github.com/d3/d3-geo):

```html
<!DOCTYPE html>
<svg width="850" height="680" fill="none" stroke="#000" stroke-linejoin="round" stroke-linecap="round"></svg>
<script src="https://d3js.org/d3.v4.min.js"></script>
<script src="https://unpkg.com/topojson-client@3"></script>
<script>

var svg = d3.select("svg");

var path = d3.geoPath();

d3.json("https://unpkg.com/jpn-atlas@1/japan.json", function(error, japan) {
  if (error) throw error;

  svg.append("path")
      .attr("stroke", "#aaa")
      .attr("stroke-width", 0.5)
      .attr("d", path(topojson.mesh(japan, japan.objects.municipalities, function(a, b) { return a !== b && (a.id / 1000 | 0) === (b.id / 1000 | 0); })));

  svg.append("path")
      .attr("stroke-width", 0.5)
      .attr("d", path(topojson.mesh(japan, japan.objects.prefectures, function(a, b) { return a !== b; })));

  svg.append("path")
      .attr("stroke-width", 0.5)
      .attr("d", path(topojson.feature(japan, japan.objects.country)));
});

</script>
```

[unpkg](https://unpkg.com/jpn-atlas@1.0.0/)からブラウザのCanvasで、[d3-geo](https://github.com/d3/d3-geo)でデータが表示される：

In-browser Canvas via [unpkg](https://unpkg.com/jpn-atlas@1.0.0/), displayed using [d3-geo](https://github.com/d3/d3-geo):

```html
<!DOCTYPE html>
nvas width="850" height="680"></canvas>
<script src="https://d3js.org/d3.v4.min.js"></script>
<script src="https://unpkg.com/topojson-client@3"></script>
<script>

var context = d3.select("canvas").node().getContext("2d"),
    path = d3.geoPath().context(context);

d3.json("https://unpkg.com/jpn-atlas@1/japan/japan.json", function(error, japan) {
  if (error) throw error;

  context.beginPath();
  path(topojson.mesh(japan));
  context.stroke();
});

</script>
```

## File Reference

<a href="#japan/japan.json" name="japan.json">#</a> <b>japan/japan.json</b> [<>](https://unpkg.com/jpn-atlas@1/japan/japan.json "Source")

このファイルは３つの[*Geometry Collection*](https://s.kitazaki.name/docs/geojson-spec-ja.html#geometry-collection)が含む [TopoJSONの*topology*](https://github.com/topojson/topojson/wiki/Specification.ja)です。データは[d3.geoAzimuthalEqualArea](https://github.com/d3/d3-geo/blob/master/README.md#geoAzimuthalEqualArea)で投影されて、850x680のビューポートにフィットされて、簡略されています。
データのトポロジーは[国土地理院の地球地図ー2016](http://www.gsi.go.jp/kankyochiri/gm_jpn.html)から導かれました。
都道府県の堺は地方公共団体が[merge](https://github.com/topojson/topojson-client/blob/master/README.md#merge)されたことの結果です。同じように国の堺は都道府県[merge](https://github.com/topojson/topojson-client/blob/master/README.md#merge)されたことの結果です。

このTopoJSONデータには各都道府県と地方公共団体の`id`プロパティ[全国地方公共団体コード](http://www.soumu.go.jp/denshijiti/code.html)が付いています。例えば、札幌市のコードは01100だから札幌市のfeatureの中で`id : 01100`のプロパティが付いています。

This file is a [TopoJSON *topology*](https://github.com/topojson/topojson-specification/blob/master/README.md#21-topology-objects) containing three geometry collections: <i>municipalities</i>, <i>prefectures</i>, and <i>country</i>.
The geometry is quantized using [topojson-client](https://github.com/topojson/topojson-client/blob/master/README.md#quantize), projected using [d3.geoAzimuthalEqualArea](https://github.com/d3/d3-geo#geoAzimuthalEqualArea) to fit a 850x680 viewport, and simplified.
The topology is derived from the Geospatial Information Authority of Japan's [Global Map Japan](http://www.gsi.go.jp/kankyochiri/gm_japan_e.html), published in 2016.
Prefecture boundaries are computed by [merging](https://github.com/topojson/topojson-client/blob/master/README.md#merge) municipalities, and country boundaries are computed by merging prefectures.

The TopoJSON data assigns each municipality and prefecture an administrative code that can be found under the `id` property. For instance, the administrative code for Sapporo City is 01100. So the Sapporo feature has an associated `id : 01100` object within it. [The official source for the administrative codes](http://www.soumu.go.jp/denshijiti/code.html) is only in Japanese. However, Nobu Funaki has a created a handy github repository called [list-og-cities-in-japan](https://github.com/nobuf/list-of-cities-in-japan), which generates this information in English.

<a href="#japan/japan.json_municipalities" name="japan/japan.json_municipalities">#</a> *japan*.objects.<b>municipalities(地方公共団体)</b>

<img src="https://raw.githubusercontent.com/biskwikman/jpn-atlas/master/img/japan-municipalities.png" width="850" height="680">

<a href="#japan/japan.json_prefectures" name="japan/japan.json_prefectures">#</a> *japan*.objects.<b>prefectures(都道府県)</b>

<img src="https://raw.githubusercontent.com/biskwikman/jpn-atlas/master/img/japan-prefectures.png" width="850" height="680">

<a href="#japan/japan.json_country" name="japan/japan.json_country">#</a> *japan*.objects.<b>country(国)</b>

<img src="https://raw.githubusercontent.com/biskwikman/jpn-atlas/master/img/japan-country.png" width="850" height="680">

### Contributing / コントリビューション

誰かがコントリビューションに興味があれば、issueを開けて下さい。または、もうあけてあるissueでポーストをしてください。私の日本語はきれいじゃなくて、このReadmeの日本語をわからなかったら新しい訳語を提供してください！

If anyone is interested in contributing go ahead and open an issue or comment on an existing one. Any and all help and ideas are welcome.

#### Acknowledgment / 礼状

このプロジェクトは[us-atlas](https://github.com/topojson/us-atlas)から全部ぬすまれました。

This whole project is inpsired by / stolen from [us-atlas](https://github.com/topojson/us-atlas), which produces US county, state, and nation data.
