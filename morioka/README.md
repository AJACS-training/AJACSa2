# ChIP-seq フォローアップ（森岡）

こちらでは、ChIP-seq解析を[Dry本](http://tinyurl.com/hypzh9u)ならびに森岡が担当したNGSハンズオン講習会2015の内容を一通り勉強した（中級者の）方が、次のステップとして必要な情報を紹介していきます。

- [平成27年度NGSハンズオン講習会の公式HP](http://biosciencedbc.jp/human/human-resources/workshop/h27)
- [2015全日程の講義資料](http://www.iu.a.u-tokyo.ac.jp/~kadota/r_seq.html#bioinfo_ngs_sokushu_2015)

- [統合データベース講習会AJACSa三島2](http://dbcls.rois.ac.jp/archives/3094)



#### [講義資料](https://drive.google.com/folderview?id=0B4oXGzAZTVKeflotMUpIMURYTVUwXzRXT3NlRUQ4NlRtaW9HdTlwVkI2ZHMwN1VrVlhqVkE&usp=sharing)


### Contents
- ChIP-seqのQC
	- [fastqc, picardtools, intersectBed](https://github.com/suimye/NGS_handson2015/wiki/NGS_beginner)
	- [NGSplotの使い方とNGSplotによるQC](https://github.com/suimye/NGS_handson2015/wiki/NGSplotsOnBiolinux8)
- [IGVの使い方(主に、TDFファイルを作る)](https://github.com/suimye/NGS_handson2015/wiki/ChIP-seq%E3%83%87%E3%83%BC%E3%82%BF%E3%81%AE%E5%8F%AF%E8%A6%96%E5%8C%96)
- [ChIP-seqデータの統合解析の方法 (Homer)](https://github.com/suimye/NGS_handson2015/wiki/Homer_Data_integration)
- [ChIP-seqデータの統合方法 (Ngsplot)](https://github.com/suimye/NGS_handson2015/wiki/Ngsplot_data_integration)
- [MEME suiteの使い方](https://github.com/suimye/NGS_handson2015/wiki/PeakCallAndMDA)

[![https://gyazo.com/5d148e75be600dd295730cb82560fe19](https://i.gyazo.com/5d148e75be600dd295730cb82560fe19.png)](https://gyazo.com/5d148e75be600dd295730cb82560fe19)



##紹介するデータについて

####DRY本 p263ページの五十嵐先生のページをある程度踏襲し、テストデータを作成し実施しています。

データは、
>ヒト大動脈内非細胞(HAEC)へのTSA/SAHA刺激のデータ

```
GSM1326443
GSM1326446
GSM916973
```

### Slides
![images/AJACSa2_morioka_01.jpg](https://raw.githubusercontent.com/AJACS-training/AJACSa2/master/morioka/images/AJACSa2_morioka_01.jpg)
---

![images/AJACSa2_morioka_02.jpg](https://raw.githubusercontent.com/AJACS-training/AJACSa2/master/morioka/images/AJACSa2_morioka_02.jpg)
---

![images/AJACSa2_morioka_03.jpg](https://raw.githubusercontent.com/AJACS-training/AJACSa2/master/morioka/images/AJACSa2_morioka_03.jpg)
---

![images/AJACSa2_morioka_04.jpg](https://raw.githubusercontent.com/AJACS-training/AJACSa2/master/morioka/images/AJACSa2_morioka_04.jpg)
---

![images/AJACSa2_morioka_05.jpg](https://raw.githubusercontent.com/AJACS-training/AJACSa2/master/morioka/images/AJACSa2_morioka_05.jpg)
---

![images/AJACSa2_morioka_06.jpg](https://raw.githubusercontent.com/AJACS-training/AJACSa2/master/morioka/images/AJACSa2_morioka_06.jpg)
---

![images/AJACSa2_morioka_07.jpg](https://raw.githubusercontent.com/AJACS-training/AJACSa2/master/morioka/images/AJACSa2_morioka_07.jpg)
---

![images/AJACSa2_morioka_08.jpg](https://raw.githubusercontent.com/AJACS-training/AJACSa2/master/morioka/images/AJACSa2_morioka_08.jpg)
---

![images/AJACSa2_morioka_09.jpg](https://raw.githubusercontent.com/AJACS-training/AJACSa2/master/morioka/images/AJACSa2_morioka_09.jpg)
---

![images/AJACSa2_morioka_10.jpg](https://raw.githubusercontent.com/AJACS-training/AJACSa2/master/morioka/images/AJACSa2_morioka_10.jpg)
---
