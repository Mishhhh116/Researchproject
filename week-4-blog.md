# week 4 update

## what I have done

* Worked on the first reproduction notebook:
  * `01_toxicdetector_style_reproduction.ipynb`
* Went back to the ToxicDetector paper and code to understand the main idea.
* Checked how ToxicDetector uses toxic concept information and model representations for detection.
* Tried to follow the original ToxicDetector direction, but the Llama model access was rejected.
* Because I could not use the original Llama checkpoint, I changed this part into a ToxicDetector-style branch.
* Built a simplified and runnable version of the ToxicDetector idea.
* Used the same toxic content detection task and kept the main idea of using representation-based features.
* Ran the notebook and generated ToxicDetector-style outputs.
* Saved the outputs so they can be used later in the fusion experiment.
* Updated the project direction based on the access limitation.

## current progress

* The first branch of the project is now runnable.
* This branch is not a full original ToxicDetector reproduction because the required Llama access was not available.
* This ToxicDetector-style branch will be used as the main detector in my final method.
* The project direction:
  * ToxicDetector-style branch as the main detector
  * MetaTox-inspired branch as the auxiliary detector
  * confidence-based fusion as my own added component

## challenges

* The main problem this week was the rejected Llama access.
* Because of this, I could not directly run the original ToxicDetector model setting.
* I needed to adjust the reproduction into a smaller version that can run in Colab.
* I also need to make sure the output format is useful for the later fusion notebook.

## next steps

* Work on the second notebook:
  * `02_metatox_inspired_reproduction.ipynb`
* Check the MetaTox paper and code again.
* Try to keep the reasoning or semantic idea from MetaTox.
* Build a runnable MetaTox-inspired branch.
* Prepare both branch outputs for the final confidence-based fusion experiment.
