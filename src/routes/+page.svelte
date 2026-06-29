<script>
    import { onMount } from "svelte";
    import { base } from "$app/paths";
    import "bulma-carousel/dist/css/bulma-carousel.min.css";
    import Papa from "papaparse";
    import Text from "../components/text.svelte";

    let carousel;

    let rows = [];
    let selectedModels = [];
    let selectedPrinciple = "";
    let filteredPrinciples = [];
    let selectedTask = "";
    let models = [];
    let principles = [];
    let tasks = [];
    let open = false;
    let multiRef;

    onMount(async () => {
        const module = await import("bulma-carousel");
        const bulmaCarousel = module.default;

        const carousels = bulmaCarousel.attach("#carousel", {
            slidesToScroll: 1,
            slidesToShow: 1,
            navigation: true,
            pagination: true,
            loop: true,
        });

        carousel = carousels[0];

        const res = await fetch(`${base}/score.csv`);
        const text = await res.text();
        const parsed = Papa.parse(text, {
            header: true,
            skipEmptyLines: true,
        });
        rows = parsed.data;
        models = [
            ...new Set(
                rows.map((r) => {
                    const m = r.model.trim();
                    return m.charAt(0).toUpperCase() + m.slice(1);
                }),
            ),
        ];
        principles = [...new Set(rows.map((r) => r.principle.trim()))];
        tasks = [...new Set(rows.map((r) => r.task.trim()))];
        if (models.length) {
            selectedModels = [models[0]];
        }
        if (tasks.length) {
            selectedTask = tasks[0];
        }
        if (principles.length) {
            selectedPrinciple = principles[0];
        }
        filteredPrinciples = selectedTask
            ? [
                  ...new Set(
                      rows
                          .filter((r) => r.task === selectedTask)
                          .map((r) => r.principle),
                  ),
              ].filter((p) => p)
            : [];
        if (filteredPrinciples.length) {
            selectedPrinciple = filteredPrinciples[0];
        }
    });

    onMount(() => {
        document.addEventListener("click", handleClickOutside);
        return () => document.removeEventListener("click", handleClickOutside);
    });

    $: filteredRows = rows.filter((r) => {
        const modelName = r.model.trim();
        if (
            selectedModels.length &&
            !selectedModels.some(
                (m) => m.toLowerCase() === modelName.toLowerCase(),
            )
        )
            return false;
        if (selectedPrinciple && r.principle !== selectedPrinciple)
            return false;
        if (selectedTask && r.task !== selectedTask) return false;
        return true;
    });

    $: filteredPrinciples = selectedTask
        ? [
              ...new Set(
                  rows
                      .filter((r) => r.task === selectedTask)
                      .map((r) => r.principle),
              ),
          ].filter((p) => p)
        : [];

    $: {
        if (filteredPrinciples.length === 0) {
            selectedPrinciple = "";
        } else if (!filteredPrinciples.includes(selectedPrinciple)) {
            selectedPrinciple = filteredPrinciples[0];
        }
    }

    function toggle(m) {
        selectedModels = selectedModels.includes(m)
            ? selectedModels.filter((x) => x !== m)
            : [...selectedModels, m];
    }

    function handleClickOutside(event) {
        if (multiRef && !multiRef.contains(event.target)) {
            open = false;
        }
    }
</script>

