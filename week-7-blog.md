## Work completed this week

This week I continued working on Notebook 03. In Week 6, I had problems with GPU memory and computation time when trying to use Qwen3 for prompt-based classification. Running the model over the full validation and test sets was too slow, and the larger models were difficult to use with the available Colab GPU.

After discussing this problem with my supervisor in this week’s meeting, he suggested freezing the Qwen3 model and only training the MLP classifier. Based on this suggestion, I changed the implementation of Notebook 03.

Instead of asking Qwen3 to generate a prediction for every sample and every experimental setting, I now use Qwen3 as a frozen feature extractor. The parameters of Qwen3 are not updated during training. The model is only used to generate embeddings, while the smaller MLP classifier is trained for toxic and non-toxic classification.

The updated process is:

```text
Input text
        ↓
Frozen Qwen3 embedding model
        ↓
Text embedding
        ↓
Optional reasoning embedding and auxiliary scores
        ↓
Trainable MLP classifier
        ↓
Toxic / non-toxic prediction
```

This design reduces the GPU requirements because backpropagation is only performed through the MLP rather than the full Qwen3 model.

## Implementation progress

Based on the new design, I created and completed the initial 4B version:

```text
03_frozen_4B_mlp_ablation.ipynb
```

This version uses `Qwen/Qwen3-Embedding-4B` as the frozen embedding model. It loads the outputs from Notebook 01 and Notebook 02 and merges them using `sample_id`. I also checked the text and label alignment, and both validation and test samples were matched correctly.

The 4B model is used to encode two types of information:

* The original input text.
* The reasoning note produced in Notebook 02.

The text embedding and reasoning embedding both have 2,560 dimensions. These embeddings are saved after extraction, so they do not need to be calculated again when training different MLP settings.

I kept the eight ablation settings from the previous plan:

1. `text_only`
2. `text_td`
3. `text_mt`
4. `text_td_mt`
5. `text_reason`
6. `text_reason_td`
7. `text_reason_mt`
8. `full_model`

The non-reasoning settings use the text embedding with optional `td_score` and `mt_score`. The reasoning settings additionally include the reasoning embedding from Notebook 02.

For a fair comparison, all eight settings use the same small MLP structure. The MLP contains two hidden layers with 256 and 64 units. Only the MLP parameters are trained, while Qwen3 remains frozen.

I used 80% of the previous validation set to train the MLP and the remaining 20% as the development set. The original test set was only used for the final evaluation. The experiment was run on the full dataset rather than a small sample.

## Problems encountered

The main issue from Week 6 was that the original prompt-based method required too much GPU time and memory. Even after reducing the model size, generating predictions sample by sample was not practical for the full dataset.

Freezing Qwen3 solved most of this problem. Extracting embeddings with the 4B model was still relatively slow, especially because the embedding batch size had to be set to 2 on the Colab T4 GPU. However, this step only needed to be completed once. After the embeddings were saved to Google Drive, all eight MLP experiments could reuse them.

This made the experiment much more manageable. It also allowed me to train several feature settings without repeatedly running the full Qwen3 model.

## Preliminary results

The complete 4B experiment has now finished. The results from the eight settings were quite close.

The best preliminary result was:

```text
Setting: text_mt
F1: 0.8236
Precision: 0.7834
Recall: 0.8680
```

The basic `text_only` setting achieved an F1 score of approximately 0.8214. The best reasoning setting was `text_reason_td`, with an F1 score of approximately 0.8229.

At this stage, adding reasoning information has not shown a clear overall improvement compared with the strongest non-reasoning setting. However, the reasoning models corrected some individual samples that were incorrectly classified by the text-only model. Therefore, I still need to examine the fixed cases and error cases before making a final conclusion.

I also generated:

* F1, precision, recall, FPR and FNR comparison figures.
* Confusion matrices for the main settings.
* Results separated by HateXplain and Jigsaw.
* UMAP visualisations for text embeddings and text-plus-reasoning embeddings.
* Files containing cases fixed by reasoning and remaining classification errors.

These results are currently treated as preliminary because this is the first completed version of the frozen-model approach.

## Current status

At the end of this week, the initial 4B version of Notebook 03 has been completed and successfully run on the full dataset.

The current version now includes:

* Frozen Qwen3-Embedding-4B feature extraction.
* Text and reasoning embeddings.
* Auxiliary `td_score` and `mt_score` features.
* Eight MLP ablation settings.
* Full test metrics and source-level results.
* Confusion matrices and UMAP visualisations.
* Fixed-case and error-case analysis.

This confirms that the frozen Qwen3 plus MLP design can run successfully in the current Colab environment. It is also more practical than the previous prompt-based inference design.

## Next steps

Next week, I will use the same experiment design with `Qwen3-Embedding-8B`. The Qwen3 model will remain frozen, and only the MLP will be trained, so the comparison between the 4B and 8B versions will remain consistent.

After the 8B version is completed, I will compare:

* Whether the larger embedding model improves the overall F1 score.
* Whether reasoning features become more useful with stronger embeddings.
* Whether the 8B model reduces false positives or false negatives.
* Whether the fixed cases and UMAP distributions change compared with the 4B version.

The current 4B notebook will be kept as the initial completed version, while the 8B experiment will be used to check whether increasing the embedding model size provides a clearer improvement.
