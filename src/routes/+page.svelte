<script>
  import { onMount } from 'svelte';
  import 'bulma-carousel/dist/css/bulma-carousel.min.css';

  let carousel;

  onMount(async () => {
    const module = await import('bulma-carousel');
    const bulmaCarousel = module.default;

    const carousels = bulmaCarousel.attach('#carousel', {
      slidesToScroll: 1,
      slidesToShow: 1,
      navigation: true,
      pagination: true,
      loop: true
    });

    carousel = carousels[0];
  });
</script>

<div>
    <h1 class="publication_title">
        From Rubrics to Recipe: Principle-Centric Benchmarking and Characterization of Tasks for Language Models
    </h1>
    <div class="pulication_authors">
        <span>
            <a href="https://www.shirley.id/" target="_blank"
            >Shirley Anugrah Hayati</a
            ><sup></sup>,
        </span>
        <span>
            <a href="https://ether9t.github.io" target="_blank"
            >Ruizi Wang</a
            ><sup></sup>,
        </span>
        <span>
            <a href="https://dykang.github.io/" target="_blank"
            >Dongyeop Kang</a
            ><sup></sup>,
        </span>
    </div>
</div>

<div>
    <h2 class="paper_title">
        Abstract
    </h2>
    <p class="paper_detail">
        As large language models (LLMs) are increasingly applied to a wide range of NLP tasks, it becomes important to systematically understand what defines these tasks beyond dataset labels or surface instructions. 
        In this work, we explore whether tasks can be characterized using principles. Previous studies have mainly focused on coarse-grained principles and saying that principles apply to all tasks. Others propose principles that are very specific to instances. 
        Our study balances the general and specific principles. We introduce a framework for automatically annotating tasks with such principles and investigate their utility for organizing task collections and diagnosing model behavior. We also introduce a new principle-guided benchmarking dataset containing more than 20k instances. 
        Our findings shows that principles can effectively guide LLMs in generating data that meets expected standards. Dataset and code will be released upon acceptance.
    </p>
    <div style="display: flex; justify-content: center">
        <img src="main_figure_paper.jpg" style="width: 25%" alt="main_figure_paper"/>
    </div>
    <p class="paper_detail" style="text-align: center; color: #363636; font-size: 0.75em;">Figure 1: Some principles are specific to that task. For a travel plan task, a user who asks LLM to design a travel schedule (input), require an output that adheres to certain principles.
        <strong>efficient planning</strong> and <strong>photography</strong> may be essential while <strong>fine dining</strong> is irrelevant unless explicitly asked. 
        Our framework extract these principles from human rubrics and can also ask an LLM to propose additional principles, producing dynamic, task-aware evaluations.
    </p>
</div>

<div>
    <h2 class="paper_title">
        Research Contributions
    </h2>
    <div class="paper_detail">
         <ul>
            <li>
                A framework that builds principles into every stage:<br>
                &nbsp;&nbsp;&nbsp;&nbsp;(i) elicit principles for task characterization, <br>
                &nbsp;&nbsp;&nbsp;&nbsp;(ii) use them to steer synthetic data generation, and <br>
                &nbsp;&nbsp;&nbsp;&nbsp;(iii) evaluate model outputs against the same (check)-list of principles. 
            </li>
            <li>
                A new taxonomy of principles to systematically describe the characteristics and roles of principles in LLM research.
            </li>
            <li>
                A new benchmarking principle-based dataset (20,970 instances) to examine language model's capability.
            </li>
        </ul>
    </div>
</div>

<div>
    <h2 class="paper_title">
        Principles of Taxonomy
    </h2>
    <div style="display: flex; justify-content: center">
        <img src="principle_taxonomy.jpg" style="width: 50%" alt="principle_taxonomy"/>
    </div>
    <p style="text-align: center; color: #363636; font-size: 0.75em;">Figure 1: Taxonomy of Principles.</p>
    <div class="paper_detail">
        We propose a taxonomy of principles (Figure 1) to systematically describe the characteristics and roles of principles. 
        This taxonomy captures multiple dimensions along which principles can vary, such as their level of granularity, the cognitive effort they demand, their coverage across instances, their functional role, and their origin. 
    </div>