<div>
    <h1 class="publication_title">
        From Rubrics to Recipe: Principle-Centric Benchmarking and
        Characterization of Tasks for Language Models
    </h1>
    <div
        style="text-align: center; font-size: 1.5em; color:#4A4A4A; padding-top: 5px;"
    >
        <span> ACL 2026 EvalEval Workshop </span>
    </div>
    <div
        class="pulication_authors"
        style="padding-bottom: 15px; padding-top: 15px;"
    >
        <span>
            <a href="https://www.shirley.id/" target="_blank"
                >Shirley Anugrah Hayati</a
            ><sup></sup>,
        </span>
        <span>
            <a href="https://ether9t.github.io" target="_blank">Ruizi Wang</a
            ><sup></sup>,
        </span>
        <span>
            <a href="https://dykang.github.io/" target="_blank">Dongyeop Kang</a
            ><sup></sup>,
        </span>
    </div>
    <div
        style="text-align: center; font-size: 1.5em; color:#4A4A4A; padding-bottom: 15px;"
    >
        University of Minnesota Twin Cities
    </div>
    <div style="display: flex; justify-content: center; gap: 12px;">
        <span class="link-block">
            <a
                href="https://openreview.net/pdf?id=yQhhR9cKE1"
                target="_blank"
                class="external-link button is-normal is-rounded is-dark"
            >
                <span class="icon">
                    <svg
                        width="24"
                        height="24"
                        class="svg-inline--fa fa-file-pdf fa-w-12"
                        aria-hidden="true"
                        focusable="false"
                        data-prefix="fas"
                        data-icon="file-pdf"
                        role="img"
                        xmlns="http://www.w3.org/2000/svg"
                        viewBox="0 0 384 512"
                        data-fa-i2svg=""
                        ><path
                            fill="currentColor"
                            d="M181.9 256.1c-5-16-4.9-46.9-2-46.9 8.4 0 7.6 36.9 2 46.9zm-1.7 47.2c-7.7 20.2-17.3 43.3-28.4 62.7 18.3-7 39-17.2 62.9-21.9-12.7-9.6-24.9-23.4-34.5-40.8zM86.1 428.1c0 .8 13.2-5.4 34.9-40.2-6.7 6.3-29.1 24.5-34.9 40.2zM248 160h136v328c0 13.3-10.7 24-24 24H24c-13.3 0-24-10.7-24-24V24C0 10.7 10.7 0 24 0h200v136c0 13.2 10.8 24 24 24zm-8 171.8c-20-12.2-33.3-29-42.7-53.8 4.5-18.5 11.6-46.6 6.2-64.2-4.7-29.4-42.4-26.5-47.8-6.8-5 18.3-.4 44.1 8.1 77-11.6 27.6-28.7 64.6-40.8 85.8-.1 0-.1.1-.2.1-27.1 13.9-73.6 44.5-54.5 68 5.6 6.9 16 10 21.5 10 17.9 0 35.7-18 61.1-61.8 25.8-8.5 54.1-19.1 79-23.2 21.7 11.8 47.1 19.5 64 19.5 29.2 0 31.2-32 19.7-43.4-13.9-13.6-54.3-9.7-73.6-7.2zM377 105L279 7c-4.5-4.5-10.6-7-17-7h-6v128h128v-6.1c0-6.3-2.5-12.4-7-16.9zm-74.1 255.3c4.1-2.7-2.5-11.9-42.8-9 37.1 15.8 42.8 9 42.8 9z"
                        ></path></svg
                    ><!-- <i class="fas fa-file-pdf"></i> Font Awesome fontawesome.com -->
                </span>
                <span>Paper</span>
            </a>
        </span>

        <span class="link-block">
            <a
                href="https://github.com/minnesotanlp/principle_centric_benchmark/tree/main"
                target="_blank"
                class="external-link button is-normal is-rounded is-dark"
            >
                <span class="icon">
                    <svg
                        width="24"
                        height="24"
                        class="svg-inline--fa fa-github fa-w-16"
                        aria-hidden="true"
                        focusable="false"
                        data-prefix="fab"
                        data-icon="github"
                        role="img"
                        xmlns="http://www.w3.org/2000/svg"
                        viewBox="0 0 496 512"
                        data-fa-i2svg=""
                        ><path
                            fill="currentColor"
                            d="M165.9 397.4c0 2-2.3 3.6-5.2 3.6-3.3.3-5.6-1.3-5.6-3.6 0-2 2.3-3.6 5.2-3.6 3-.3 5.6 1.3 5.6 3.6zm-31.1-4.5c-.7 2 1.3 4.3 4.3 4.9 2.6 1 5.6 0 6.2-2s-1.3-4.3-4.3-5.2c-2.6-.7-5.5.3-6.2 2.3zm44.2-1.7c-2.9.7-4.9 2.6-4.6 4.9.3 2 2.9 3.3 5.9 2.6 2.9-.7 4.9-2.6 4.6-4.6-.3-1.9-3-3.2-5.9-2.9zM244.8 8C106.1 8 0 113.3 0 252c0 110.9 69.8 205.8 169.5 239.2 12.8 2.3 17.3-5.6 17.3-12.1 0-6.2-.3-40.4-.3-61.4 0 0-70 15-84.7-29.8 0 0-11.4-29.1-27.8-36.6 0 0-22.9-15.7 1.6-15.4 0 0 24.9 2 38.6 25.8 21.9 38.6 58.6 27.5 72.9 20.9 2.3-16 8.8-27.1 16-33.7-55.9-6.2-112.3-14.3-112.3-110.5 0-27.5 7.6-41.3 23.6-58.9-2.6-6.5-11.1-33.3 2.6-67.9 20.9-6.5 69 27 69 27 20-5.6 41.5-8.5 62.8-8.5s42.8 2.9 62.8 8.5c0 0 48.1-33.6 69-27 13.7 34.7 5.2 61.4 2.6 67.9 16 17.7 25.8 31.5 25.8 58.9 0 96.5-58.9 104.2-114.8 110.5 9.2 7.9 17 22.9 17 46.4 0 33.7-.3 75.4-.3 83.6 0 6.5 4.6 14.4 17.3 12.1C428.2 457.8 496 362.9 496 252 496 113.3 383.5 8 244.8 8zM97.2 352.9c-1.3 1-1 3.3.7 5.2 1.6 1.6 3.9 2.3 5.2 1 1.3-1 1-3.3-.7-5.2-1.6-1.6-3.9-2.3-5.2-1zm-10.8-8.1c-.7 1.3.3 2.9 2.3 3.9 1.6 1 3.6.7 4.3-.7.7-1.3-.3-2.9-2.3-3.9-2-.6-3.6-.3-4.3.7zm32.4 35.6c-1.6 1.3-1 4.3 1.3 6.2 2.3 2.3 5.2 2.6 6.5 1 1.3-1.3.7-4.3-1.3-6.2-2.2-2.3-5.2-2.6-6.5-1zm-11.4-14.7c-1.6 1-1.6 3.6 0 5.9 1.6 2.3 4.3 3.3 5.6 2.3 1.6-1.3 1.6-3.9 0-6.2-1.4-2.3-4-3.3-5.6-2z"
                        ></path></svg
                    >
                </span>
                <span>Github</span>
            </a>
        </span>
    </div>
