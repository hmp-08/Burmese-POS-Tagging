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

# Inference with pre-built model
You can download my pre-built model here : https://drive.google.com/file/d/1aGGffFb0KbZPoRP8S2mscQAFrF0tu8aG/view?usp=sharing

### With GPU 
```
onmt-main --config data.yml --auto_config infer --features_file src-test.txt
```
### Without GPU 
```
CUDA_VISIBLE_DEVICES="" onmt-main --config data.yml --auto_config infer --features_file src-test.txt
```

Opennmt-tf ---> https://opennmt.net/OpenNMT-tf/

Dataset ---> https://github.com/ye-kyaw-thu/myPOS/blob/master/corpus-ver-3.0/corpus/mypos-ver.3.0.txt
