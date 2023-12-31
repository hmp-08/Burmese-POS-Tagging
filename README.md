# Burmese-POS-Tagging-Model
Burmese Part-of-Speech (POS) model Based on Transformer (Opennmt-tf framework)

# Introduction
Part-of-Speech (POS) refers to categorizing words in a sentence based on their grammatical roles. It helps in parsing, information retrieval, named entity recognition, sentiment analysis, machine translation, and language generation. POS tagging uses machine learning to assign tags to words. It improves language analysis and understanding in NLP tasks.

# Data
The data used in this project is sourced from Dr. Ye Kyaw Thu's GitHub repository. The data consists of annotated Burmese text in the form of word sequences accompanied by their corresponding Part-of-Speech (POS) tags. Maximum words per a sentence is cut to be 150 and left ~43k in data sentences. The data format follows a tokenized representation, where each word is separated by a space and annotated with its POS tag using the slash (/) delimiter. For example,  ဒီ/adj ဆေး/n က/ppm ၁၀၀/num ရာခိုင်နှုန်း/n ဆေးဘက်ဝင်/adj အပင်/n များ/part မှ/ppm ဖော်စပ်/v ထား/part တာ/part ဖြစ်/v တယ်/ppm ။/punc. The word "ဒီ" is tagged as "adj" (adjective), "ဆေး" as "n" (noun), "က" as "ppm" (preposition), "၁၀၀" as "num" (numeral), and so on.

# POS Tags
There are 15 POS tags. The definitions and descriptions of POS tags are presented in detail as follows:

1. abb as Abbreviation (E.g. အ.မ.က, အ.လ.က),
2. adj as Adjective (E.g. ၀သော, ပိန်သော),
3. adv as Adverb (E.g.နှေးနှေး),
4. conj as Conjunction (E.g. နှင့်, ဖြင့် ),
5. fw as Foreign Word (E.g. 1, 2, Facebook),
6. int as Interjection (E.g. အောင်မလေး),
7. n as Noun (E.g. ကား, နွား),
8. num as Number (E.g. ၁, ၂, ၃),
9. part as Particle (E.g. ပြီး, များ),
10. ppm as Post-positional Marker (E.g. သည်, က, ကို , သို့, မှာ, တွ င် ),
11. pron as Pronoun(E.g. ကျွန်မ, သင်, သူ , သူ မ),
12. punc as Punctuation(E.g. ။, ၊),
13. sb as Symbol(E.g. %, $),
14. tn as Text Number (E.g. တစ်, နှစ်)
15. v as Verb (E.g. လှုပ်ရှား )

# Data Preprocessing
I splitted the corpus into training, validation, and testing sets. The training set contains the majority of the data (90%), while the validation and testing sets includes 5% each maintaing a balanced representation of POS tags across sets.
I also generated a vocabulary file that includes all unique words from the training set. I utilized preprocessing and formatting scripts provided by OpenNMT and formatting the data into the desired input format for OpenNMT.

# Data Integration with OpenNMT
I integrated the formatted and preprocessed data into the OpenNMT training pipeline, configured the training settings, model architecture, hyperparameters, and data loading according to the OpenNMT documentation.
OpenNMT-TF is a powerful framework for training neural machine translation models, and it can also be used for other sequence-to-sequence tasks like part-of-speech (POS) tagging. 

# Install opennmt
```
pip install OpenNMT-tf[tensorflow]
```

# How to train 

## 1. Build Vocab
   
```
onmt-build-vocab --from_vocab sp-src.vocab --from_format sentencepiece --save_vocab sp_src_vocab.txt
onmt-build-vocab --from_vocab sp-tgt.vocab --from_format sentencepiece --save_vocab sp_tgt_vocab.txt
```

## 2. Train
```
onmt-main --model_type Transformer --config data.yml --auto_config train --with_eval
```

## 3. Inference

### With GPU 
```
onmt-main --config data.yml --auto_config infer --features_file src-test.txt > pred-test.txt
```
### Without GPU 
```
CUDA_VISIBLE_DEVICES="" onmt-main --config data.yml --auto_config infer --features_file src-test.txt > pred-test.txt
```

## Inference Using Ctranslate2

I have exported the trained Burmese Part-of-Speech (POS) model in the ctranslate2 format for easy and efficient inference. This allows you to utilize the model for POS tagging without the need for the entire training infrastructure.

### Prerequisites
Before using the exported model, ensure you have the ctranslate2 library installed. You can install it using the following command:

```
pip install ctranslate2
```

### Model Export Settings
The exported model was created with the following settings from the data.yml configuration file:

Batch Size: The number of sentences or sequences processed in each batch during export.

Scorers: The evaluation metric used to determine the best model for export.

Export on Best: The model was exported when the specified evaluation metric (BLEU score) improved.

Export Format: The model was exported in the ctranslate2 format for efficient inference.


```
import ctranslate2
import sentencepiece as spm

model_path = "/home/user/Documents/My_test/POS/dataset/run/export/6400/"

translator = ctranslate2.Translator(model_path, device="cpu")  # "cpu" or "cuda"
encode_sp = spm.SentencePieceProcessor('sp-src.model')
decode_sp = spm.SentencePieceProcessor('sp-tgt.model')

source_sent = "ဗိုလ်ချုပ် အောင်ဆန်း ၏ ဖက်ဆစ် ဆန့်ကျင် ရေး အဖွဲ့ တည်ထောင် ရန် အဆို ကို ဆွေးနွေး ကြ ပြီး အဖွဲ့ ၏ ကြေညာစာတမ်း နှင့် စုပေါင်း ဆောင်ရွက် ရ မည့် စီမံကိန်း တစ် ရပ် ကို သဘောတူ အတည်ပြု လိုက် ပါ သည် ။"
source_tokens = encode_sp.encode(source_sent, out_type=str)

results = translator.translate_batch([source_tokens])
target_tokens = results[0].hypotheses[0]
target_text = decode_sp.decode(target_tokens)
target_text

# output --> 'n n ppm n v part n v conj n ppm v part conj n ppm n ppm v v part part n tn part ppm v v part part ppm punc'
```
![](https://github.com/hmp-08/Burmese-POS-Tagging/blob/main/Screenshot%20from%202023-12-20%2011-42-32.png)

## 4. Evaluation using WER

I have provided a Jupyter Notebook, wer_calc.ipynb, that demonstrates the evaluation of the performance of the trained Burmese Part-of-Speech (POS) model using Word Error Rate (WER) calculation.

You can download my pretrained model here. https://drive.google.com/file/d/1YksaDMcCuwHlVQ_32LtDHGhui3Z5Ipf_/view?usp=drive_link

Opennmt-tf ---> https://opennmt.net/OpenNMT-tf/

Dataset ---> https://github.com/ye-kyaw-thu/myPOS/blob/master/corpus-ver-3.0/corpus/mypos-ver.3.0.txt