</div>
<div>
    <div
        style="background-color: #f5f5f5; color:#363636; padding-top: 5px; padding-bottom: 15px; margin-bottom: 15px;"
    >
        <h2 class="paper_title">Abstract</h2>
        <p class="paper_detail">
            As large language models (LLMs) are increasingly applied to a wide
            range of NLP tasks, it becomes important to systematically
            understand what defines these tasks beyond dataset labels or surface
            instructions. In this work, we explore whether tasks can be
            characterized using principles. Previous studies have mainly focused
            on coarse-grained principles and saying that principles apply to all
            tasks. Others propose principles that are very specific to
            instances. Our study balances the general and specific principles.
            We introduce a framework for automatically annotating tasks with
            such principles and investigate their utility for organizing task
            collections and diagnosing model behavior. We also introduce a new
            principle-guided benchmarking dataset containing more than 20k
            instances. Our findings shows that principles can effectively guide
            LLMs in generating data that meets expected standards. Dataset and
            code will be released upon acceptance.
        </p>
    </div>
    <div style="display: flex; justify-content: center">
        <img
            src="main_figure_paper.jpg"
            style="width: 25%"
            alt="main_figure_paper"
        />
    </div>
    <p
        style="text-align: center; color: #363636; font-size: 1em; margin-bottom: 10px"
    >
        <strong>Figure 1:</strong> Principles are task-specific. Our framework extracts
        them from human rubrics and generates additional task-aware principles for
        evaluation.
    </p>
</div>

