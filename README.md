# MAM, BERT & DAD: Multi-Aspect Modelling, Joint-BERT and Dual-Annotated Datasets for In-Game Toxicity Detection

## Sony Jufri

## University of Sydney's NLP Group

<b>Abstract:</b> In the past few years, the popularity of online gaming had grown rapidly. Unfortunately, with this phenomenon, came along the emergence of harmful behaviour in those communities. Many attempts had been done by gaming companies and Natural Language Processing (NLP) researchers to detect online abuse. These attempts ranged from the simple use of human reviewers to the more sophisticated machine learning and deep learning techniques. Despite these attempts, studies showed that online abuse remained a big issue for many people to this day. Therefore, it is important that we continue to improve our effort in detecting online abuse. 

Inspired by two of the most recent literatures in abusive language detection from the University of Sydney's NLP Group, we developed deep learning models that could better detect toxic behaviour in online gaming. Using dual annotated datasets from two well-known games, Defense of the Ancients 2 (Dota 2) and League of Legends (LOL), a variety of multi-apsect embeddings and Joint-BERT models were used to better detect toxic behaviour in online gaming. We hoped this would benefit not only online gamers, but also other stakeholders in the gaming industry.

## MAM (Multi-Aspect Modelling)

The multi-aspect embeddings in the modelling were developed using the method outlined in [Detect All Abuse! Toward Universal Abusive Language Detection Models](https://github.com/usydnlp/MACAS).

<p align="left">
  <img width="450" src="/static/MAE.png">
</p>

## BERT (the Joint-BERT)

The Joint-BERT models used the (Unofficial) Pytorch implementation of `JointBERT`: [BERT for Joint Intent Classification and Slot Filling](https://arxiv.org/abs/1902.10909) as the base.

### The Base Joint-BERT Model Architecture

<p float="left" align="left">
    <img width="600" src="/static/j_bert_conda.png" />  
</p>

- Predict `intent` and `slot` at the same time from **one BERT model** (=Joint model)
- total_loss = intent_loss + coef \* slot_loss

## DAD (Dual-Annotated Datasets)

|       | Train  | Dev   | Test  | Intent Labels | Slot Labels |
| ----- | ------ | ----- | ----- | ------------- | ----------- |
| CONDA | 26,078 | 8,705 | 0     | 4             | 6           |
| LOL   | 29,358 | 3,258 | 3,628 | 4             | 6           |

The CONDA and LOL datasets were developed from two well-known games, Defense of the Ancients 2 (Dota 2) and League of Legends (LOL), using the method outlined in [CONDA: a CONtextual Dual-Annotated dataset for in-game toxicity understanding and detection](https://arxiv.org/abs/2106.06213) and provided by the University of Sydney’s NLP Group.  

Below is an example of the Intent and Slot annotations.

<p align="left">
  <img width="550" src="/static/conda_annot2.png">
</p>

## Dependencies

- python==3.8.11
- torch==1.8.1
- transformers==3.0.2
- seqeval==0.0.12
- pytorch-crf==0.7.2
- torchvision==0.9.1
- flask==1.1.2
- numpy==1.20.3
- pillow==8.3.1
- gunicorn==20.1.0

## Training & Evaluation
$ python main.py --task {task_name} \
                 --model_type {model_type} \
                 --model_dir {model_dir_name} \
                 --do_train --do_eval \
                 --add_embed {d,g,e,dg,de} \
		 --optim_metric {JSA, UCA, U-F1(I), T-F1, T-F1(T), etc}

### Examples
python main.py --task conda --model_type bert --model_dir final_conda_bert_dg_model --do_train --add_embed dg --optim_metric JSA  

python main.py --task low --model_type bert --model_dir final_low_bert_dg_model --do_train --do_eval --add_embed dg --optim_metric JSA  

## Processing data
vocab_process.py

## Prediction
$ python predict.py --input_file {INPUT_FILE_PATH} --output_file {OUTPUT_FILE_PATH} --model_dir {SAVED_CKPT_PATH}

### Example
python predict.py --input_file sample_pred_in.txt --output_file sample_pred_out.txt --model_dir final_conda_bert_dg_model

## Model Demo
Heroku deployment: [MAM, BERT & DAD on Heroku](https://usydnlp-conda.herokuapp.com/)  

Gcloud deployment: [MAM, BERT & DAD on Google Cloud](https://usydnlp-conda-dtupb6paeq-ue.a.run.app/)

## References

- [Detect All Abuse! Toward Universal Abusive Language Detection Models](https://github.com/usydnlp/MACAS)
- [BERT for Joint Intent Classification and Slot Filling](https://arxiv.org/abs/1902.10909)
- [CONDA: a CONtextual Dual-Annotated dataset for in-game toxicity understanding and detection](https://arxiv.org/abs/2106.06213)
- [monologg/JointBERT](https://github.com/monologg/JointBERT)
- [avinassh/pytorch-flask-api-heroku](https://github.com/avinassh/pytorch-flask-api-heroku)
- [Huggingface Transformers](https://github.com/huggingface/transformers)
- [pytorch-crf](https://github.com/kmkurn/pytorch-crf)