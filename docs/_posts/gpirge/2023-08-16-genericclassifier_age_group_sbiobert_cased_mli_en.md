---
layout: model
title: Age Group Classification
author: John Snow Labs
name: genericclassifier_age_group_sbiobert_cased_mli
date: 2023-08-16
tags: [clinical, licensed, en, text_classification, age, age_group]
task: Text Classification
language: en
edition: Healthcare NLP 5.0.1
spark_version: 3.0
supported: true
annotator: GenericClassifierModel
article_header:
  type: cover
use_language_switcher: "Python-Scala-Java"
---

## Description

This Generic Classifier model is trained for analyzing the age group of a person mentioned in health documents. Age of the person may or may not be mentioned explicitly in the training dataset.

The Text Classifier model has been trained using in-house annotated health-related text that have been labeled with three different classes:

`Adult`: A person who is fully grown or developed. Typically refers to someone who is 18 years or older,

`Child`: Requires intervention, urgent, not life-threatening cases.

`Unknown`: Not possible to comprehend/figure out the age group from the given text.

## Predicted Entities

`Adult`, `Child`, `Unknown`

{:.btn-box}
<button class="button button-orange" disabled>Live Demo</button>
<button class="button button-orange" disabled>Open in Colab</button>
[Download](https://s3.amazonaws.com/auxdata.johnsnowlabs.com/clinical/models/genericclassifier_age_group_sbiobert_cased_mli_en_5.0.1_3.0_1692194496033.zip){:.button.button-orange.button-orange-trans.arr.button-icon.hidden}
[Copy S3 URI](s3://auxdata.johnsnowlabs.com/clinical/models/genericclassifier_age_group_sbiobert_cased_mli_en_5.0.1_3.0_1692194496033.zip){:.button.button-orange.button-orange-trans.button-icon.button-copy-s3}

## How to use



<div class="tabs-box" markdown="1">
{% include programmingLanguageSelectScalaPythonNLU.html %}
```python
document_assembler = DocumentAssembler()\
    .setInputCol("text")\
    .setOutputCol("document")

sentence_embeddings = BertSentenceEmbeddings.pretrained("sbiobert_base_cased_mli", 'en','clinical/models')\
    .setInputCols(["document"])\
    .setOutputCol("sentence_embeddings")

features_asm = FeaturesAssembler()\
    .setInputCols(["sentence_embeddings"])\
    .setOutputCol("features")

generic_classifier = GenericClassifierModel.pretrained("genericclassifier_age_group_sbiobert_cased_mli", "en", "clinical/models")\
    .setInputCols(["features"])\
    .setOutputCol("prediction")

pipeline = Pipeline(stages=[
    document_assembler,
    sentence_embeddings,
    features_asm,
    generic_classifier
])

data = spark.createDataFrame([["""A patient presented with complaints of chest pain and shortness of breath. The medical history revealed the patient had a smoking habit for over 30 years, and was diagnosed with hypertension two years ago. After a detailed physical examination, the doctor found a noticeable wheeze on lung auscultation and prescribed a spirometry test, which showed irreversible airway obstruction. The patient was diagnosed with Chronic obstructive pulmonary disease (COPD) caused by smoking."""],
 ["""Hi, wondering if anyone has had a similar situation. My 1 year old daughter has the following; loose stools/ pale stools, elevated liver enzymes, low iron.  5 months and still no answers from drs. """],
 ["""Hi have chronic gastritis from 4 month(confirmed by endoscopy).I do not have acid reflux.Only dull ache above abdomen and left side of chest.I am on reberprozole and librax.My question is whether chronic gastritis is curable or is it a lifetime condition?I am loosing hope because this dull ache is not going away.Please please reply"""]]).toDF("text")

result = clf_Pipeline.fit(data).transform(data)
```
```scala
val document_assembler =new DocumentAssembler()
    .setInputCol("text")
    .setOutputCol("document")

val sentence_embeddings = new BertSentenceEmbeddings.pretrained("sbiobert_base_cased_mli", 'en','clinical/models')
    .setInputCols("document")
    .setOutputCol("sentence_embeddings")

val features_asm = new FeaturesAssembler()
    .setInputCols("sentence_embeddings")
    .setOutputCol("features")

val generic_classifier = new GenericClassifierModel.pretrained("genericclassifier_age_group_sbiobert_cased_mli", "en", "clinical/models")
    .setInputCols("features")
    .setOutputCol("prediction")

val clf_Pipeline = new Pipeline().setStages(Array(document_assembler, sentence_embeddings, features_asm, generic_classifier))

val data = Seq(Array("A patient presented with complaints of chest pain and shortness of breath. The medical history revealed the patient had a smoking habit for over 30 years, and was diagnosed with hypertension two years ago. After a detailed physical examination, the doctor found a noticeable wheeze on lung auscultation and prescribed a spirometry test, which showed irreversible airway obstruction. The patient was diagnosed with Chronic obstructive pulmonary disease (COPD) caused by smoking.", "Hi, wondering if anyone has had a similar situation. My 1 year old daughter has the following; loose stools/ pale stools, elevated liver enzymes, low iron.  5 months and still no answers from drs.", "Hi have chronic gastritis from 4 month(confirmed by endoscopy).I do not have acid reflux.Only dull ache above abdomen and left side of chest.I am on reberprozole and librax.My question is whether chronic gastritis is curable or is it a lifetime condition?I am loosing hope because this dull ache is not going away.Please please reply")).toDS().toDF("text")

val result = clf_Pipeline.fit(data).transform(data)
```
</div>

## Results

```bash
+------------------------------------------------------------------------------------------------------------------------------------------------------+---------+
|                                                                                                                                                  text|   result|
+------------------------------------------------------------------------------------------------------------------------------------------------------+---------+
|A patient presented with complaints of chest pain and shortness of breath. The medical history revealed the patient had a smoking habit for over 30...|  [Adult]|
|Hi, wondering if anyone has had a similar situation. My 1 year old daughter has the following; loose stools/ pale stools, elevated liver enzymes, l...|  [Child]|
|Hi have chronic gastritis from 4 month(confirmed by endoscopy).I do not have acid reflux.Only dull ache above abdomen and left side of chest.I am o...|[Unknown]|
+------------------------------------------------------------------------------------------------------------------------------------------------------+---------+
```

{:.model-param}
## Model Information

{:.table-model}
|---|---|
|Model Name:|genericclassifier_age_group_sbiobert_cased_mli|
|Compatibility:|Healthcare NLP 5.0.1+|
|License:|Licensed|
|Edition:|Official|
|Input Labels:|[features]|
|Output Labels:|[prediction]|
|Language:|en|
|Size:|3.4 MB|

## References

In-house annotated health-related text.

## Benchmarking

```bash
       label  precision    recall  f1-score   support
       Adult       0.83      0.76      0.79       358
       Child       0.93      0.85      0.89       203
     Unknown       0.77      0.86      0.81       383
    accuracy       -         -         0.82       944
   macro avg       0.84      0.83      0.83       944
weighted avg       0.83      0.82      0.82       944
```