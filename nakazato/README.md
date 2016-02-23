# AJACSa2

情報・システム研究機構（ROIS）
ライフサイエンス統合データベースセンター（DBCLS）
[仲里 猛留](http://data.dbcls.jp/~nakazato/)   nakazato@dbcls.rois.ac.jp
twitter: @chalkless

2016年2月25日・26日 AJACSa2

----

[AJACSa2三島](https://github.com/AJACS-training/AJACSa2) > 遺伝子発現解析・つづき

----

## やったこと（ここまでの復習）
- 解析したいNGSデータ（リード）の入手
- レファレンス配列データの入手
- NGSデータに対するクオリティチェック (FastQC)
- トリミング：アダプター配列などの除去
- レファレンス配列データへのNGSデータ（リード）のマッピング (tophat)

## やること（予告）
- 遺伝子発現量 (FPKM) の計算 (cufflinks)
- サンプル間での遺伝子発現量の比較 (cuffmerge, cuffdiff)
- 結果の解釈1：結果の可視化 (CummeRbund)
- 結果の解釈2：生物学的な解釈 (DAVID)

## 遺伝子発現解析(4)
トランスクリプトームアセンブリと発現差分解析 (cufflinks, cuffdiff)
UNIXコマンドによるファイル操作

### やること（原理）
- 遺伝子発現量(FPKM)の計算
- FPKM とは
	- Fragments Per Kilobase of exon per Million mapped fragments

### インストールの確認
p.98 を見られたし

### 遺伝子発現量(FPKM)の計算



途中経過

You are using Cufflinks v2.2.1, which is the most recent release.
[13:58:44] Loading reference annotation.
[13:58:53] Inspecting reads and determining fragment length distribution.
> Processing Locus 19:46978770-46978870        [****                     ]  19%


最終結果
nakazato@grouper:~/lecture/ajacsa2/data/expression$ cufflinks -p 8 -g NGS_DAT/Lv2_2/iGenome/Homo_sapiens/NCBI/build37.2/Annotation/Archives/archive-2014-06-02-13-47-29/Genes/genes.gtf tophat/ERR266335_P0/ERR266335_P0.bam -o tophat/ERR266335_P0/cufflinks_results
You are using Cufflinks v2.2.1, which is the most recent release.
[13:58:44] Loading reference annotation.
[13:58:53] Inspecting reads and determining fragment length distribution.
> Processed 307510 loci.                       [*************************] 100%
> Map Properties:
>	Normalized Map Mass: 58311525.14
>	Raw Map Mass: 58311525.14
>	Fragment Length Distribution: Truncated Gaussian (default)
>	              Default Mean: 200
>	           Default Std Dev: 80
[14:07:11] Assembling transcripts and estimating abundances.
> Processed 307510 loci.                       [*************************] 100%

できたものを確認

ファイルの中身


cuffmurge

nakazato@grouper:~/lecture/ajacsa2/data/expression$ cuffmerge -p 8 -o fpkm_compare -g NGS_DAT/Lv2_2/iGenome/Homo_sapiens/NCBI/build37.2/Annotation/Archives/archive-2014-06-02-13-47-29/Genes/genes.gtf -s NGS_DAT/Lv2_2/iGenome/Homo_sapiens/NCBI/build37.2/Sequence/Bowtie2Index/genome.fa transcripts.gtf.txt 

[Fri Feb 19 16:37:30 2016] Beginning transcriptome assembly merge
-------------------------------------------

[Fri Feb 19 16:37:30 2016] Preparing output location fpkm_compare/
[Fri Feb 19 16:37:45 2016] Converting GTF files to SAM
[16:37:45] Loading reference annotation.
[16:37:54] Loading reference annotation.
[16:38:04] Loading reference annotation.
[16:38:14] Loading reference annotation.
[16:38:25] Loading reference annotation.
[16:38:34] Loading reference annotation.
[Fri Feb 19 16:38:45 2016] Quantitating transcripts
You are using Cufflinks v2.2.1, which is the most recent release.
Command line:
cufflinks -o fpkm_compare/ -F 0.05 -g NGS_DAT/Lv2_2/iGenome/Homo_sapiens/NCBI/build37.2/Annotation/Archives/archive-2014-06-02-13-47-29/Genes/genes.gtf -q --overhang-tolerance 200 --library-type=transfrags -A 0.0 --min-frags-per-transfrag 0 --no-5-extend -p 8 fpkm_compare/tmp/mergeSam_tmp.6.mN3Hsh 
[bam_header_read] EOF marker is absent. The input is probably truncated.
[bam_header_read] invalid BAM binary header (this is not a BAM file).
File fpkm_compare/tmp/mergeSam_tmp.6.mN3Hsh doesn't appear to be a valid BAM file, trying SAM...
[16:38:46] Loading reference annotation.
[16:38:53] Inspecting reads and determining fragment length distribution.
Processed 48059 loci.                       
> Map Properties:
>       Normalized Map Mass: 468197.00
>       Raw Map Mass: 468197.00
>       Fragment Length Distribution: Truncated Gaussian (default)
>                     Default Mean: 200
>                  Default Std Dev: 80
[16:39:00] Assembling transcripts and estimating abundances.
Processed 48059 loci.                       
[Fri Feb 19 16:40:32 2016] Comparing against reference file NGS_DAT/Lv2_2/iGenome/Homo_sapiens/NCBI/build37.2/Annotation/Archives/archive-2014-06-02-13-47-29/Genes/genes.gtf


Warning: couldn't find fasta record for '1|NT_167207.1'!




nakazato@grouper:~/lecture/ajacsa2/data/expression$ cuffdiff -p 4 --upper-quartile-norm -o cuffdiff_result -L ERR266335_P0,ERR266349_P2,ERR266337_P5,ERR266351_P7,ERR266338_P10,ERR266247_Adult fpkm_compare/merged.gtf tophat/ERR266335_P0/ERR266335_P0.bam tophat/ERR266349_P2/ERR266349_P2.bam tophat/ERR266337_P5/ERR266337_P5.bam tophat/ERR266351_P7/ERR266351_P7.bam tophat/ERR266338_P10/ERR266338_P10.bam tophat/ERR266347_Adult/ERR266347_Adult.bam 
You are using Cufflinks v2.2.1, which is the most recent release.
[16:55:12] Loading reference annotation.
Warning: No conditions are replicated, switching to 'blind' dispersion method
[16:55:24] Inspecting maps and determining fragment length distributions.
Segmentation fault: 11

















## 遺伝子発現解析(5)
R/Bioconductorによるデータ解析 (cummeRbund 他)

## 遺伝子発現解析(6)
エンリッチメント解析 (DAVID)