</div>

<div>
    <h2 class="paper_title">
        Our Approach
    </h2>
    <div style="display: flex; justify-content: center">
        <img src="principle_generation.jpg" style="width: 25%" alt="principle_generation"/>
    </div>
    <p class="paper_detail" style="text-align: center; color: #363636; font-size: 0.75em;">Figure 2: LLM extracts principles from a given task, and then generates instances (input-output pairs) given the principles, and finally uses the principles to evaluate models' responses.</p>
    <div class="paper_detail">
        <ul>
            <li>
                Step 1: Principle Generation
                    <p>
                        <strong>Extracted principles</strong> = we prompt an LLM with an input, an output, criteria, and a rubric score of 5 from BiGGen Bench to extract principles, and then cluster similar principles and ask the LLM to choose a representative name and summary. 
                        On average, 13.1 principles are extracted per task, resulting in a total of 917.
                    </p>
                    <p>
                        <strong>Generated principles</strong> = we generate principles using one-shot prompting, providing only the task name without examples. On average, 16.85 principles are generated per task, with a total of 1,180.
                    </p>
            </li>
            <li>
                Step 2: Instance Generation
            </li>
            <p>
                <strong>With principles</strong> = we prompt the LLM with a specific principle and ask it to generate 10 instances that adhere to it. This results in a total of 20,970 instances.
            </p>
            <p>
                <strong>Without principles (baseline)</strong> = we use an iterative bootstrapping method without any principles, generating 100 instances per task.
            </p>
            <p>
                In both settings, 10 example instances from BiGGen Bench are shown to guide format, not content.
            </p>
            <li>
                Step 3: Model Inference and Evaluation
            </li>
            <p>
                We use GPT-4o to assess how well each generated response adheres to the given principles. 
                For each principle, GPT-4o assigns a score from 0 to 5:
            </p>
            <ul>
                <li>
                    Score 0: The principle is not relevant to the response.
                </li>
                <li>
                    Score 1: The response does not follow the principle at all.
                </li>
                <li>
                    Score 2: The response follows the principle poorly.
                </li>
                <li>
                    Score 3: The response partially follows the principle.
                </li>
                <li>
                    Score 4: The response sufficiently follows the principle.
                </li>
                <li>
                    Score 5: The response correctly and fully follows the principle.
                </li>
            </ul>
        </ul>
    </div>
</div>

