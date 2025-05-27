<script>

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
    <p style="text-align: center; color: #363636; font-size: 12px;">Figure 1: Taxonomy of Principles.</p>
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
    <p class="paper_detail" style="text-align: center; color: #363636; font-size: 12px;">Figure 2: LLM extracts principles from a given task, and then generates instances (input-output pairs) given the principles, and finally uses the principles to evaluate models' responses.</p>
    <div class="paper_detail">
        <p>
            To obtain task-level principles, we have two approaches.
        </p>
        <ul>
            <li>
                Step 1: Principle Generation
                    <p>
                        We use two approaches to obtain task-level principles. In the first approach, we prompt an LLM with an input, an output, criteria, and a rubric score of 5 from BiGGen Bench to extract principles. We refer to these as <strong>extracted principles</strong>.
                    </p>
                    <p>
                        Since some criteria within the same task are similar, the LLM may generate overlapping principles. We cluster similar principles and ask the LLM to choose a representative name and summary. On average, 13.1 principles are extracted per task, resulting in a total of 917.
                    </p>
                    <p>
                        In the second approach, we generate principles using one-shot prompting, providing only the task name without examples. We refer to these as <strong>generated principles</strong>. On average, 16.85 principles are generated per task, with a total of 1,180.
                    </p>
            </li>
            <li>
                Step 2: Instance Generation
            </li>
            <p>
                We test if the LLM follows principles by prompting it with one specific principle to generate 10 instances each, totaling 20,970 instances.
            </p>
            <p>
                As a baseline, we generate 100 instances per task without using principles, via iterative bootstrapping.
            </p>
            <p>
                For both principle-based and baseline generation, we provide 10 BiGGen Bench examples to guide the format, not the content.
            </p>
            <li>
                Step 3: Model Inference and Evaluation
            </li>
            <p>
                To examine how much the generated data instances follow the principles, we ask GPT-4o to evaluate the quality of each response based on the principles. 
                GPT-4o must give a score from 0 to 5, defined as follows:
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
            <p>
                We analyzed principle co-occurrences in human-curated and LLM-generated instances without explicit principles. <br>
                By inferring and selecting high-scoring principles from the latter, we observed clear differences in usage and co-occurrence patterns. <br>
                This indicates that incorporating principles during generation leads to more structured and controlled datasets. 
            </p>
            <li>
                Similarity between LLM-extracted and generated principles vs. human-written Principles?
            </li>
            <p>
                We compare LLM-extracted and generated principles to human-written ones created from task names and descriptions. <br>
                Using SentenceBERT embeddings, both LLM-generated and extracted principles show similar similarity to human-written principles, with average cosine scores of 0.37 and 0.35, respectively.
            </p>
            <li>
                How useful are principles for data generation
            </li>
            <p>
                We evaluate the impact of providing principles during data generation using a rubric. 
                Generation with principles consistently achieves higher adherence, especially for unfamiliar principles. <br> 
                Without principles, adherence scores are notably lower. 
                Overall, including principles in prompts significantly improves adherence, with average scores of 4.35 versus 2.73 without principles.
            </p>
            <li>
                Benchmarking LLMs
            </li>
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
    font-size: 36px; 
    margin-top: 50px; 
    margin-left: 50px; 
    margin-right: 100px;
    color: #363636;
  }
  
  .pulication_authors {
    text-align: center;
    font-size: 18px; 
  }

  .paper_title {
    text-align: center;
    font-size: 24px; 
    color: #363636;
  }

  .paper_detail {
    font-size: 16px; 
    text-align: justify;
    margin-left: 300px;
    margin-right: 300px;
    color: #4a4a4a;
  }
</style>

<svelte:head>
  <title>From Rubrics to Recipe: Principle-Centric Benchmarking and Characterization of Tasks for Language Models</title>
  <link href="https://fonts.googleapis.com/css2?family=Poppins&display=swap" rel="stylesheet" />
</svelte:head>