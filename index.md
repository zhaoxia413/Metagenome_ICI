---
layout: default
title: Home
nav_order: 1
description: "Just the Docs is a responsive Jekyll theme with built-in search that is easily customizable and hosted on GitHub Pages."
permalink: /
---

# Focus on writing good documentation
{: .fs-9 }

Just the Docs gives your documentation a jumpstart with a responsive Jekyll theme that is easily customizable and hosted on GitHub Pages.
{: .fs-6 .fw-300 }

[Get started now](#getting-started){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 } [View it on GitHub](https://github.com/pmarsceill/just-the-docs){: .btn .fs-5 .mb-4 .mb-md-0 }

---

## Getting started

### Dependencies

Just the Docs is built for [Jekyll](https://jekyllrb.com), a static site generator. View the [quick start guide](https://jekyllrb.com/docs/) for more information. Just the Docs requires no special plugins and can run on GitHub Pages' standard Jekyll compiler. The [Jekyll SEO Tag plugin](https://github.com/jekyll/jekyll-seo-tag) is included by default (no need to run any special installation) to inject SEO and open graph metadata on docs pages. For information on how to configure SEO and open graph metadata visit the [Jekyll SEO Tag usage guide](https://jekyll.github.io/jekyll-seo-tag/usage/).

### Quick start: Use as a GitHub Pages remote theme

1. Add Just the Docs to your Jekyll site's `_config.yml` as a [remote theme](https://blog.github.com/2017-11-29-use-any-theme-with-github-pages/)
```zsh
parallel -j 35 --xapply \
$'kneaddata -i {1} -i {2} \
-o kneaddata_res/{3} -v -t 20 --fastqc FastQC --remove-intermediate-output \
--bowtie2-options "--very-sensitive --dovetail" \
--reference-db /data3/zhaoxia/metagenomeTools/kneaddata_db/bowtie2_contam_hg37_db' \
::: /data2/ZhouZhiyuan/RJNA678737/metagenome/*_1.fastq.gz ::: /data2/ZhouZhiyuan/RJNA678737/metagenome/*_2.fastq.gz :::: samplelist
```
<small>You must have GitHub Pages enabled on your repo, one or more Markdown files, and a `_config.yml` file. [See an example repository](https://github.com/pmarsceill/jtd-remote)</small>

### Local installation: Use the gem-based theme

1. Install the Ruby Gem
```bash
$ for i in $(cat samplelist )
do
kneaddata -i /data2/ZhouZhiyuan/RJNA678737/metagenome/"$i"_1.fastq.gz -i /data2/ZhouZhiyuan/RJNA678737/metagenome/"$i"_2.fastq.gz --reference-db /data3/zhaoxia/metagenomeTools/kneaddata_db/bowtie2_contam_hg37_db -v -t 20 --fastqc FastQC --remove-intermediate-output --bowtie2-options "--very-sensitive --dovetail" -o ./kneaddata_res/"$i"
done
```

```bash
$ mkdir kraken_braken_merge_tables
python2 /usr/local/bin/combine_bracken_outputs.py --files ./bracken_res/*P.braken  -o ./kraken_braken_merge_tables/combine_phylum.txt
python2 /usr/local/bin/combine_bracken_outputs.py --files ./bracken_res/*C.braken  -o ./kraken_braken_merge_tables/combine_Class.txt
python2 /usr/local/bin/combine_bracken_outputs.py --files ./bracken_res/*O.braken  -o ./kraken_braken_merge_tables/combine_Order.txt
python2 /usr/local/bin/combine_bracken_outputs.py --files ./bracken_res/*G.braken  -o ./kraken_braken_merge_tables/combine_Genus.txt
python2 /usr/local/bin/combine_bracken_outputs.py --files ./bracken_res/*F.braken  -o ./kraken_braken_merge_tables/combine_Family.txt
python2 /usr/local/bin/combine_bracken_outputs.py --files ./bracken_res/*S.braken  -o ./kraken_braken_merge_tables/combine_Spices.txt
```

```zsh
# _Optional:
for i in $(cat samplelist)
 do
kraken2 --db /data3/zhaoxia/metagenomeTools/Kraken_database/krakenV1_db/ --threads 56  --report ./kraken_res/"$i".report --output ./kraken_res/"$i".output  ./clean_data/"$i"_kneaddata.fastq
bracken -d /data3/zhaoxia/metagenomeTools/Kraken_database/krakenV1_db/ -i ./kraken_res/"$i".report -o ./bracken_res/"$i".P.braken -w ./bracken_res/"$i".P.braken.report -r 150 -l P
bracken -d /data3/zhaoxia/metagenomeTools/Kraken_database/krakenV1_db/ -i ./kraken_res/"$i".report -o ./bracken_res/"$i".S.braken -w ./bracken_res/"$i".S.braken.report -r 150 -l S 
bracken -d /data3/zhaoxia/metagenomeTools/Kraken_database/krakenV1_db/ -i ./kraken_res/"$i".report -o ./bracken_res/"$i".F.braken -w ./bracken_res/"$i".F.braken.report -r 150 -l F
bracken -d /data3/zhaoxia/metagenomeTools/Kraken_database/krakenV1_db/ -i ./kraken_res/"$i".report -o ./bracken_res/"$i".G.braken -w ./bracken_res/"$i".G.braken.report -r 150 -l G
bracken -d /data3/zhaoxia/metagenomeTools/Kraken_database/krakenV1_db/ -i ./kraken_res/"$i".report -o ./bracken_res/"$i".C.braken -w ./bracken_res/"$i".C.braken.report -r 150 -l C
bracken -d /data3/zhaoxia/metagenomeTools/Kraken_database/krakenV1_db/ -i ./kraken_res/"$i".report -o ./bracken_res/"$i".O.braken -w ./bracken_res/"$i".O.braken.report -r 150 -l O

done
```
2. Add Just the Docs to your Jekyll site’s `_config.yml`
```zsh
$ parallel -j 42 --xapply \
'megahit -1 {1} -2 {2} \
--out-prefix {3} \
-o Assembly_megahit_res/{3}' \
::: kneaddata_clean/*_1.fastq ::: kneaddata_clean/*_2.fastq :::: samplelist
```
3. _Optional:_ Initialize search data (creates `search-data.json`)
```zsh
$ cat kneaddata_clean/*_1.fastq > merge_assembly_reads/ALL_READS_1.fastq
  cat kneaddata_clean/*_2.fastq > merge_assembly_reads/ALL_READS_2.fastq
  metawrap assembly -1 merge_assembly_reads/ALL_READS_1.fastq -2 merge_assembly_reads/ALL_READS_2.fastq -m 24 -t 8 \
  --metaspades -o merged_megahit_assembly
```

```zsh
$ conda activate py3
/data3/zhaoxia/metagenomeTools/quast/quast.py ./merged_megahit_assembly/assembly.contigs.fa -o Assembly_quast_evaluation/megahit-report
conda deactivate
```

3. Run you local Jekyll servers
```zsh
$ for i in $(cat samplelist)
do
bbrename.sh in=kneaddata_clean/"$i"_1_kneaddata_paired_1.fastq \
in2=kneaddata_clean/"$i"_1_kneaddata_paired_2.fastq \
out=bbrename/"$i"_rename_1.fastq out2=bbrename/"$i"_rename_2.fastq 
done
```
```bash
# .. or if you're using a Gemfile (bundler)
$ conda activate py3
metawrap binning -o metawrap_initial_bining -t 96 \
-a merged_megahit_assembly/assembly.contigs.fa --metabat2 --maxbin2 --concoct bbrename/SRR*fastq
conda deactivate
```
4. Point your web browser to [http://localhost:4000](http://localhost:4000)

If you're hosting your site on GitHub Pages, [set up GitHub Pages and Jekyll locally](https://help.github.com/en/articles/setting-up-your-github-pages-site-locally-with-jekyll) so that you can more easily work in your development environment.

### Configure Just the Docs

- [See configuration options]({{ site.baseurl }}{% link docs/configuration.md %})

---

## About the project

Just the Docs is &copy; 2017-{{ "now" | date: "%Y" }} by [Patrick Marsceill](http://patrickmarsceill.com).

### License

Just the Docs is distributed by an [MIT license](https://github.com/pmarsceill/just-the-docs/tree/master/LICENSE.txt).

### Contributing

When contributing to this repository, please first discuss the change you wish to make via issue,
email, or any other method with the owners of this repository before making a change. Read more about becoming a contributor in [our GitHub repo](https://github.com/pmarsceill/just-the-docs#contributing).

#### Thank you to the contributors of Just the Docs!

<ul class="list-style-none">
{% for contributor in site.github.contributors %}
  <li class="d-inline-block mr-1">
     <a href="{{ contributor.html_url }}"><img src="{{ contributor.avatar_url }}" width="32" height="32" alt="{{ contributor.login }}"/></a>
  </li>
{% endfor %}
</ul>

### Code of Conduct

Just the Docs is committed to fostering a welcoming community.

[View our Code of Conduct](https://github.com/pmarsceill/just-the-docs/tree/master/CODE_OF_CONDUCT.md) on our GitHub repository.