<div>
    <h2 class="paper_title">Research Contributions</h2>
    <div class="paper_detail" style="text-align: center;">
        <ul style="display: inline-block; text-align: left;">
            <li>
                A principle-based framework for task characterization, data
                generation, and evaluation.
            </li>
            <li>
                A taxonomy of principles for systematically studying their roles
                in LLM research.
            </li>
            <li>
                A principle-based benchmark dataset with 20,970 instances for
                evaluating LLM capabilities.
            </li>
        </ul>
    </div>
</div>

<div
    style="background-color: #f5f5f5; color:#363636; padding-top: 5px; padding-bottom: 15px; margin-bottom: 15px; margin-top: 15px;"
>
    <h2 class="paper_title">Principles of Taxonomy</h2>
    <div style="display: flex; justify-content: center">
        <img
            src="principle_taxonomy.jpg"
            style="width: 50%"
            alt="principle_taxonomy"
        />
    </div>
    <p style="text-align: center; color: #363636; font-size: 1em;">
        <strong>Figure 2</strong>: Taxonomy of Principles.
    </p>
    <div
        class="paper_detail"
        style="margin-top: 15px; margin-bottom: 15px; font-weight: 500;"
    >
        We propose a taxonomy of principles (Figure 2) to systematically
        describe the characteristics and roles of principles:
    </div>
    <div class="paper_detail">
        <ul>
            <li style="margin-bottom: 10px;">
                <strong style="color: #6193ff;">Granularity</strong>: The level
                of specificity of a principle, from
                <span style="color: #4486ff;">coarse-grained</span> to
                <span style="color: #4486ff;">domain-</span>,
                <span style="color: #4486ff;">task-</span>, and
                <span style="color: #4486ff;">instance-specific</span>.
            </li>

            <li style="margin-bottom: 10px;">
                <strong style="color: #3cb58d;">Requirement Specificity</strong
                >: The degree of
                <span style="color: #4cb58e;">detail</span> and
                <span style="color: #4cb58e;">concreteness</span>, ranging from
                broad criteria to
                <span style="color: #4cb58e;"
                    >narrowly defined requirements</span
                >.
            </li>

            <li style="margin-bottom: 10px;">
                <strong style="color: #f4bd00;">Cognitive Load</strong>: The
                <span style="color: #f7b300;">mental effort</span>
                required to apply a principle, from
                <span style="color: #f7b300;">easy</span> to
                <span style="color: #f7b300;">medium</span> and
                <span style="color: #f7b300;">difficult</span>.
            </li>

            <li style="margin-bottom: 10px;">
                <strong style="color: #fe9910;">Coverage</strong>: Whether a
                principle applies
                <span style="color: #ff9d00;">within a single instance</span>
                or
                <span style="color: #ff9d00;">across multiple instances</span>.
            </li>

            <li style="margin-bottom: 10px;">
                <strong style="color: #dc554b;">Function</strong>: The intended
                <span style="color: #ee6b60;">role</span>
                of a principle, such as
                <span style="color: #ee6b60;">alignment</span>,
                <span style="color: #ee6b60;">evaluation</span>,
                <span style="color: #ee6b60;">data generation</span>, or
                <span style="color: #ee6b60;">prompting</span>.
            </li>

            <li>
                <strong style="color: #987cd6;">Source</strong>: How a principle
                is derived:
                <span style="color: #957bd6;">top-down</span>,
                <span style="color: #957bd6;">task-driven</span>, or
                <span style="color: #957bd6;">bottom-up</span>.
            </li>
        </ul>
    </div>
</div>

