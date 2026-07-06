# week 5 update

## what I have done

* Worked on the second reproduction notebook:
  * `02_metatox_inspired_reproduction.ipynb`
* Went back to the MetaTox paper and checked its main pipeline again.
* Focused on the parts related to reasoning, semantic judgement, and toxic knowledge.
* Tried to understand how MetaTox uses external toxic knowledge to help detection.
* Found that the original MetaTox knowledge graph could not be accessed or used directly.
* Because the original KG was not available, I changed this part into a MetaTox-inspired branch.
* Built a simplified and runnable version that keeps the reasoning / semantic auxiliary idea.
* Ran the notebook and generated MetaTox-inspired outputs.
* Saved the outputs so they can be used together with the ToxicDetector-style outputs.
* Started planning the next notebook for confidence-based fusion.

## current progress

* The second branch of the project is now runnable.
* This branch is not a full original MetaTox reproduction because the original KG was not available.
* The MetaTox-inspired branch will be used as a reasoning / semantic auxiliary detector.
* The current final project structure:
  * ToxicDetector-style branch as the main detector
  * MetaTox-inspired branch as the reasoning / semantic auxiliary detector
  * confidence-based fusion as my own added part
* The fusion idea:
  * if the ToxicDetector-style branch is confident, use its prediction
  * if it is uncertain, use the MetaTox-inspired branch as extra judgement

## challenges

* The main problem this week was that the original MetaTox KG could not be accessed.
* Because of this, I could not reproduce the full original MetaTox pipeline.
* The MetaTox-inspired branch is simpler than the original paper.
* The next challenge is to make the two branches work together in a fair way.
* The two branches should use the same dataset and output comparable scores or predictions.
* The fusion method needs to show whether the auxiliary branch helps when the main detector is uncertain.

## next steps

* Start the third notebook for the final method:
  * confidence-based fusion
* Load the outputs from:
  * `01_toxicdetector_style_reproduction.ipynb`
  * `02_metatox_inspired_reproduction.ipynb`
* Compare:
  * ToxicDetector-style only
  * MetaTox-inspired only
  * simple average fusion
  * confidence-based fusion
* Check whether confidence-based fusion improves the result.
* Look at error cases to see when the MetaTox-inspired branch helps.
* Prepare result tables and plots for the report.
