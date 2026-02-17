# Awesome AI Evaluation Metrics [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated cross-modality taxonomy of AI evaluation metrics — predictive, generative, computer vision, and agentic — with structured data and critical analysis of the benchmark landscape.

AI evaluation is fragmented across modalities. Predictive AI uses AUROC and calibration. Generative AI uses BERTScore and LLM-as-judge. Computer vision uses Dice and IoU. Agentic AI uses task completion and safety scores. This list provides a unified taxonomy, highlights cross-cutting themes (fairness, abstention, calibration), and critically examines why most benchmarks are broken.

**RAIGH Academy Module:** This list serves as a reference resource for the AI Fundamentals & Metrics module within the [RAIGH Academy](https://raigh.org) workforce development program.

## Contents

- [Predictive AI Metrics](#predictive-ai-metrics)
- [Generative AI / LLM Metrics](#generative-ai--llm-metrics)
- [Computer Vision Metrics](#computer-vision-metrics)
- [Agentic AI Metrics & Autonomy Levels](#agentic-ai-metrics--autonomy-levels)
- [The Benchmark Problem](#the-benchmark-problem)
- [Cross-Modality Themes](#cross-modality-themes)
- [Existing Evaluation Repos](#existing-evaluation-repos)
- [Structured Data](#structured-data)
- [Training & Certification](#training--certification)
- [Methodology](#methodology)
- [Contribute & Publish](#contribute--publish)
- [Sister Repositories](#sister-repositories)

---

## Predictive AI Metrics

### Discrimination

How well does the model separate positive from negative cases?

| Metric | Definition | When to Use | Limitations |
|---|---|---|---|
| **AUROC** | Area under ROC curve | Binary classification, comparing models | Insensitive to calibration, can be misleading with class imbalance |
| **AUPRC** | Area under precision-recall curve | Imbalanced datasets (prevalence <5%) | Threshold-dependent interpretation |
| **F1 Score** | Harmonic mean of precision and recall | Single-threshold evaluation | Assumes equal cost of FP and FN |
| **Sensitivity** | True positive rate | When missing positives is costly (screening) | Must pair with specificity or PPV |
| **Specificity** | True negative rate | When false positives are costly (confirmatory) | Must pair with sensitivity or NPV |
| **PPV / NPV** | Positive/Negative predictive value | Clinical decision-making (depends on prevalence) | Changes with prevalence — not intrinsic to model |
| **MCC** | Matthews Correlation Coefficient | Balanced metric for binary classification | Less intuitive than F1 |
| **Cohen's Kappa** | Agreement beyond chance | Inter-rater agreement, AI vs clinician | Affected by prevalence and bias |

**Key resources:**
- [Steyerberg (2019)](https://doi.org/10.1007/978-3-030-16399-0) — *Clinical Prediction Models* — definitive textbook
- [Saito & Rehmsmeier (2015)](https://doi.org/10.1371/journal.pone.0118432) — AUPRC vs AUROC for imbalanced data
- [Chicco & Jurman (2020)](https://doi.org/10.1186/s12864-019-6413-7) — MCC advantages over F1 and accuracy

### Calibration

Do predicted probabilities match observed frequencies?

| Metric | Definition | When to Use | Limitations |
|---|---|---|---|
| **Calibration Slope** | Slope of observed vs predicted | Primary calibration measure | Requires sufficient sample size |
| **Calibration-in-the-Large** | Intercept of calibration model | Detect systematic over/under-prediction | Single number, limited detail |
| **ECE** | Expected Calibration Error | Binned calibration assessment | Sensitive to binning strategy |
| **Brier Score** | Mean squared error of probabilities | Combined discrimination + calibration | Hard to decompose clinically |
| **Hosmer-Lemeshow** | Chi-squared goodness-of-fit | Traditional calibration test | Low power, depends on binning |

**Key resources:**
- [Van Calster et al. (2019)](https://doi.org/10.1016/j.jclinepi.2019.02.004) — "Calibration: the Achilles heel of predictive analytics"
- [Niculescu-Mizil & Caruana (2005)](https://doi.org/10.1145/1102351.1102430) — Predicting good probabilities with supervised learning

### Clinical Utility

Is the model useful for clinical decisions?

| Metric | Definition | When to Use | Limitations |
|---|---|---|---|
| **Net Benefit (DCA)** | Expected benefit minus expected harm | Clinical decision-making at specific threshold | Requires choosing threshold range |
| **NRI** | Net Reclassification Index | Comparing two models | Can be misleading without calibration |
| **IDI** | Integrated Discrimination Improvement | Comparing models (threshold-free) | Less intuitive than DCA |

**Key resources:**
- [Vickers & Elkin (2006)](https://doi.org/10.1177/0272989X06295361) — Decision curve analysis
- [Vickers et al. (2016)](https://doi.org/10.1186/s12916-016-0577-y) — "Net benefit approaches to the evaluation of prediction models"

### Fairness

Does the model perform equitably across demographic groups?

| Metric | Definition | Framework |
|---|---|---|
| **Demographic Parity** | Equal positive rate across groups | Group fairness |
| **Equalized Odds** | Equal TPR and FPR across groups | Group fairness |
| **Calibration by Group** | Equal calibration across groups | Individual fairness |
| **Predictive Parity** | Equal PPV across groups | Group fairness |
| **Counterfactual Fairness** | Same prediction if protected attribute changed | Causal fairness |

62+ fairness metrics have been identified in the literature.

**Key resources:**
- [IBM AIF360](https://aif360.mybluemix.net/) — Open-source fairness toolkit with 70+ metrics
- [Obermeyer et al. (2019)](https://doi.org/10.1126/science.aax2342) — "Dissecting racial bias in an algorithm used to manage the health of populations"
- [Mehrabi et al. (2021)](https://doi.org/10.1145/3457607) — Survey on bias and fairness in machine learning

---

## Generative AI / LLM Metrics

### Traditional Text Metrics

| Metric | Definition | When to Use | Limitations |
|---|---|---|---|
| **BLEU** | n-gram precision | Machine translation | Poor correlation with human judgment for open-ended text |
| **ROUGE** | n-gram recall | Summarization | Rewards extractive over abstractive |
| **BERTScore** | Contextual embedding similarity | General text evaluation | Depends on embedding model choice |
| **METEOR** | Alignment-based with stemming + synonyms | Better than BLEU for paraphrase | Still surface-level |
| **Perplexity** | Cross-entropy of model predictions | Language modeling | Not a quality metric — a model metric |

### Modern LLM Evaluation

| Metric / Method | Definition | When to Use | Limitations |
|---|---|---|---|
| **LLM-as-Judge** | Use a strong LLM to rate responses | Open-ended generation quality | Self-serving bias, position bias, verbosity bias |
| **G-Eval** | GPT-4 evaluation with chain-of-thought | Nuanced quality assessment | Expensive, non-deterministic |
| **Hallucination Detection** | Faithfulness to source material | Factual accuracy (RAG, summarization) | Hard to define ground truth at scale |
| **Toxicity Scoring** | Proportion of toxic/harmful outputs | Safety assessment | Threshold-dependent, cultural variation |
| **Sycophancy Rate** | Rate of agreement with user regardless of truth | Alignment evaluation | Requires adversarial prompts to detect |

### Medical AI Benchmarks (LLM)

| Benchmark | Focus | Limitations |
|---|---|---|
| **MedQA (USMLE)** | Medical knowledge (MCQ) | Multiple-choice, saturated (>90% by top models) |
| **PubMedQA** | Biomedical literature comprehension | Binary yes/no/maybe, limited clinical reasoning |
| **MedMCQA** | Medical MCQ from Indian exams | Domain-specific, not patient-facing |
| **MedXpertQA** | Expert-level medical reasoning | Newer, less established |
| **MMLU (Medical)** | Multitask medical knowledge | MCQ format, knowledge recall not clinical reasoning |

**Key resources:**
- [Zheng et al. (2023)](https://doi.org/10.48550/arXiv.2306.05685) — "Judging LLM-as-a-Judge" — systematic analysis of LLM evaluation
- [Ji et al. (2023)](https://doi.org/10.1145/3571730) — "Survey of Hallucination in Natural Language Generation"
- [Singhal et al. (2023)](https://doi.org/10.1038/s41586-023-06291-2) — "Large language models encode clinical knowledge" (Med-PaLM 2)

---

## Computer Vision Metrics

### Segmentation

| Metric | Definition | When to Use | Limitations |
|---|---|---|---|
| **Dice Coefficient** | 2×|A∩B|/(|A|+|B|) | Overlap between predicted and ground truth | Sensitive to object size |
| **IoU (Jaccard)** | |A∩B|/|A∪B| | Alternative overlap metric | More conservative than Dice |
| **Hausdorff Distance** | Max of min distances between contours | Boundary accuracy | Sensitive to outliers; use 95th percentile |
| **Surface Dice** | Dice applied to surface voxels within tolerance | Clinically relevant boundary accuracy | Requires tolerance parameter |
| **Volumetric Similarity** | 1 - |V_pred - V_true|/(V_pred + V_true) | Volume comparison | Insensitive to spatial overlap |

### Detection

| Metric | Definition | When to Use |
|---|---|---|
| **mAP** | Mean Average Precision across IoU thresholds | Object detection (COCO-style) |
| **mAP@50** | AP at IoU threshold 0.50 | Coarse detection (sufficient for clinical) |
| **mAP@75** | AP at IoU threshold 0.75 | Precise detection (surgical planning) |
| **FROC** | Free-Response ROC | Lesion detection (variable number per image) |

### Explainability (XAI)

| Method | Type | Fidelity | Limitations |
|---|---|---|---|
| **Grad-CAM** | Gradient-based | Medium | Coarse resolution, class-specific |
| **LIME** | Perturbation-based | High (local) | Slow, depends on perturbation strategy |
| **SHAP** | Game-theoretic | High (local + global) | Computationally expensive |
| **Integrated Gradients** | Path-based | High | Requires baseline input choice |
| **Attention Maps** | Architecture-specific | Low-Medium | May not reflect actual reasoning |

**Key resources:**
- [Maier-Hein et al. (2024)](https://doi.org/10.1038/s41592-023-02151-z) — "Metrics reloaded" — comprehensive metric selection framework
- [Reinke et al. (2024)](https://doi.org/10.1038/s41592-023-02150-0) — "Understanding metric-related pitfalls in image analysis validation"
- [Selvaraju et al. (2017)](https://doi.org/10.1109/ICCV.2017.74) — Grad-CAM original paper

---

## Agentic AI Metrics & Autonomy Levels

### Task-Level Metrics

| Metric | Definition | When to Use |
|---|---|---|
| **Task Completion Rate** | % of tasks completed successfully | End-to-end agent evaluation |
| **Tool Accuracy** | Correct tool selection and parameter usage | Tool-using agents |
| **Planning Quality** | Efficiency and correctness of multi-step plans | Planning agents |
| **Safety Score** | Rate of harmful or unintended actions | All agentic systems |
| **Recovery Rate** | Ability to recover from errors | Robust agent evaluation |
| **Delegation Accuracy** | Correct sub-task delegation (multi-agent) | Multi-agent systems |

### Autonomy Level Frameworks

Three frameworks for classifying AI agent autonomy:

| Level | Knight Columbia (2024) | Data Agents/SIGMOD 2026 | Bessemer (2024) |
|---|---|---|---|
| **L0** | No AI | No automation | Human does everything |
| **L1** | AI generates, human executes | AI assists with subtasks | AI suggests, human decides |
| **L2** | AI executes with human approval | AI executes routine, human monitors | AI drafts, human approves |
| **L3** | AI executes, human reviews after | AI handles full workflow, human oversight | AI executes, human spot-checks |
| **L4** | AI autonomous, human sets goals | AI autonomous with guardrails | AI fully autonomous, human exception handling |
| **L5** | Full autonomy | Full autonomy, self-improving | Full autonomy, self-directed |

### Agentic Benchmarks

| Benchmark | Focus | Limitations |
|---|---|---|
| **AgentBench** | Multi-task agent evaluation | Controlled environment ≠ real world |
| **ToolEmu** | Tool-using agent safety | Simulated tools, limited tool diversity |
| **WebArena** | Web browsing agents | Narrow web tasks, reproducibility issues |
| **SWE-bench** | Software engineering agents | Code-only, narrow skill |
| **GAIA** | General AI Assistants | Multi-step reasoning, still MCQ-like |
| **τ-bench** | Real-world task completion | Retail domain specific |

**Key resources:**
- [Liu et al. (2023)](https://doi.org/10.48550/arXiv.2308.03688) — AgentBench: Evaluating LLMs as Agents
- [Ruan et al. (2024)](https://doi.org/10.48550/arXiv.2309.15817) — ToolEmu: identifying risks of LM agents with emulated tool execution
- [Wang et al. (2024)](https://doi.org/10.48550/arXiv.2401.13178) — Survey on LLM-based autonomous agents

---

## The Benchmark Problem

Why most AI benchmarks fail to measure what matters:

### Systemic Issues

| Problem | Description | Prevalence |
|---|---|---|
| **Saturation** | Top models score 90%+ on established benchmarks, reducing discriminative power | ~40% of popular benchmarks |
| **Wrong Interface** | MCQ format tests knowledge recall, not clinical reasoning or real-world capability | Most medical AI benchmarks |
| **Missing Abstention** | Benchmarks don't measure the ability to say "I don't know" — a critical safety feature | Nearly universal |
| **Missing Calibration** | Benchmarks report accuracy but not probability calibration — crucial for clinical trust | ~98% of benchmarks |
| **Data Contamination** | Test data may appear in training corpora | Hard to verify for closed models |
| **Narrow Tasks** | Single-skill evaluation misses multi-step clinical reasoning | Most task-specific benchmarks |
| **Static Evaluation** | Benchmarks don't capture performance drift over time | All static benchmarks |

### What Better Benchmarks Need

1. **Abstention scoring** — Reward models that abstain on uncertain cases rather than guessing
2. **Calibration metrics** — Require ECE, Brier, or calibration slope alongside accuracy
3. **Subgroup analysis** — Report performance across demographics, not just aggregate
4. **Longitudinal evaluation** — Track performance over time (distribution shift, model drift)
5. **Clinical workflow integration** — Test in realistic clinical scenarios, not isolated MCQs
6. **Adversarial robustness** — Include cases designed to expose failure modes
7. **Multi-step reasoning** — Evaluate planning, tool use, and information synthesis
8. **Human-AI collaboration** — Measure whether AI improves human performance, not just standalone accuracy

**Key resources:**
- [Kiela et al. (2021)](https://doi.org/10.48550/arXiv.2104.14337) — "Dynabench: Rethinking Benchmarking in NLP"
- [Bowman & Dahl (2021)](https://doi.org/10.48550/arXiv.2104.02145) — "What Will it Take to Fix Benchmarking in Natural Language Understanding?"
- [Raji et al. (2021)](https://doi.org/10.1145/3442188.3445923) — "AI and the Everything in the Whole Wide World Benchmark"

---

## Cross-Modality Themes

Themes that apply across all AI modalities:

| Theme | Description | Relevant Metrics |
|---|---|---|
| **Calibration** | Predicted confidence should match actual accuracy | ECE, Brier, calibration slope |
| **Abstention** | Ability to say "I don't know" on uncertain inputs | Coverage, selective accuracy, conformal prediction |
| **Fairness** | Equitable performance across demographic groups | 62+ metrics, IBM AIF360 toolkit |
| **Robustness** | Consistent performance under distribution shift | OOD detection, adversarial accuracy |
| **Explainability** | Human-understandable reasoning for predictions | Grad-CAM, SHAP, attention, concept-based |
| **Uncertainty Quantification** | Reliable confidence estimates | Conformal prediction, Bayesian methods, MC Dropout |

**Key resources:**
- [Angelopoulos & Bates (2023)](https://doi.org/10.48550/arXiv.2107.07511) — "Conformal Prediction: A Gentle Introduction" — guaranteed coverage without distributional assumptions
- [Gal & Ghahramani (2016)](https://doi.org/10.48550/arXiv.1506.02142) — "Dropout as a Bayesian Approximation" — MC Dropout for uncertainty

---

## Existing Evaluation Repos

Cross-reference of existing AI evaluation repositories:

| Repository | Scope | Stars | Gap |
|---|---|---|---|
| [awesome-machine-learning](https://github.com/josephmisiti/awesome-machine-learning) | General ML tools & frameworks | 65K+ | No evaluation focus |
| [awesome-deep-learning](https://github.com/ChristosChristofidis/awesome-deep-learning) | Deep learning resources | 23K+ | Limited evaluation metrics |
| [Papers with Code](https://paperswithcode.com/) | Benchmarks + leaderboards | N/A | Lacks cross-modality taxonomy |
| [Hugging Face Evaluate](https://github.com/huggingface/evaluate) | NLP/ML metric library | 1.8K+ | Code-focused, not curated analysis |
| [TorchMetrics](https://github.com/Lightning-AI/torchmetrics) | PyTorch metric library | 2K+ | Implementation, not curation |
| [scikit-learn metrics](https://scikit-learn.org/stable/modules/model_evaluation.html) | Classical ML metrics | N/A | No clinical or LLM metrics |
| [HELM](https://github.com/stanford-crfm/helm) | LLM holistic evaluation | 2K+ | LLM-only, single modality |
| [lm-evaluation-harness](https://github.com/EleutherAI/lm-evaluation-harness) | LLM benchmark suite | 6K+ | LLM-only, task-focused |
| [OpenCompass](https://github.com/open-compass/opencompass) | LLM evaluation platform | 4K+ | LLM-only, Chinese AI focus |
| [IBM AIF360](https://github.com/Trusted-AI/AIF360) | Fairness toolkit | 2.4K+ | Fairness-only, not comprehensive |
| [Metrics Reloaded](https://metrics-reloaded.dkfz.de/) | Biomedical image analysis | N/A | Vision-only, not cross-modality |
| [OHDSI Methods Library](https://github.com/OHDSI/MethodsLibrary) | Observational study methods | N/A | EHR-focused, not AI evaluation |

**What this repo adds:** Cross-modality taxonomy + benchmark critique + autonomy levels + health AI focus + curated structured data.

---

## Structured Data

This repository includes machine-readable evaluation data in the [`data/`](data/) directory:

```
data/
├── metrics/
│   ├── predictive.json         # Predictive AI metrics with definitions
│   ├── generative.json         # Generative AI / LLM metrics
│   ├── computer-vision.json    # Computer vision metrics
│   ├── agentic.json            # Agentic AI metrics
│   └── schema.json             # JSON Schema for metric entries
├── benchmarks/
│   ├── medical-ai-benchmarks.json  # Public medical AI benchmarks
│   └── schema.json                 # JSON Schema for benchmark entries
├── autonomy-levels/
│   ├── frameworks-comparison.json  # L0-L5 framework comparison
│   └── classification-checklist.json # How to classify agent autonomy
└── repos/
    └── evaluation-repos-landscape.json # Survey of existing repos
```

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidance on adding structured data.

---

## Training & Certification

- [fast.ai Practical Deep Learning](https://course.fast.ai/) — Free practical ML course with strong model evaluation sections
- [Stanford CS229: Machine Learning](https://cs229.stanford.edu/) — Foundational ML including evaluation methodology
- [deeplearning.ai ML Specialization](https://www.deeplearning.ai/courses/machine-learning-specialization/) — Andrew Ng's ML course with evaluation modules
- [Coursera: AI in Healthcare Specialization](https://www.coursera.org/specializations/ai-healthcare) — Stanford. Includes clinical AI evaluation
- [DataCamp: Machine Learning Scientist](https://www.datacamp.com/) — Hands-on ML evaluation exercises
- [Kaggle Learn](https://www.kaggle.com/learn) — Free courses on ML fundamentals including model evaluation
- [MIT 6.S191: Introduction to Deep Learning](https://introtodeeplearning.com/) — Free deep learning course
- [Hugging Face Course](https://huggingface.co/course/) — Free NLP/ML course with evaluation library integration
- [RAIGH Academy](https://raigh.org) — PAATHI workforce development program. AI Fundamentals & Metrics module covers cross-modality evaluation, calibration, fairness, and benchmark critical analysis

---

## Methodology

This list is curated using a combination of **agentic extraction** and **human validation**:

1. **Agentic Extraction** — Automated search across PubMed, arXiv, GitHub, and conference proceedings (NeurIPS, ICML, AAAI, MICCAI) using the EvidenceOS systematic review pipeline
2. **Knowledge Graph Integration** — Metrics are linked to entities (modalities, benchmarks, frameworks, tools) to identify coverage gaps
3. **Human Validation** — Every resource is reviewed by at least one ML researcher before inclusion
4. **Continuous Update** — GitHub Actions scan for new evaluation methods weekly; community PRs reviewed by AI + human

We are conducting a **Human vs AI Curation Study** comparing manual metric landscape mapping against agentic extraction. Results will be published as a companion paper: *"A Systematic Landscape Analysis of AI Evaluation Metrics Using Agentic Extraction and Knowledge Graphs."*

---

## Contribute & Publish

We invite ML researchers, clinicians, and students to participate in our **Human vs AI Curation Study**:

- **What:** Compare your manual curation of AI evaluation metrics against our agentic extraction pipeline
- **How:** Fork this repo, add metrics, benchmarks, or tool surveys, submit a PR
- **Outcome:** Co-authorship eligibility on our systematic landscape analysis paper
- **Measurement:** Inter-rater reliability (Krippendorff's alpha) between AI and human curators

Top contributors (5+ validated resources) are eligible for RAIGH Academy Campus Lead status. See [CONTRIBUTING.md](CONTRIBUTING.md) for details.

---

## Sister Repositories

| Repository | Focus | RAIGH Module |
|---|---|---|
| [awesome-african-digital-health](https://github.com/EvidenceOS/awesome-african-digital-health) | African digital health landscape | Digital Health Foundations |
| [awesome-dpi-infrastructure](https://github.com/EvidenceOS/awesome-dpi-infrastructure) | DPI platforms & protocols | DPI Architecture |
| [awesome-health-ai-regulations](https://github.com/EvidenceOS/awesome-health-ai-regulations) | Health AI regulatory frameworks | Regulatory Navigator |
| [awesome-health-ai-evaluation](https://github.com/EvidenceOS/awesome-health-ai-evaluation) | Health AI evaluation methods | Clinical AI Evaluation |
| **awesome-ai-evaluation-metrics** (this repo) | AI evaluation metrics taxonomy | AI Fundamentals & Metrics |

---

## License

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, the authors have waived all copyright and related rights to this work. See [LICENSE](LICENSE).