<div>
    <h2 class="paper_title">Our Approach</h2>
    <div style="display: flex; justify-content: center">
        <img
            src="principle_generation.jpg"
            style="width: 25%"
            alt="principle_generation"
        />
    </div>
    <p
        class="paper_detail"
        style="text-align: center; color: #363636; font-size: 1em;"
    >
        <strong>Figure 3</strong>: LLM extracts principles from a given task,
        and then generates instances (input-output pairs) given the principles,
        and finally uses the principles to evaluate models' responses.
    </p>
    <div class="paper_detail">
        <ul>
            <li>
                <p
                    style="font-size: 1.25em; font-weight: 500; margin-top: 10px; margin-bottom: 10px;"
                >
                    Step 1: Principle Generation
                </p>
                <p>
                    <strong style="font-size: 1.10em; color: #4A4A4A"
                        >Extracted Principles</strong
                    >
                    = extracted from <em>BiGGen Bench</em> examples and clustered
                    (917 total), whereas generated principles are created from task
                    names alone via one-shot prompting (1180 total).
                </p>
                <p>
                    <strong style="font-size: 1.10em; color: #4A4A4A"
                        >Generated Principles</strong
                    > = generated from task names via one-shot prompting, yielding
                    1,180 principles (16.85 per task).
                </p>
            </li>
            <li>
                <p
                    style="font-size: 1.25em; font-weight: 500; margin-top: 10px; margin-bottom: 10px;"
                >
                    Step 2: Instance Generation
                </p>
            </li>
            <p>
                <strong style="font-size: 1.10em; color: #4A4A4A"
                    >With Principles</strong
                > = generate 10 instances per principle, yielding 20970 instances
                in total.
            </p>

            <p>
                <strong style="font-size: 1.10em; color: #4A4A4A"
                    >Without Principles (baseline)</strong
                > = generate 100 instances per task via iterative bootstrapping without
                principles.
            </p>
            <p>
                In both settings, 10 example instances from <em>BiGGen Bench</em
                > are shown to guide format, not content.
            </p>
            <li>
                <p
                    style="font-size: 1.25em; font-weight: 500; margin-top: 10px; margin-bottom: 10px;"
                >
                    Step 3: Model Inference and Evaluation
                </p>
            </li>
            <strong style="font-size: 1.10em; color: #4A4A4A">
                GPT-4o assigns a 0–5 score for each principle:
            </strong>
            <div style="text-align: center;">
                <ul style="display: inline-block; text-align: left;">
                    <li>
                        Score 0: The principle is not relevant to the response.
                    </li>
                    <li>
                        Score 1: The response does not follow the principle at
                        all.
                    </li>
                    <li>Score 2: The response follows the principle poorly.</li>
                    <li>
                        Score 3: The response partially follows the principle.
                    </li>
                    <li>
                        Score 4: The response sufficiently follows the
                        principle.
                    </li>
                    <li>
                        Score 5: The response correctly and fully follows the
                        principle.
                    </li>
                </ul>
            </div>
        </ul>
    </div>
</div>

<div
    style="background-color: #f5f5f5; color:#363636; padding-top: 5px; padding-bottom: 30px; margin-bottom: 15px; margin-top: 15px;"
