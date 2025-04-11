# Opensource operators evals

<p align="center">
  <img src="https://github.com/user-attachments/assets/36fd0a19-c0bb-45fe-9593-5c01d39a2f2e" alt="podium">
</p>

| Rank | Provider                                                    | Agent Self-Report | LLM Evaluation | Time per Task | Task Reliability |
| ---- | ----------------------------------------------------------- | ----------------- | -------------- | ------------- | ---------------- |
| 🏆   | [Notte](https://github.com/nottelabs/notte)                 | **86.2%**         | **79.0%**      | **47s**       | **96.6%**        |
| 2️⃣   | [Browser-Use](https://github.com/browser-use/browser-use)   | 77.3%             | 60.2%          | 113s          | 83.3%            |
| 3️⃣   | [Convergence](https://github.com/convergence-ai/proxy-lite) | 38.4%             | 31.4%          | 83s           | 50%              |

# Key highlights

[You can investigate all replays/logs](#replays) and [reproduce the benchmark yourself](#reproduce-evals) 👇🏻

- [Notte](https://github.com/nottelabs/notte) leads the benchmark by achieving the highest performance with 86.2% self-reported success and 79% LLM-verified completion. It also has the fastest execution time at 47s per task and an impressive 96.6% task reliability—Percentage of tasks an agent successfully completes at least once when given multiple attempts
- Our evaluation of [Browser-Use](https://github.com/browser-use/browser-use) shows 77.3% self-reported agent performance and 60.2% LLM-verified success, compared to their reported 89% in [their blog post](https://browser-use.com/posts/sota-technical-report). Their results files are not available for verification.
- [Convergence](https://github.com/convergence-ai/proxy-lite) achieved 38.4% agent success and 31.4% evaluation success, lower than other methods. Performance was limited by CAPTCHA and bot detection. Notably, it showed strong self-awareness, with near-perfect alignment in some cases, suggesting potential if detection issues are addressed.

PS: [We are actively hiring software and research engineers](https://nottelabs.notion.site/jobs-for-humans) 🪩

# The metrics

[In the main table](#opensource-operators-evals)

- `Agent Self-Report` The success rate reported by the agent itself across all tasks. This reflects the agent's internal confidence in its performance.
- `LLM Evaluation` The success rate determined by GPT-4 using WebVoyager's evaluation prompt as a judge evaluator, assessing the agent's actions and outputs. This provides an objective measure of task completion.
- `Time per Task` The average execution time in seconds for the agent to attempt and complete a single task. This indicates the efficiency and speed of the agent's operations.
- `Task Reliability` The percentage of tasks the agent successfully completed at least once across multiple attempts (8 in this benchmark). This metric highlights the agent's ability to handle a diverse set of tasks given sufficient retries, indicating system robustness.

[In the breakdowns](#breakdowns)

- `Alignment` Ratio of Agent Self-Report to LLM Evaluation, indicating overestimation (>1.0) or underestimation (<1.0) by the agent. Being close to 1 or <1.0 is typically better.
- `Mismatch` Counts the specific instances where the agent claimed success but the evaluator disagreed. This reveals how often the agent incorrectly assessed its own performance.

# Blog post

The race for open-source web agents is heating up, leading to very bold statements being thrown around. We cut through the noise and bring a fully transparent and reproducible benchmark to get a sense of the curent scene. Everything is open, inviting you to see exactly how different systems perform—and perhaps prompting a closer look at other's claims ;)

| Rank | Provider                                                    | Agent Self-Report | LLM Evaluation | Time per Task | Task Reliability |
| ---- | ----------------------------------------------------------- | ----------------- | -------------- | ------------- | ---------------- |
| 🏆   | [Notte](https://github.com/nottelabs/notte)                 | **86.2%**         | **79.0%**      | **47s**       | **96.6%**        |
| 2️⃣   | [Browser-Use](https://github.com/browser-use/browser-use)   | 77.3%             | 60.2%          | 113s          | 83.3%            |
| 3️⃣   | [Convergence](https://github.com/convergence-ai/proxy-lite) | 38.4%             | 31.4%          | 83s           | 50%              |

Results are averaged over tasks and then over 8 separate runs to account for the high variance inherent in web agent systems. In our benchmarks, each provider ran each task 8 times using the same configuration, headless mode, and strict limits: 6 minutes or 20 steps maximum—because no one wants an agent burning 80 steps to find a lasagna recipe. Agents had to handle execution and failures autonomously.

# The dataset

WebVoyager is a dataset of ~600 tasks for web agents. Example:

```
task: Book a journey with return option on same day from Edinburg to Manchester for Tomorrow, and show me the lowest price option available
url: https://www.google.com/travel/flights
```

An agent navigates the site and returns a success status and an answer. Relying on the agent’s self-reported success is unreliable, as agents may misjudge task completion. WebVoyager addresses this with an independent LLM evaluator that judges success based on agent actions and screenshots.

### The challenge of high variance

Beyond known limitations like outdated web content, a key issue is the high variance in agent performance. These systems, powered by non-deterministic LLMs and operating on a constantly changing web, often yield inconsistent results. Reasoning errors, execution failures, and unpredictable network behavior make single-run evaluations unreliable. To counter this, we propose to run each task multiple times for a much more accurate view—averaging results helps smooth out randomness and gives a more statistically sound estimate of performance.

### WebVoyager30

To reduce variance and improve reproducibility, we sampled [WebVoyager30](eval/data/webvoyager/webvoyager_simple.jsonl)—a 30-task subset across 15 diverse websites. It retains the full dataset’s complexity while enabling practical multi-run evaluation, offering a more reliable benchmark for the community.

Running 30 tasks × 8 times (240 runs total) is far more informative than running 600 tasks once, as it averages out randomness and provides a statistically sounder view of performance. Running all 600 tasks 8× would be ideal but is often impractical due to compute costs and time, making fast and accessible reproduction difficult.

The selected tasks are neither trivial nor overly complex—they reflect the overall difficulty of the full dataset, making this a reasonable and cost-effective proxy.

# Breakdowns

Benchmark results breakdown for each provider.

## Notte

```
Provider: notte
Version: [v1.3.3](https://github.com/nottelabs/notte/releases/tag/v1.3.3)
Reasoning: gemini/gemini-2.0-flash
```

Notte leads the benchmark with 86.2% self-reported success and 79% LLM-verified completion, along with the fastest execution time at 47s per task and an impressive 96.6% task reliability. It shows consistent performance, with self-assessments slightly overestimating results. Alignment ratios range from 0.960 to 1.183, with low mismatch counts (mostly 3). Task times are really efficient (45-51s), and run 1743001170-7 achieved near-perfect alignment at 0.960.0.

| Runs                                          | Agent Self-Report | LLM Evaluation | Alignment | Mismatch | Time per Task |
| --------------------------------------------- | ----------------- | -------------- | --------- | -------- | ------------- |
| [1743001170-0](WebVoyager30/Notte/1743001170) | 0.929             | 0.857          | 1.084     | 3        | 47s           |
| [1743001170-3](WebVoyager30/Notte/1743001170) | 0.867             | 0.767          | 1.130     | 3        | 50s           |
| [1743001170-4](WebVoyager30/Notte/1743001170) | 0.867             | 0.800          | 1.084     | 3        | 51s           |
| [1743001170-6](WebVoyager30/Notte/1743001170) | 0.867             | 0.733          | 1.183     | 4        | 45s           |
| [1743001170-1](WebVoyager30/Notte/1743001170) | 0.862             | 0.759          | 1.136     | 3        | 47s           |
| [1743001170-7](WebVoyager30/Notte/1743001170) | 0.857             | 0.893          | 0.960     | 1        | 47s           |
| [1743001170-2](WebVoyager30/Notte/1743001170) | 0.828             | 0.759          | 1.091     | 2        | 45s           |
| [1743001170-5](WebVoyager30/Notte/1743001170) | 0.821             | 0.750          | 1.095     | 3        | 49s           |

## Browser-Use

```markdown
Provider: Browser-Use
Version: [v0.1.40](https://github.com/browser-use/browser-use/releases/tag/0.1.40)
Reasoning: openai/gpt-4o
```

[Browser-Use](https://browser-use.com/posts/sota-technical-report) reports 89% success on WebVoyager. In our evaluation, we were unable to reproduce this result using WebVoyager30 with multiple retries and the full dataset in a single run. We tested different agent and browser configurations, as well as more lenient interpretations of ambiguous outcomes. Browser-Use showed alignment ratios between 1.2 and 1.534, indicating 20–50% overestimation relative to LLM-verified success, and 5–8 mismatches between self-reported and verified outcomes.

| Runs                                               | Agent Self-Report | LLM Evaluation | Alignment | Mismatch | Time per Task |
| -------------------------------------------------- | ----------------- | -------------- | --------- | -------- | ------------- |
| [1743016360-6](WebVoyager30/BrowserUse/1743016360) | 0.833             | 0.667          | 1.249     | 7        | 98s           |
| [1743016360-4](WebVoyager30/BrowserUse/1743016360) | 0.815             | 0.667          | 1.222     | 5        | 119s          |
| [1743016360-1](WebVoyager30/BrowserUse/1743016360) | 0.808             | 0.577          | 1.400     | 7        | 127s          |
| [1743016360-5](WebVoyager30/BrowserUse/1743016360) | 0.800             | 0.600          | 1.333     | 6        | 95s           |
| [1743016360-2](WebVoyager30/BrowserUse/1743016360) | 0.786             | 0.679          | 1.158     | 5        | 132s          |
| [1743016360-7](WebVoyager30/BrowserUse/1743016360) | 0.767             | 0.500          | 1.534     | 8        | 105s          |
| [1743016360-3](WebVoyager30/BrowserUse/1743016360) | 0.708             | 0.542          | 1.306     | 5        | 113s          |
| [1743016360-0](WebVoyager30/BrowserUse/1743016360) | 0.667             | 0.583          | 1.144     | 2        | 118s          |

## Convergence

```markdown
Provider: Convergence
Version: [a4389c5](https://github.com/convergence-ai/proxy-lite/commit/a4389c599d5f5f77dc18510c879e2e783434766b)
Reasoning: Convergence Proxy-lite
```

[Convergence Proxy-lite](https://github.com/convergence-ai/proxy-lite) achieved 38.4% agent success and 31.4% LLM-verified success, lower than other evaluated systems. These results were affected by frequent triggers of Google’s CAPTCHA and bot detection. In one run, the system achieved perfect alignment (1.000) with zero mismatches between self-reported and verified outcomes, indicating accurate self-assessment under certain conditions. This suggests that with improved bot detection handling, Convergence would likely outperform Browser-Use due to its superior self-awareness and calibration.

| Runs                                                | Agent Self-Report | LLM Evaluation | Alignment | Mismatch | Time per Task |
| --------------------------------------------------- | ----------------- | -------------- | --------- | -------- | ------------- |
| [1743114165-6](WebVoyager30/Convergence/1743114165) | 0.483             | 0.345          | 1.400     | 4        | 77s           |
| [1743114165-0](WebVoyager30/Convergence/1743114165) | 0.407             | 0.333          | 1.222     | 2        | 85s           |
| [1743114165-3](WebVoyager30/Convergence/1743114165) | 0.393             | 0.286          | 1.374     | 3        | 82s           |
| [1743114165-4](WebVoyager30/Convergence/1743114165) | 0.379             | 0.345          | 1.099     | 2        | 82s           |
| [1743114165-5](WebVoyager30/Convergence/1743114165) | 0.379             | 0.276          | 1.373     | 3        | 84s           |
| [1743114165-7](WebVoyager30/Convergence/1743114165) | 0.367             | 0.333          | 1.102     | 3        | 84s           |
| [1743114165-2](WebVoyager30/Convergence/1743114165) | 0.357             | 0.286          | 1.248     | 3        | 86s           |
| [1743114165-1](WebVoyager30/Convergence/1743114165) | 0.310             | 0.310          | 1.000     | 0        | 84s           |

# Config

We conducted our evaluation on a Macbook M1 machine using Python 3.11, with our IP address located in a residential area in Switzerland, which explains the presence of some German screenshots in our findings. This IP location triggers cookie consent popups that make task completion more challenging for the agents.

# Conclusion

Our open-source agent evaluation reveals notable differences between reported and observed performance. While Notte shows strong capabilities and good self-awareness, other systems exhibit issues with reproducibility and self-assessment. These results underscore the importance of clear, reproducible benchmarks. We encourage collaboration from the research and engineering community to develop improved trusted evaluation standards.

# Reproduce evals

We believe in full transparency. Our entire evaluation is open source, accessible, and reproducible. You can verify every claim by reviewing execution trajectories, agent reasoning, outputs, and evaluations. Replay runs, explore the data, and confirm our findings yourself.

To reproduce the benchmark results yourself, simply run for a given config:

```
cat configs/notte.headless.gemini | uv run python -m eval.run
```

This will produce results, that are organized hierarchically:

```
dataset/
└── provider/              # e.g., Notte, BrowserUse, Convergence
    └── timestamp/         # Unique identifier for each evaluation run
        └── task_name/     # Individual task directory
            ├── results.json
            ├── results_no_screenshot.json
            └── summary.webp
```

## Costs

Running `WebVoyager30` with `tries_per_task = 8` will costs you $0 for `notte` with Gemini, $0 for `convergence` with Proxy-lite, and approximately $20 total for BrowserUse with GPT-4o. This cost comes from processing 7.4M input tokens at $2.5/1M tokens ($18.53) plus 145K output tokens at $10/1M tokens ($1.45).

# Replays

You can access any replay just looking at the files; (This is a [@notte](https://github.com/nottelabs/notte) example)

```
task: Look up a model with a license of cc-by-sa-4.0 with the most likes on Hugging face
replay: WebVoyager30/Notte/1743001170/Huggingface_webvoyager--Huggingface--3/0/summary.webp
results: WebVoyager30/Notte/1743001170/Huggingface_webvoyager--Huggingface--3/0/results_no_screenshot.json
```

![summary](WebVoyager30/Notte/1743001170/Huggingface_webvoyager--Huggingface--3/0/summary.webp)
