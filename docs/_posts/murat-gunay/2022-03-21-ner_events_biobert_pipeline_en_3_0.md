---
layout: model
title: Pipeline to Detect clinical events (biobert)
author: John Snow Labs
name: ner_events_biobert_pipeline
date: 2022-03-21
tags: [licensed, ner, clinical, en]
task: Named Entity Recognition
language: en
nav_key: models
edition: Healthcare NLP 3.4.1
spark_version: 3.0
supported: true
annotator: PipelineModel
article_header:
  type: cover
use_language_switcher: "Python-Scala-Java"
---

## Description

This pretrained pipeline is built on the top of [ner_events_biobert](https://nlp.johnsnowlabs.com/2021/04/01/ner_events_biobert_en.html) model.

{:.btn-box}
<button class="button button-orange" disabled>Live Demo</button>
<button class="button button-orange" disabled>Open in Colab</button>
[Download](https://s3.amazonaws.com/auxdata.johnsnowlabs.com/clinical/models/ner_events_biobert_pipeline_en_3.4.1_3.0_1647873577802.zip){:.button.button-orange.button-orange-trans.arr.button-icon.hidden}
[Copy S3 URI](s3://auxdata.johnsnowlabs.com/clinical/models/ner_events_biobert_pipeline_en_3.4.1_3.0_1647873577802.zip){:.button.button-orange.button-orange-trans.button-icon.button-copy-s3}

## How to use



<div class="tabs-box" markdown="1">
{% include programmingLanguageSelectScalaPythonNLU.html %}
```python
pipeline = PretrainedPipeline("ner_events_biobert_pipeline", "en", "clinical/models")

pipeline.annotate("The patient presented to the emergency room last evening")
```
```scala
val pipeline = new PretrainedPipeline("ner_events_biobert_pipeline", "en", "clinical/models")

pipeline.annotate("The patient presented to the emergency room last evening")
```


{:.nlu-block}
```python
import nlu
nlu.load("en.med_ner.biobert_events.pipeline").predict("""The patient presented to the emergency room last evening""")
```

</div>

## Results

```bash
+------------------+-------------+
|chunks            |entities     |
+------------------+-------------+
|presented         |OCCURRENCE   |
|the emergency room|CLINICAL_DEPT|
+------------------+-------------+
```

{:.model-param}
## Model Information

{:.table-model}
|---|---|
|Model Name:|ner_events_biobert_pipeline|
|Type:|pipeline|
|Compatibility:|Healthcare NLP 3.4.1+|
|License:|Licensed|
|Edition:|Official|
|Language:|en|
|Size:|422.0 MB|

## Included Models

- DocumentAssembler
- SentenceDetectorDLModel
- TokenizerModel
- BertEmbeddings
- MedicalNerModel
- NerConverter