>
    <h2 class="paper_title">Key Takeaways</h2>
    <div class="paper_detail">
        <ul>
            <li
                style="font-weight: 500; text-align: center; font-size: 1.25em; margin-top: 15px; margin-bottom: 10px;"
            >
                What principles characterize a task?
            </li>
            <div style="display: flex; justify-content: center;">
                <img
                    src="cooccurrence.jpg"
                    style="width: 75%;"
                    alt="cooccurrence"
                />
            </div>
            <p
                style="text-align: center; color: #363636; font-size: 1em; margin-bottom: 10px"
            >
                <strong>Figure 4</strong>: Principle co-occurrences across three
                tasks. <br />
                Top: <em>BiGGen Bench</em> principles; Bottom: principles from unguided
                LLM-generated data.
            </p>
            <p>
                <span
                    style="display:inline-block; background-color: white; padding:4px 8px;"
                >
                    Principle co-occurrence patterns differ substantially
                    between human-curated and unguided LLM-generated data,
                    suggesting that principle-guided generation produces more
                    structured datasets.
                </span>
            </p>
            <li
                style="font-weight: 500; text-align: center; font-size: 1.25em; margin-top: 15px; margin-bottom: 10px;"
            >
                Similarity between LLM-extracted and generated principles vs.
                human-written Principles?
            </li>
            <div style="display: flex; justify-content: center">
                <img
                    src="principle_similarity.png"
                    style="width: 125%"
                    alt="principle_similarity"
                />
            </div>
            <p
                style="text-align: center; color: #363636; font-size: 1em; margin-bottom: 10px"
            >
                <strong>Table 1</strong>: Cosine similarity between
                human-written, LLM-extracted, and LLM-generated principles.
            </p>
            <p>
                <span
                    style="display:inline-block; background-color: white; padding:4px 8px;"
                >
                    LLM-generated and extracted principles exhibit similar
                    semantic alignment with human-written principles, achieving
                    average SentenceBERT cosine similarities of 0.37 and 0.35,
                    respectively.
                </span>
            </p>
            <li
                style="font-weight: 500; text-align: center; font-size: 1.25em; margin-top: 15px; margin-bottom: 10px;"
            >
                How useful are principles for data generation?
            </li>
            <div style="display: flex; justify-content: center">
                <img
                    src="travel_plan.png"
                    style="width: 125%"
                    alt="travel_plan"
                />
            </div>
            <p
                style="text-align: center; color: #363636; font-size: 1em; margin-bottom: 10px;"
            >
                <strong>Table 2</strong>: Comparison of Extracted and Generated
                Principles Data for task Travel Plan.
            </p>
            <p>
                <span
                    style="display:inline-block; background-color: white; padding:4px 8px;"
                >
                    Providing principles during generation substantially
                    improves adherence, achieving an average score of 4.35
                    compared to 2.73 without principles.
                </span>
            </p>
            <li
                style="font-weight: 500; text-align: center; font-size: 1.25em; margin-top: 15px; margin-bottom: 10px;"
            >
                Benchmarking LLMs
            </li>

            <div style="display: flex; justify-content: center;">
                <img
                    src="eval_results_subcolumns.png"
                    style="width: 100%;"
                    alt="eval_results_subcolumns"
                />
            </div>
            <p
                style="text-align: center; color: #363636; font-size: 1em; margin-bottom: 10px;"
            >
                <strong>Table 3</strong>: Evaluation results on generated (Gen.)
                and extracted (Extr.) principles across LLMs.
                <strong>Bold</strong> and <u>underlined</u> values indicate the best
                results.
            </p>

            <div id="carousel" class="carousel results-carousel">
                <div class="item" style="margin-bottom: 20px;">
                    <div style="display: flex; justify-content: center">
                        <img
                            src="reasoning.jpg"
                            style="width: 45%;"
                            alt="reasoning"
                        />
                    </div>
                    <p><strong>Figure 5</strong>: Reasoning</p>
                </div>
                <div class="item">
                    <div style="display: flex; justify-content: center">
                        <img
                            src="safety.jpg"
                            style="width: 45%;"
                            alt="safety"
                        />
                    </div>
                    <p><strong>Figure 6</strong>: Safety</p>
                </div>
            </div>
            <p>
                <span
                    style="display:inline-block; background-color: white; padding:4px 8px;"
                >
                    Principle-based evaluation reveals substantial differences
                    across LLM capabilities. Qwen2.5-14B performs best overall,
                    while reasoning and safety-related logic remain challenging
                    for all models.
                </span>
            </p>
        </ul>
    </div>
</div>

