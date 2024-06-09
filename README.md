# Detection and Generation of Textual Adversarial Examples 

Here are a few general details, the detailed documentation is `Detect_AI_generated_text.doc`.

### 1. Dataset - Wiki

Dataset for training models to classify human written vs GPT/ChatGPT generated text. This dataset contains Wikipedia introductions and GPT (Curie) generated introductions for 150k topics.

Prompt used for generating text:

`200 word wikipedia style introduction on '{title}'
{starter_text}`

where title is the title for the wikipedia page, and starter_text is the first seven words of the wikipedia introduction.

10% of the dataset taken as <b>validation data</b>.


### 2. Attacker - BERT Paraphraser

BERT paraphraser model adapts the Bidirectional Encoder Representations from Transformers (BERT) architecture to generate paraphrases by fine-tuning it on a dataset of sentence pairs that are paraphrases of each other.

BERT is fine-tuned in a sequence-to-sequence (seq2seq) manner where the model learns to produce sentences similar in meaning but different in wording from the input. The process involves tokenizing input sentences, using BERT to generate contextual embeddings, and applying decoding techniques like beam search to create paraphrases. 

### 3. Detector - RoBERTa 

The OpenAI RoBERTa detector model is designed to identify text generated by language models like GPT-3. Built upon the robust RoBERTa (Robustly optimized BERT approach) architecture, which is a variant of the BERT (Bidirectional Encoder Representations from Transformers) model, it excels in understanding and processing natural language by leveraging extensive pre-training on diverse datasets. 

The detector works by analyzing input text and assessing various linguistic patterns, structures, and inconsistencies that are indicative of machine-generated content. During training, the model is exposed to both human-written and machine-generated texts, learning to distinguish between the two based on subtle differences. This results in a tool that can effectively flag AI-generated text by evaluating stylistic and content-related features, providing a reliable means of identifying non-human authorship.

### 4. Training

The GAN-like training enhances AI-text detection by employing adversarial learning involving 2 models: a paraphraser and a detector. Training begins with generating AI text from the target model and updating the paraphraser to create paraphrased versions aimed at evading detection. The detector is trained to distinguish between human text and AI text. The process involves an iterative adversarial loop where the paraphraser and detector continuously improve against each other. This iterative training enhances the detector's robustness, ensuring it remains effective against sophisticated paraphrasing techniques​. Proximal Policy Optimization (PPO) is used to enhance the paraphraser model's training: initially, the paraphraser creates alternate versions of text to evade detection by the detector, which then evaluates these variations and provides feedback in the form of rewards. PPO is leveraged to adjust the paraphraser's parameters based on these rewards, balancing between exploring new strategies and exploiting existing ones. This integration enables the paraphraser to progressively generate more sophisticated paraphrases, thus strengthening the overall system through iterative adversarial training.

### 5. Metrics

<b>Area Under the Receiver Operating Characteristic Curve (AUROC)</b>:

This metric quantifies the ability of a classification model to distinguish between classes.
It plots the true positive rate (sensitivity) against the false positive rate (1 - specificity) for different threshold values.
AUROC ranges from 0 to 1, where 1 indicates a perfect classifier, and 0.5 represents a random classifier.


<b>Accuracy</b>:

It measures the proportion of correctly predicted instances out of the total instances.
Accuracy is a straightforward metric but can be misleading in the case of imbalanced datasets where one class dominates the others.


<b>Precision</b>:

Precision is the ratio of correctly predicted positive observations to the total predicted positives.
It quantifies the accuracy of positive predictions, indicating how many of the predicted positive instances are actually positive.


<b>Recall (also known as Sensitivity)</b>:

Recall is the ratio of correctly predicted positive observations to all observations in the actual positive class.
It measures the ability of the model to capture all the positive instances, minimizing false negatives.


<b>F1 Score</b>:

The F1 score is the harmonic mean of precision and recall.
It provides a balance between precision and recall, particularly useful when the classes are imbalanced.
F1 score ranges from 0 to 1, with 1 being the best score, indicating perfect precision and recall.