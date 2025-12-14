---
layout: single
permalink: /metagenomes/respiratory
title: Respiratory Metagenomes
author_profile: false
---

# **Overview of the Metagenomic Dataset and Rationale for Controlled Access**

The respiratory metagenomics datasets comprises whole-shotgun metagenomes generated from sputum and bronchoalveolar lavage specimens collected from people with CF or bronchiectasis as part of our microbiome research program. These metagenomes provide comprehensive information on bacterial, fungal, and viral communities, as well as the antibiotic-resistance and functional gene content of the airway microbiome. The scientific value of these datasets lies in their ability to reveal organismal interactions, pathogen emergence, treatment responses, and changes in airway ecology over time.

As is common in clinical metagenomics, the raw sequencing reads may contain a small proportion of human genomic DNA originating from host cells present in the specimens. Although these reads are incidental to the research and are not used for human genomic analysis, they must be handled with care due to their potential identifiability. To minimise this risk, we apply established host-depletion protocols during both laboratory processing and computational analysis. Our bioinformatics pipeline removes human genomic sequences by aligning raw reads against the human reference genome and discarding all matches with appropriate stringency. Only the remaining non-human metagenomic content is used for downstream microbial and ecological analyses, or is provided to researchers through the controlled access [European Genome-phenome Archive](https://ega-archive.org/).

Despite these rigorous measures, it is technically impossible to guarantee perfect removal of every human-derived sequence. A small number of reads may evade filtering due to incomplete alignment, highly conserved regions, or technical artefacts. For this reason, and in line with national and international best practice for human-derived metagenomic datasets, we do not make the full dataset broadly or publicly accessible. Instead, metagenomes with the human sequences removed, to the best of our current ability, and associated metadata are provided only under **restricted, controlled access** and only to _bona fide_ researchers following review by a dedicated **Data Access Committee (DAC)**.

The DAC evaluates each request to ensure the proposed research is appropriate, secure, and compliant with participant consent and privacy protections. Approved users must agree not to analyse, interpret, or retain any human genomic material, and to store data within secure institutional environments. Importantly, they are required to notify the DAC if they identify any human sequences that were not removed during preprocessing, so that we can maintain a high standard of genomic privacy and refine our filtering approaches.

This controlled-access model enables us to maximise the scientific and clinical value of the respiratory metagenomes while upholding stringent ethical obligations. Our aim is to share the microbial and ecological information that is vital for advancing respiratory research while ensuring that any incidental human genomic material is protected, minimised, and handled with the highest level of ethical stewardship.


### Access to the data for scientists

{% for post in site.categories.metagenomes.respiratory limit:10 %}
  {% include archive-single.html type=entries_layout %}
{% endfor %}