<div style="margin-bottom: 30px;">
    <h2 class="paper_title">Data</h2>
    <div class="paper_detail">
        <div class="filters">
            <div class="multi-select" bind:this={multiRef}>
                <span>Model: </span>
                <button class="trigger" on:click={() => (open = !open)}>
                    {selectedModels.length
                        ? selectedModels.join(", ")
                        : "Select models"}
                    <span>▾</span>
                </button>
                {#if open}
                    <div class="panel">
                        {#each models as m}
                            <label>
                                <input
                                    type="checkbox"
                                    checked={selectedModels.includes(m)}
                                    on:change={() => toggle(m)}
                                />
                                {m}
                            </label>
                        {/each}
                    </div>
                {/if}
            </div>
        </div>
        <div class="filters">
            <span>Task: </span>
            <select bind:value={selectedTask} class="select-style">
                {#each tasks as t}
                    <option value={t}>{t}</option>
                {/each}
            </select>
            <span>Principle: </span>
            <select bind:value={selectedPrinciple} class="select-style">
                {#each filteredPrinciples as p}
                    <option value={p}>{p}</option>
                {/each}
            </select>
        </div>
        <table>
            <thead>
                <tr>
                    <th>No</th>
                    <th>Input</th>
                    <th>Output</th>
                    <th>Score</th>
                </tr>
            </thead>
            <tbody>
                {#each filteredRows as row, i}
                    <tr>
                        <td>{i + 1}</td>
                        <td><Text text={row.input} /></td>
                        <td><Text text={row.output} /></td>
                        <td>{row.score}</td>
                    </tr>
                {/each}
            </tbody>
        </table>
    </div>
</div>

<h2 class="paper_title">Citation</h2>
<div style="text-align: center;">
    <div class="citation-box" style="text-align: center;">
        <pre id="citation">
    @misc&#123;koo2023benchmarking,
        title=&#123;Benchmarking Cognitive Biases in Large Language Models as Evaluators&#125;,
        author=&#123;Ryan Koo and Minhwa Lee and Vipul Raheja and
                Jong Inn Park and Zae Myung Kim and Dongyeop Kang&#125;,
        year=&#123;2023&#125;,
        eprint=&#123;2309.17012&#125;,
        archivePrefix=&#123;arXiv&#125;,
        primaryClass=&#123;cs.CL&#125;
    &#125;
    </pre>
    </div>
</div>

<svelte:head>
    <title>From Rubrics to Recipe</title>
</svelte:head>

<style>
    * {
        font-family: "Roboto", sans-serif;
    }

    .publication_title {
        text-align: center;
        font-weight: 500;
        font-size: 2.25em;
        margin-top: 5vh;
        margin-left: 5vw;
        margin-right: 5vw;
        color: #363636;
    }

    .pulication_authors {
        text-align: center;
        font-size: 1.5em;
    }

    .paper_title {
        text-align: center;
        font-size: 1.5em;
        font-weight: 500;
        color: #363636;
        margin-top: 15px;
        margin-bottom: 10px;
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

    a:not(.button) {
        color: #137a7f;
        text-decoration: none;
    }

    a:not(.button):hover {
        color: #86cecb;
        text-decoration: underline;
    }

    .multi-select {
        display: flex;
        gap: 8px;
        align-items: center;
        position: relative;
        width: 200px;
    }

    .trigger {
        width: 100%;
        padding: 8px 10px;
        border: 1px solid #ddd;
        border-radius: 6px;
        background: #fafafa;
        cursor: pointer;
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .panel {
        position: absolute;
        top: 100%;
        left: 0;
        width: 100%;
        background-color: #fafafa;
        border: 1px solid #ddd;
        margin-top: 2px;
        padding: 4px 8px;
        z-index: 10;
        overflow: hidden;
    }

    .panel label {
        display: flex;
        gap: 6px;
        padding: 4px;
        font-size: 14px;
    }

    select {
        padding: 8px 10px;
        border-radius: 6px;
        border: 1px solid #ddd;
        background: #fafafa;
    }

    table {
        width: 100%;
        border-collapse: collapse;
        margin-top: 24px;
    }

    th,
    td {
        border: 1px solid #eee;
        padding: 8px;
        text-align: center;
    }
    th {
        background: #f5f5f5;
    }
    .filters {
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 16px;
        margin: 24px 0;
    }

    .select-style {
        padding: 8px 12px;
        border-radius: 8px;
        border: 1px solid #ddd;
        background: #fafafa;
        font-size: 14px;
        cursor: pointer;
    }

    .external-link.button {
        transition: all 0.2s ease;
    }

    .external-link.button:hover {
        background-color: #4a4a4a;
        transform: translateY(-1px);
    }

    .link-block {
        display: flex;
        justify-content: center;
        margin-bottom: 1rem;
    }

    .citation-box {
        display: inline-block;
        justify-content: center;
        background: #f5f5f5;
        padding: 20px;
        border-radius: 4px;
        overflow-x: auto;
        position: relative;
    }

    .citation-box pre {
        text-align: left;
    }
</style>
