## Work completed this week

This week I mainly worked on Notebook 03. After finishing the two reproduction-style branches in Notebook 01 and Notebook 02, I started to design the final comparison experiment for the project.

The main idea of Notebook 03 is to compare reasoning and non-reasoning toxic content detection in a more direct way. Instead of building another classifier by myself, I decided to use an existing Qwen3 model for both settings. This makes the comparison cleaner because the base model is the same, and the main difference is whether reasoning is enabled or not.

The current design is:

- **Non-reasoning setting:** use Qwen3 with thinking mode disabled.
- **Reasoning setting:** use Qwen3 with thinking mode enabled.
- **Auxiliary information:** use `td_score` from Notebook 01 and `mt_score` from Notebook 02 as optional extra information.

So the four main settings in Notebook 03 are:

1. `nr_text`: Qwen3 non-reasoning judgement using text only.
2. `r_text`: Qwen3 reasoning judgement using text only.
3. `nr_text_td_mt`: Qwen3 non-reasoning judgement using text + `td_score` + `mt_score`.
4. `r_text_td_mt`: Qwen3 reasoning judgement using text + `td_score` + `mt_score`.

This design allows me to check two things. First, whether reasoning mode helps compared with direct judgement. Second, whether the outputs from Notebook 01 and Notebook 02 can provide useful auxiliary information.

## Implementation progress

I rewrote Notebook 03 based on this design. The notebook now loads the validation and test outputs from Notebook 01 and Notebook 02, merges them by `sample_id`, and checks that the samples are aligned. For the moment, I used the old Notebook 01 output based on Qwen3-Embedding-0.6B, because the newer Qwen3-Embedding-8B version was still running and had not finished yet.

I also wrote the prompt-based inference part for Qwen3. The prompt asks the model to classify each comment as toxic or non-toxic and return a simple JSON output with a label and toxicity score. For the reasoning setting, the model is allowed to use thinking mode, while for the non-reasoning setting, thinking mode is disabled and the model is asked to make a direct judgement only.

## Problems encountered

The main problem this week was computational cost. At first, I tried to use Qwen3-8B for Notebook 03, because it is a stronger model and should be more suitable for reasoning-based toxic content detection. However, it was too slow on the free Colab GPU. The inference speed was around several seconds per sample, and the full test set had more than 25,000 samples. One setting alone was estimated to take more than one day, and it still did not finish after using up the daily free GPU quota.

After that, I tried to reduce the model size to Qwen3-4B. This was better in theory, but in practice it was still too slow for full-data inference on free Colab. The speed was still not realistic if I wanted to run all four main settings.

Because of this, I finally changed the model to **Qwen3-1.7B**. This model is smaller and more practical for Colab, but it still belongs to the Qwen3 family and supports the same thinking / non-thinking mode comparison. This means the experiment logic can stay the same, while the computation becomes more manageable.

Another issue was that Colab may disconnect during long runs. To avoid losing progress, I modified the code to support checkpoint-style running. The notebook now saves partial results every 20 samples. If Colab disconnects or the GPU quota is used up, I can restart the notebook and continue from the saved `sample_id` instead of starting from the beginning again.

## Current status

At the end of this week, Notebook 03 has been redesigned into a more practical version:

- Qwen3-1.7B is used for the main prompt-based experiment.
- The experiment keeps the full dataset instead of only using a small sample.
- Only the four core settings are run first.
- The code supports partial saving and resume.
- The outputs from Notebook 01 and Notebook 02 are used as auxiliary scores.

This makes the experiment more realistic to run in Colab while still keeping the main research question clear.

## Next steps

Next week, I will continue running the four main settings in Notebook 03. I will first finish `nr_text` and `r_text`, because these two are the most important comparison between non-reasoning and reasoning. After that, I will run `nr_text_td_mt` and `r_text_td_mt` to check whether adding `td_score` and `mt_score` improves the result.

After the inference outputs are ready, I will calculate accuracy, precision, recall, F1, FPR, and FNR. I will also generate confusion matrices and compare fixed cases, especially the cases where the non-reasoning setting is wrong but the reasoning setting is correct.
