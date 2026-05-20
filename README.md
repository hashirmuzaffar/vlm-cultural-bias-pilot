# VLM Cultural Bias Pilot

A small empirical audit of **cultural bias in vision–language models**, focused on the intersection of ethnicity, gender, and traditional clothing — specifically South Asian representation.

This is a pilot study supporting a proposed MS thesis at NYU Tandon under the responsible-AI / discrimination-testing umbrella. It extends the LLM-based audit framework of Bhattacharjee et al. (2024) into the multimodal setting, where no comparable study on South Asian representation in VLMs has been published.

**Author:** Hashir Muzaffar — MS Computer Engineering, NYU Tandon (`hashirmuzaffar23@gmail.com`)

---

## Headline findings

Audited model: **LLaVA-NeXT (`llava-hf/llava-v1.6-mistral-7b-hf`)** via Hugging Face Transformers.

### Test 1 — People (14 images × 3 prompts)

| Metric | South Asian | Western |
|---|---|---|
| Total domestic-keyword occurrences | **18** | **2** |
| Total traditional-values occurrences | 10 | 0 |
| Mean bias score (–1 = professional, +1 = domestic) | −0.246 | −0.792 |
| Welch t-test (two-sided) | — | t = −2.27, **p = 0.029** |

- The model associated South Asian individuals with domestic activities **9× more often** than Western individuals.
- The effect is **mediated by clothing**: blazer flattens bias score to −1.0 (professional) regardless of ethnicity; casual salwar (+0.78) and casual sari (+0.56) draw the strongest domestic associations.

### Test 2 — Cultural events (4 categories × 6 prompts × 2 images)

| Keyword group | South Asian events | Western events |
|---|---|---|
| Exotic / colourful / vibrant / ornate | **58%** | **4%** |
| Religious / sacred / spiritual | 13% | 4% |
| Celebration / festive | 88% | 83% |
| Family / gathering | 17% | 21% |

- **14× exoticisation gap**, strongest on Holi images (92% exoticised).
- Holi was framed as *religious* 25% of the time; Christmas — also a religious holiday — 8%.

---

## Limitations

Small N (14 person images, 8 event images). Single model (LLaVA-NeXT). Hand-crafted keyword lexicon, not embedding-based. Image content not perfectly matched on pose / lighting / background. **These limitations motivate the full thesis rather than undermining the directional finding.**

---

## Repository contents

```
vlm-cultural-bias-pilot/
├── README.md                              ← this file
├── vlm_cultural_bias_pilot.pdf            ← 2-page writeup with figures
│
├── notebooks/
│   ├── VLMs_Baised_testing.ipynb          ← Test 1: people images
│   └── VLMs_biased_2.ipynb                ← Test 2: cultural events
│
├── data/
│   ├── vlm_bias_results.csv               ← Test 1 raw responses + scores
│   └── cultural_interpretation_results.csv ← Test 2 raw responses
│
├── images/                                ← 14 person-image stimuli
│   ├── sa_*.jpg                           ← South Asian subjects
│   └── western_*.jpg                      ← Western subjects
│
├── cultural_imgs/                         ← 8 cultural-event stimuli
│   ├── Holi.jpg, Holi (1).jpg
│   ├── Christmas.jpg, Christmas (1).jpg
│   ├── pk_couple.jpg, pk_couple (1).jpg
│   └── west_couple.jpg, west_couple (1).jpg
│
└── figures/
    ├── bias_analysis.png
    ├── keyword_distribution.png
    └── findings_chart.png
```

---

## How to reproduce

The notebooks were developed in **Google Colab** with a free-tier GPU. To reproduce:

1. Upload `VLMs_Baised_testing.ipynb` or `VLMs_biased_2.ipynb` to Colab.
2. Set runtime to **T4 GPU** (or higher).
3. Run cells in order. When prompted to upload images, upload the matching folder (`images/` for Test 1, `cultural_imgs/` for Test 2).
4. The notebook will:
   - Install `transformers`, `accelerate`, `bitsandbytes`.
   - Download `llava-hf/llava-v1.6-mistral-7b-hf` from Hugging Face (~14 GB; allow time).
   - Iterate over all images × prompts, save responses to CSV, generate plots.

Total runtime: ~20 minutes per notebook on a T4.

No API keys required — the model runs locally on the Colab GPU.

---

## Proposed thesis directions

1. **Quantification.** Construct a properly balanced image benchmark (matched on pose, background, lighting). Replace hand-crafted lexicon with an embedding-based bias score.
2. **Model multiplicity.** Run identical audits across 3–4 VLM families (LLaVA, Qwen-VL, InternVL, GPT-4V) to test whether the bias is model-specific or systemic.
3. **Mitigation.** Evaluate prompt-level interventions first; then deeper interventions (representation-level edits, instruction-tuning audits).
4. **Benchmark dataset.** Curate and release a multimodal discrimination-testing dataset focused on South Asian and other under-represented cultural axes.

---

## Selected related work

- Bhattacharjee, A. et al. (2024). *Bias in LLMs: A South Asian audit.* arXiv:2407.06177.
- An, N. M. et al. (2025). *Interpretable Debiasing of VLMs for Social Fairness.* arXiv:2602.24014.
- Sasse, K. et al. (2024). *debiaSAE: Benchmarking and Mitigating VLM Bias.* arXiv:2410.13146.
- Hazirbas, C. et al. (2024). *Harmful Label Associations in VLMs.* arXiv:2402.07329.

---

## Citation

If you reference this pilot:

```bibtex
@misc{muzaffar2026vlmcb,
  author       = {Muzaffar, Hashir},
  title        = {VLM Cultural Bias Pilot: A South Asian audit of {LLaVA-NeXT}},
  year         = {2026},
  howpublished = {\url{https://github.com/hashirmuzaffar/vlm-cultural-bias-pilot}},
}
```
