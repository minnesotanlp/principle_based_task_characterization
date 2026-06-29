# Principle-Based Task Characterization

A SvelteKit web application visualizing the taxonomy and benchmark results from the paper:

> **"From Rubrics to Recipe: Principle-Centric Benchmark for Evaluating Large Language Models"**
> Hayati et al., ACL 2026 EvalEval Workshop

**Live Demo**: https://minnesotanlp.github.io/principle_based_task_characterization/

## About

This interactive site presents:
- A taxonomy of task principles extracted from [BigGen](https://arxiv.org/abs/2406.05761) evaluation rubrics
- Similarity and co-occurrence heatmaps across 52+ task types
- Evaluation results comparing GPT, Qwen, and Gemma model families
- Reasoning capability breakdown across principle categories

## Development

```bash
npm install
npm run dev        # start dev server (http://localhost:5173)
npm run build      # production build
npm run preview    # preview production build
```

## Deployment

The site auto-deploys to GitHub Pages via the workflow in `.github/workflows/deploy.yml` on every push to `main`.

## Citation

```bibtex
@inproceedings{hayati-etal-2026-from,
    title     = "From Rubrics to Recipe: Principle-Centric Benchmark for Evaluating Large Language Models",
    author    = "Hayati, Shirley Anugrah and Wang, Ruizi and Kang, Dongyeop",
    booktitle = "ACL 2026 Workshop on Evaluating Evaluations (EvalEval)",
    year      = "2026",
    address   = "San Diego, United States of America",
    publisher = "Association for Computational Linguistics",
}
```
