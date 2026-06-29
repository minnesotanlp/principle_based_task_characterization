# principle_centric_benchmark
Website for the ACL 2026 EvalEval Paper paper ["From Rubrics to Recipe: Principle-Centric Benchmark for Evaluating Large Language Models"](https://openreview.net/pdf?id=yQhhR9cKE1). Dataset and code can be found [here](https://github.com/minnesotanlp/principle_centric_benchmark).

## Data
`static/score.csv` contains evaluation results for all benchmark instances.

| Column      | Description                                                                                                                                  |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| `score`     | Evaluation score assigned to the model output. Scores are on a scale of `0-5`, where higher values indicate better performance. |
| `model`     | The evaluated model. Current models include `gemma`, `gpt`, `qwen3b`, `qwen7b`, and `qwen14b`.                                               |
| `task`      | Task category associated with the example.                                                                                         |
| `principle` | Human readable standards or requirements that shape the output.                                                                                                    |
| `input`     | Input prompt provided to the model.                                                                                                          |
| `output`    | Model-generated response.                                                                                         |


## Citation
```
@inproceedings{hayati-etal-2026-from,
    title = "From Rubrics to Recipe: Principle-Centric Benchmark for Evaluating Large Language Models",
    author = "Hayati, Shirley Anugrah and Wang, Ruizi and Kang, Dongyeop",
    booktitle = "ACL 2026 Workshop on Evaluating Evaluations (EvalEval)",
    month = jul,
    year = "2026",
    address = "San Diego, United States of America",
    publisher = "Association for Computational Linguistics",
}
```
