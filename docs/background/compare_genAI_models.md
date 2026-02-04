# Comparison of generative AI models

## Caution

We miss a good Danish or multilingual instruction following benchmark and for the more specialized benchmarks we need 
to have all the models, that we like to compare, evaluated on that benchmark. 

With the simpler benchmarks that the models, we like to compare, already have been evaluated at, they are seldom of
much use to us. 
It is kind of similar to evaluating different fruits by comparing how good the fruit is for hitting bullseye at a 
30-yards distance and then hoping that will somehow translate into how well consumers will like the fruit.
 
As a concrete example, lets take eg. the ["angry tweets"-task](https://euroeval.com/datasets/danish/#angry-tweeets) 
from the EuroEval Danish benchmark. 
Here the LLMs are asked to determine the sentiment of a tweet after having had 12 examples (using few-shot-learning
prompting philosophy). It could look something like:

```text
Følgende er tweets og deres sentiment, som kan være 'positiv', 'neutral' eller 'negativ'.

Tweet: Jeg tror, det der var kampen. Goff virker lost
Sentiment: negativ

Tweet: @USER @USER Vi bruger også snildt 1-2 timer (nogle gange flere timer end det) på at putte den yngste. Det er oftest Tommi, som gør det, for jeg går helt amok i processen. Så smører jeg madpakker og rydder op i stedet.
Sentiment: neutral

```

and so on with another 10 examples until finally:

```text
Tweet: Er du nysgerrig på, hvordan du diskvalificerer dig selv fra at blive taget seriøst i den offentlige debat? Naser har svaret. #dkpol #dkmedier [LINK]

Klassificer sentimentet i tweetet. Svar kun med 'positiv', 'neutral' eller 'negativ'.
```

and following the annotation from the dataset it is known that the LLM must answer "negativ" in order to succeed at this
question on the ["angry tweets"-task](https://euroeval.com/datasets/danish/#angry-tweeets).

Depending on your intended usecase a task like this (or any other Benchmark task) might be relevant or not so relevant
and that is the nature of benchmarks.

Finally also to note, a lot of benchmarks exist and so it is quite rare that models are actually evaluated on the same benchmark 
(or same version of the benchmark). There might even be differences on how they are evaluated on the same benchmark. 


## Model comparison

Anyway with that very serious word of caution. We can compare some models:

|                                           | Mistral 3.2 24B | Olmo 3.1 32B    | Qwen3 Next 80B A3B |
|-------------------------------------------|-----------------|-----------------|--------------------|
| Rank EuroEval Danish (better closer to 1) | 1.48            | 2.17            | 1.73               |
| European values alignment                 | N/A             | 74% (39% ~ 86%) | 2% ~ 65%           |
| MOF (Model openness - 1 is best)          | 3               | 1               | 3                  |
| MMLU-Pro                                  | [69.06%](https://huggingface.co/mistralai/Mistral-Small-3.2-24B-Instruct-2506) |                 | [82.7](https://huggingface.co/Qwen/Qwen3-Next-80B-A3B-Thinking)|
| IFEval                                    | [82.9](https://llm-stats.com/models/compare/mistral-small-24b-instruct-2501-vs-qwen3-next-80b-a3b-instruct) | [88.8](https://allenai.org/blog/olmo3)        | [87.6](https://llm-stats.com/models/compare/mistral-small-24b-instruct-2501-vs-qwen3-next-80b-a3b-instruct) |
| GPQA | [55.5%](https://artificialanalysis.ai/evaluations/gpqa-diamond?models=deepinfra_olmo-3-1-32b-instruct%2Cgpt-oss-120b%2Cgpt-oss-20b%2Cllama-4-maverick%2Cgemini-3-pro%2Cmistral-small-3-2%2Cfalcon-h1r-7b%2Colmo-3-1-32b-instruct%2Cqwen3-next-80b-a3b-instruct) | [53.9%](https://artificialanalysis.ai/evaluations/gpqa-diamond?models=deepinfra_olmo-3-1-32b-instruct%2Cgpt-oss-120b%2Cgpt-oss-20b%2Cllama-4-maverick%2Cgemini-3-pro%2Cmistral-small-3-2%2Cfalcon-h1r-7b%2Colmo-3-1-32b-instruct%2Cqwen3-next-80b-a3b-instruct) | [73.8%](https://artificialanalysis.ai/evaluations/gpqa-diamond?models=deepinfra_olmo-3-1-32b-instruct%2Cgpt-oss-120b%2Cgpt-oss-20b%2Cllama-4-maverick%2Cgemini-3-pro%2Cmistral-small-3-2%2Cfalcon-h1r-7b%2Colmo-3-1-32b-instruct%2Cqwen3-next-80b-a3b-instruct)


## Rankings from the LLM arena

We can also get an idea about their performances by looking to the [LLM Arena](https://arena.ai/da/leaderboard) where 
model evaluation are crowdsourced by comparing outputs from two models and prefering one over the other 
(_retrieved 2026-02-04_ Rank 1 means you are the best):

|                            | Mistral 3.1 24B | Olmo 3.1 32B | Qwen3 Next 80B A3B |
|----------------------------|-----------------|--------------|--------------------|
| Overall rank               | 170             | 141          | 60                 |
| Rank coding                | 142             | 119          | 56                 |
| Rank Creative Writing      | 163             | 150          | 112                |
| Rank Instruction following | 156             | 127          | 70                 |
| Rank Longer Queries        | 151             | 131          | 73                 |
| Rank English               | 168             | 119          | 62                 |
| Rank German                | 129             | 142          | 58                 |
| Rank French                | 133             | 99           | 66                 |
| Rank Spanish               | 126             | 89           | 29                 |
| Rank Russian               | 152             | 159          | 62                 |

Another word of caution, since neither of these models compares the flagship models, they probably almost always loose, 
so I expect the rankings out of top 10, or something like that, to be doubious. I have not looked into this, so they 
might account for this in some clever way.


## Performance on possible hardware-specific setups

In addition to the benchmark results above, it is also very important how fast it is possible to provide inference at 
a given hardware setup. 
Especially the time-to-first-token (TTFT), that is generally speaking how long it takes to interpret a prompt/message,
and the tokens-per-seconds (TPS), which signifies how fast a response will be appearing on the users end. 
For details on the metrics see [Nvidea's NIM LLMs Benchmarking](https://docs.nvidia.com/nim/benchmarking/llm/latest/metrics.html) 

Related the possible amount of context that can be allocate on a given hardware setop is also important. 
Eg. single experiments it seems that 32K token context windows is a minimum for serving agentic RAG solutions (using 
the [smolagent](https://huggingface.co/docs/smolagents/index) framework).   

Here especially two hardware setups are relevant:
- NVidea A100 80GB GPU setup available through Computerome
- NVIDIA RTX 6000 Blackwell Max-Q 96GB GPU setup available through [Hetzner](https://www.hetzner.com/dedicated-rootserver/gex131/) 

For a back-of-envelope calculation see [AarhusAI-RAG / infrastructure: Opsummering af teoretisk sammenligning mellem eksisterende AarhusAI og alternativ](https://github.com/AarhusAI-RAG/infrastructure/blob/main/docs/compare_A100-mistral24B_vs_RTXpro6000-qwen80b.md) 