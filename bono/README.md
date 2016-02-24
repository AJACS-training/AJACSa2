# AJACSa三島2

情報・システム研究機構 ライフサイエンス統合データベースセンター  
[坊農 秀雅](http://bonohu.jp/) bono@dbcls.rois.ac.jp  
2016年2月25日


----

これは統合データベース講習会AJACSa三島2の資料です。 

----

## 概要

本講習は、NGSによる遺伝子発現データ解析について学びます。 

----

## 講習の流れ

講習では[次世代シークエンサーDRY解析教本](https://books.google.co.jp/books?id=ZujwCgAAQBAJ)(以下DRY解析教本)のp10「遺伝子発現解析」のシラバスをベースに、DRY解析教本のLevel2「②発現解析」したがって、皆さんとやっていきます。早くできてしまった人は、どんどん先に進んでやってください。詰まってしまった人はいつでも手を挙げて講師やスタッフを呼び止めてください。

## 参考
### NGSそのもの

- 清水厚志 **次世代シーケンサーデータの取扱と疾患ゲノム解析の基礎** 統合データベース講習会AJACS津軽 [DOI: 10.7875/togotv.2015.110](http://doi.org/10.7875/togotv.2015.110)
- 鈴木絢子 **次世代シーケンサーを用いたがん細胞のオミクス解析** 統合データベース講習会AJACS米子 [DOI: 10.7875/togotv.2015.090](http://doi.org/10.7875/togotv.2015.090)

 
### NGSデータ解析
-  坊農秀雅 **次世代シークエンサーにより得られたデータの解析** *領域融合レビュー*, **4**, e008 (2015) [DOI: 10.7875/leading.author.4.e008](http://doi.org/10.7875/leading.author.4.e008) 
- 坊農秀雅 **次世代シーケンサーから得られたデータの使い方** 統合データベース講習会AJACS薩摩 [DOI: 10.7875/togotv.2016.040](http://doi.org/10.7875/togotv.2016.040)
- バイオサイエンスデータベースセンター(NBDC) [平成27年度NGSハンズオン講習会の講義資料と動画(統合TV)](http://biosciencedbc.jp/human/human-resources/workshop/h27)

--
## はじめに-NGSデータ解析の概要

- アンケート
	- NGS使っている?
	- 解析すべき自分たちのNGSデータを所持している

NGSそのものの話は過去のAJACSの講義、例えば[AJACS津軽の清水さんの講習](http://togotv.dbcls.jp/ja/20151121.html)を参照してください。

[![leading_authors](http://leading.lifesciencedb.jp/wordpress/wp-content/uploads/2015/05/Bono-4.e008-Fig.2.jpg)](http://dx.doi.org/10.7875/leading.author.4.e008)

出展: 領域融合レビュー, **4**, e008 (2015) [DOI: 10.7875/leading.author.4.e008](http://dx.doi.org/10.7875/leading.author.4.e008) の **図2 データ解析のハブとなるFASTQ形式**

###FASTQ形式
FASTQフォーマット、とも言う。
- 4行/readが基本単位
 - 1行目: @ID (コメント)
 - 2行目: 塩基配列
 - 3行目: +ID
 - 4行目: クオリティ値
- したがって、5000万リード(MiSeq v3) x 4行 = 2億行
 - ファイルサイズも8Gbyteを超えることも
 - FAT32フォーマットでは扱えず、NTFSやexFAT
 - いわゆる「開く」ことが不可能

```FASTQファイルの実例(抜粋)
SRR001356.1 2023DAAXX:5:1:123:563 length=33
TGTCGGTCCAGCTCGGCCTTGGGCTCCGTTTTC
+SRR001356.1 2023DAAXX:5:1:123:563 length=33
-IIIIIIII8IIIIIIIIIII6IIIIIIIII9I
@SRR001356.2 2023DAAXX:5:1:123:476 length=33
TCTGAACCCGACTCCCTTTCGATCGGCCGCGGG
+SRR001356.2 2023DAAXX:5:1:123:476 length=33
IIIIIIIIIIIIIIIIIIIIIGIIIIIII-III
@SRR001356.3 2023DAAXX:5:1:121:746 length=33
GTGGCAGCGTTTTTGGGCCCGCCGCTTGCCGTT
+SRR001356.3 2023DAAXX:5:1:121:746 length=33
IIIII&IIIIIIIIIIIIIIIIHI1IIIIIIII
```

シーケンサーの種類に関係なく、データ解析はFASTQ形式のファイルから、マッピングかアセンブルするのが基本。RNA-seqやChIP-seqというのは配列解読の応用の話で、後述します。

### マッピング (mapping)
- すでに決定されたゲノム配列に対して、手持ちの配列がそのどの部分に相当するか写像(マッピング)すること。
- ゲノム配列決定された生物種では、普通はこちらのアプローチで配列解析する。

### アセンブル (assemble)
- 手持ちの短い配列から元の長い配列(ゲノムなど)を組み立てる(assemble)こと。
- ゲノム配列を決定するプロセスで必要。

### NGSデータ解析に関わるデータフォーマット

|フォーマット名|読み方|拡張子|用途|
|:-----------|:----|:----|---|
|FASTA|ふぁすた or ふぁすとえー|	.faや.fasta	|1行目に“>”で始まるヘッダ行，2行目以降に実際の塩基配列文字列という配列データ形式|
|FASTQ|ふぁすときゅー|	.fqや.fastq	|NGS配列データ形式のデファクトスタンダード．配列クオリティ値付き，４行1エントリ|
|SRA|えすあーるえー|	.sra|	NGS配列データ配布フォーマット．NCBI SRA-toolkitを使ってFASTQを生成できる|
|SAM|さむ|	.sam|	ゲノムマッピングした時に生成されるアラインメントのフォーマット．プレーンテキスト形式|
|BAM|ばむ|	.bam|	ゲノムマッピングした時に生成されるアラインメントのフォーマット．バイナリ形式|
|GTF(GFF)|じーてぃーえふ (じーえふえふ)|	.gtf(.gff)|	ゲノム上のどこに遺伝子があるかなどが記述されたゲノムアノテーションのフォーマット|
|BED|べっど|	.bed	|ゲノム上のどこに遺伝子があるかなどが記述されたゲノムアノテーションのフォーマット|
|VCF|ぶいしーえふ|	.vcf	|Variant Call Format．配列の多型を記述するフォーマット．bcfはvcfのバイナリ版|

UCSC Genome Browser上に出ているTrackは、このBED形式のデータが基本となっています。

### よく用いられるNGSの応用
塩基配列情報が関係する研究分野、すべてに応用されています。以下の様な分野によく使われています。


1. ゲノムの解析(Exome, WGBS(Whole Genome Bisulfite Sequence))
2. トランスクリプトームの解析(RNA-seq)
3. DNA結合タンパク質の結合配列の解析(ChIP-seq)
4. メタゲノムの解析

[![leading_authors](http://leading.lifesciencedb.jp/wordpress/wp-content/uploads/2015/05/Bono-4.e008-Fig.1.jpg)](http://dx.doi.org/10.7875/leading.author.4.e008)

出展: 領域融合レビュー, **4**, e008 (2015) [DOI: 10.7875/leading.author.4.e008](http://dx.doi.org/10.7875/leading.author.4.e008) の **図1 公共データベースSRAへのエントリー数を研究分野ごとに分類したもの**

**どんな人でもNGSデータを用いて遺伝子発現解析ができます。**

## RNA-seq解析準備

ほぼ「Level1 ②コマンドラインの使い方」に従って進めていきます。

- アンケート
	- UNIXつかったことがある?
	- 日常的にUNIX使っている?
	- R使ったことがある?
	- 日常的にR使っている?
- コンピュータのセットアップ
	- ネットワーク設定等
	- それぞれのOS(Yosemite, El capitan等)でソフトウェア・アップデートをきっちりかけてあって、最新版になっていますか?
- Rのインストール　p44参照
- RStudioのインストール p46参照
- ターミナルの起動 p25参照
	- terminalで天気予報 `curl -4 wttr.in/Mishima`
	- 動きがおかしい…p38上から5行目 → [正誤表](http://gakken-mesh.jp/book/detail/9784780909203.html)をチェック
- コマンドライン・デベロッパ・ツールのインストール p40参照
- Homebrewのインストール p40参照
- 解析データを入れるフォルダの作成 p97参照

## 一通りの解析を終えて
- 解析の流れおさらい
- 有償ソフトウェアならここまでできます(RNA-seq編)
	- By 宮本真理さん(キアゲン)
