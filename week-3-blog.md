# week 3 update

## what I have done
- Reorganized the project direction for the remaining weeks.
- Rebuilt the demo code for my proposed method.
- The current demo tests whether the relationship between text representation and LLM-generated reasoning representation can help toxic content detection.
- Used RoBERTa to extract:
  - text representation
  - reasoning representation
- Built simple alignment features:
  - cosine similarity
  - absolute difference
  - element-wise product
- Added alignment signal analysis before the ablation experiment.
- Ran the first ablation experiment with:
  - text-only
  - reasoning-only
  - text + reasoning concatenation
  - full alignment
  - variants removing individual alignment features
- Generated result tables and plots for analysis.
- Checked error cases, including cases where text-only was wrong but full alignment was correct.

## current results
- The current result suggests that LLM-generated reasoning may provide useful additional signal compared with text-only.
- The full alignment setting performs better than text-only in the current run.
- However, full alignment is not clearly stronger than simple text + reasoning concatenation yet.
- This suggests that the current simple alignment features may still be limited.
- The current experiment is useful as a first feasibility check, but it is not strong enough as a final conclusion.

## challenges
- The current experiment is still based on a small controlled sample.
- The result needs to be tested with a larger sample size.
- The experiment also needs multiple random seeds to check stability.
- My previous ToxicDetector and MetaTox code was based on simplified adaptations.
- These simplified versions should not be treated as formal core paper reproduction.
- True original-code reproduction may require extra resources:
  - original Llama model checkpoint for ToxicDetector
  - original datasets for MetaTox
  -LLM setup for MetaTox
  - meta-toxic knowledge graph for MetaTox
- The remaining timeline is tight because I also need to complete other assessments.
- I am still not fully sure whether the current alignment signal analysis is enough to validate the idea of alignment.
- The current variables, such as cosine similarity, absolute difference, and element-wise product, are simple relationship features between text and reasoning representations.
- These features may show some interaction between h_text and h_reason, but they may not fully prove that the reasoning is truly aligned with the original toxic cues.
- I need to discuss whether these variables are suitable for validating alignment, or whether I should add other analysis such as span-level evidence or more detailed error case analysis.

## points to discuss in the meeting
- Confirm whether the final baseline comparison must include full original-code reproduction of ToxicDetector and MetaTox.
- Discuss whether simpler reproducible baselines on the same dataset are acceptable for the main comparison.
- Discuss whether ToxicDetector and MetaTox can be documented as original-code reproduction attempts if full reproduction is limited by model, data, or KG access.
- Discuss whether the current variables used for alignment validation are suitable:
  - cosine similarity
  - absolute difference
  - element-wise product
- Ask whether these features are enough to support the claim of alignment, or whether I should describe them more carefully as simple interaction features.
- Discuss whether I should add more evidence.

## next steps
- Finalize the feasible project scope for the remaining six weeks.
- Run experiments on a larger sample size.
- Add multiple random seeds.
- Finalize baseline comparison:
  - TF-IDF + Logistic Regression
  - RoBERTa text-only
  - reasoning-only
  - text + reasoning concatenation
  - full alignment
- Continue ablation experiments.
- Expand error case analysis.

# after week3 meeting
## discussion 
- Presented the current alignment-based experiments and preliminary results.
- Discussed whether cosine similarity, absolute difference, and element-wise product are sufficient to represent alignment.
- Supervisor pointed out that these features only describe the relationship between text and reasoning representations and do not directly prove semantic alignment.
- The current results do not show a clear improvement over simple text + reasoning concatenation, so alignment alone is not strong enough as the main contribution.
- Discussed the need to understand how alignment features are combined with the original text and reasoning features in the final classifier.
- Discussed the importance of comparing with strong and recent baseline methods instead of relying only on ablation studies.
- Supervisor suggested using ToxicDetector as the main baseline and studying its original implementation before adding new components.
- Supervisor also suggested reviewing MetaTox carefully to identify useful reasoning or knowledge components that could be incorporated.
- Discussed building on top of existing methods rather than proposing a completely independent method.
- Confirmed that extending previous work with a new component is acceptable and is not considered copying.
- Discussed using the same dataset for all compared methods to ensure a fair evaluation.
- Supervisor suggested increasing the dataset size if computationally feasible and testing whether the current observations remain consistent.

## updated challenges
- The current alignment features provide only weak evidence for semantic alignment.
- The current improvement is limited, so stronger experimental evidence is needed.
- Original reproduction of ToxicDetector and MetaTox still requires additional setup and understanding of the original implementations.

## updated next steps
- Continue alignment experiments using a larger dataset as an exploratory study.
- Study the original ToxicDetector implementation and reproduce the main pipeline.
- Study the MetaTox pipeline and identify useful reasoning or knowledge components.
- Design a simple method by building on top of ToxicDetector and incorporating useful ideas from MetaTox.
- Compare the proposed method with ToxicDetector, MetaTox, and other baseline methods on the same dataset.
- Start organizing the project around the final direction for the remaining weeks.

