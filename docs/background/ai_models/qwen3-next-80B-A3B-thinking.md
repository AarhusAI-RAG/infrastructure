# Qwen3-Next-80B-A3B-Thinking

Generative model (Thinking fashion) with mixture of expert (MoE) design with only 3B active parameters at inference time.

- see [huggingface model card](https://huggingface.co/Qwen/Qwen3-Next-80B-A3B-Thinking)
- alt see [Qwen blog post](https://qwen.ai/blog?id=4074cca80393150c248e508aa62983f9cb7d27cd)

From [EuroEval danish leaderboard](https://euroeval.com/leaderboards/Monolingual/danish/) it gets
- an overall rank of 1.73 (the closer to one the better) 
  - a placement as number 40 on the leaderboard (as of 4 February 2026)
- Qwen3-Next-80B-A3B-Thinking have not be scored on the European values alignment benchmark, but comparable models from 
  Qwen gets alignment scores varying from 2% to 65%  

Results from [AI-arenaen](https://ai-arenaen.dk/) is not yet available.

Using the [Linux Foundation](https://lfaidata.foundation/) [Model Openness framework (MOF)](https://isitopen.ai/)
Qwen 3 next is deemed to be a class 3 open model (an "open weights model") (class 1 is best/most open). 
[Alternative take/additional info on model openess from Mozilla](https://assets.mofoprod.net/network/documents/Towards_a_Framework_for_Openness_in_Foundation_Models.pdf).
[Another alternative from the OpenSource Initiative](https://opensource.org/ai)

The developers of Qwen are based in China

## Instruct model

[Qwen3-Next-80B-A3B-Instruct](https://huggingface.co/Qwen/Qwen3-Next-80B-A3B-Instruct) is also available, but have not
been evaluated on the euroeval benchmark