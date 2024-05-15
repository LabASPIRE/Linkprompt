# LinkPrompt
Implementation of paper "Linkprompt: Natural and Universal Adversarial Attacks on Prompt-based Language Models" on the main conference of NAACL 2024.

![image](misc/main_ill.png)

## Overview
We develop $\textit{LinkPrompt}$, an adversarial attack algorithm to generate universal adversarial triggers (UATs) by a gradient-based beam search algorithm that not only effectively attacks the target pre-trained language models (PLMs) and prompt-based fine-tuning models (PFMs) but also maintains the naturalness among the trigger tokens. 

We formulate a combined objective function that includes both an attack objective and a semantic objective. The objective $\mathcal{L}\_{adv}$ is designed to minimize the probability of the $[\texttt{mask}]$ token being correctly predicted by the PLM. The semantic objective $\mathcal{L}\_{sem}$ is to maximize the probability of the current candidate token given the previous tokens. The total loss objective is the weighted combination of the above two parts: $\mathcal{L}(\mathbf{t})= \mathcal{L}\_{adv}(\mathbf{t})+\alpha\mathcal{L}\_{sem}(\mathbf{t})$.

Extensive results demonstrate the effectiveness of $\textit{LinkPrompt}$, as well as the transferability of UATs generated by $\textit{LinkPrompt}$ to Llama2 and GPT-3.5-turbo.

## Setup
$\textit{LinkPrompt}$ is implemented on Python 3.9.18. We recommend running this program on a Linux platform. To install all dependencies, please get into this directory and run the following command:
```
git clone https://github.com/SavannahXu79/LinkPrompt.git
cd LinkPrompt
conda create -n linkprompt python=3.9
conda activate linkprompt
pip install -r requirements.txt
```

## Usage

### Search
`linkprompt.py` implements the trigger search on the RoBERTa-large model. 
```
python linkprompt.py  --alpha 0.05 --trigger_len 5
```
To control the effectiveness and naturalness of generated UATs, you can change `--alpha` balance two loss terms.
You can change `--trigger_len` to attain triggers of different lengths.

### Evaluation
`eval.py` can evaluate $\textit{LinkPrompt}$ on different types of Bert and Roberta.

```
python "./eval.py" --shots 16 --dataset ag_news --repeat 5 \
	--bert_type roberta-large --template_id 0 --load_trigger trigger/<trigger_json>
```
The arguments:
- `dataset`: The evaluation datasets. 
- `shots`: The number of samples per label.
- `bert_type`: The type of PLMs. 
- `template_id`: The chosen template. `0` stands for manual prompt and `1` stands for null prompt. Check `prompt/` folder for all prompt templates.


## Acknowledgement and citation
We thank the following open-source repositories.
```
[1] https://github.com/leix28/prompt-universal-vulnerability
```

If you find $\textit{LinkPrompt}$ useful in your research, please consider citing our [paper](http://arxiv.org/abs/2403.16432):
```
@article{xu2024textit,
  title={$$\backslash$textit $\{$LinkPrompt$\}$ $: Natural and Universal Adversarial Attacks on Prompt-based Language Models},
  author={Xu, Yue and Wang, Wenjie},
  journal={arXiv preprint arXiv:2403.16432},
  year={2024}
}
```