<div>
    <h2 class="paper_title">
        Key Takeaways
    </h2>
    <div class="paper_detail">
        <ul>
            <li>
                What principles characterize a task?
            </li>
            <div style="display: flex; justify-content: center">
                <img src="cooccurrence.jpg" style="width: 75%" alt="cooccurrence"/>
            </div>
            <p style="text-align: center; color: #363636; font-size: 0.75em;">
                Figure 3: Principle co-occurrences for three tasks (travel plan, moral belief, and math proof) from different model capabilities. Top graphs are principles on data from BigGen Bench while bottom graphs are principles on dataset generated by LLM without principle guidance.
            </p>
            <p>
                We analyzed principle co-occurrences in human-curated and LLM-generated instances without explicit principles. <br>
                By inferring and selecting high-scoring principles from the latter, we observed clear differences in usage and co-occurrence patterns. <br>
                This indicates that incorporating principles during generation leads to more structured and controlled datasets. 
            </p>
            <li>
                Similarity between LLM-extracted and generated principles vs. human-written Principles?
            </li>
            <div style="display: flex; justify-content: center">
                <img src="principle_similarity.png" style="width: 125%" alt="principle_similarity"/>
            </div>
            <p style="text-align: center; color: #363636; font-size: 0.75em;">
                Table 1: Cosine similarity score between human-written principles, LLM-extracted principles (Extract), and LLM-generated principles (Generate).
            </p>
            <p>
                We compare LLM-extracted and generated principles to human-written ones created from task names and descriptions. <br>
                Using SentenceBERT embeddings, both LLM-generated and extracted principles show similar similarity to human-written principles, with average cosine scores of 0.37 and 0.35, respectively.
            </p>
            <li>
                How useful are principles for data generation?
            </li>
            <div style="display: flex; justify-content: center">
                <img src="travel_plan.png" style="width: 125%" alt="travel_plan"/>
            </div>
            <p style="text-align: center; color: #363636; font-size: 0.75em;">
                Table 2: Comparison of Extracted and Generated Principles Data for task Travel Plan.
            </p>
            <p>
                We evaluate the impact of providing principles during data generation using a rubric. 
                Generation with principles consistently achieves higher adherence, especially for unfamiliar principles. <br> 
                Without principles, adherence scores are notably lower. 
                Overall, including principles in prompts significantly improves adherence, with average scores of 4.35 versus 2.73 without principles.
            </p>
            <li>
                Benchmarking LLMs
            </li>
            <div id="carousel" class="carousel results-carousel">
                <div class="item">
                    <div style="display: flex; justify-content: center; margin-top: 12vh;">
                        <img src="eval_results_subcolumns.png" style="width: 100%;" alt="eval_results_subcolumns" />
                    </div>
                    <p>Table 3: Evaluation results on generated principles (Gen.) and extracted (Extr.) principles for various LLMs. <strong>Bold</strong> numbers are the highest scores for a capability for generated principles while <u>underlined</u> numbers are highest scores for extracted principles.</p>
                </div>
                <div class="item">
                    <div style="display: flex; justify-content: center">
                        <img src="reasoning.jpg" style="width: 45%;" alt="reasoning" />
                    </div>
                    <p>Figure 4: Reasoning</p>
                </div>
                <div class="item">
                    <div style="display: flex; justify-content: center">
                        <img src="safety.jpg" style="width: 45%;" alt="safety" />
                    </div>
                    <p>Figure 5: Safety</p>
                </div>
            </div>
            <p>
                We propose a principle-based benchmarking approach that evaluates LLMs on specific qualities such as planning, reasoning, and cultural awareness, offering more precise insights than general benchmarks. <br>
                Using this method, we assessed variants of Gemma-2 and Qwen 2.5, with GPT-4 as a reference. 
                Our results show that Qwen2.5-14B consistently outperforms other models, including GPT-4o. <br>
                However, all models face challenges in reasoning with first-order logic and handling if-else statements related to safety. 
                Notably, GPT-4 excels at the “keeping a secret” safety task but struggles with if-else logic. 
                Overall, Gemma variants perform the weakest.
            </p>
        </ul>
    </div>
</div>
    
<style>
  * {
    font-family: 'Poppins', sans-serif;
  }

  .publication_title {
    text-align: center;
    font-size: 2.25em;
    margin-top: 5vh;
    margin-left: 5vw;
    margin-right: 5vw;
    color: #363636;
  }

  .pulication_authors {
    text-align: center;
    font-size: 1.125em;
  }

  .paper_title {
    text-align: center;
    font-size: 1.5em;
    color: #363636;
  }

  .paper_detail {
    font-size: 1em;
    text-align: justify;
    margin-left: 20vw;
    margin-right: 20vw;
    color: #4a4a4a;
  }

  @media (max-width: 600px) {
    .paper_detail {
        font-size: 1em;
        text-align: justify;
        margin-left: 5vw;
        margin-right: 5vw;
        color: #4a4a4a;
    }
  }

  .carousel {
    max-width: 100%;
    overflow: hidden;
    position: relative;
  }

  .carousel .item {
    text-align: center;
  }

  .carousel .item img {
    display: block;
    margin: 0 auto;
  }
</style>

<svelte:head>
  <title>From Rubrics to Recipe: Principle-Centric Benchmarking and Characterization of Tasks for Language Models</title>
  <link href="https://fonts.googleapis.com/css2?family=Poppins&display=swap" rel="stylesheet" />
</svelte:head>