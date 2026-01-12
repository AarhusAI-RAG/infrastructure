# Opsummering af teoretisk sammenligning mellem eksisterende AarhusAI og alternativ

## AarhusAI

- GPU: A100 Ampere-generation
  - Memory: 80 GB
  - Bandwidth: 2 TB/s
  - Compute: 624 TOPS
  - Native fp16, dvs. hvert parameter fylder 2 bytes
- LLM: Mistral Small 3.2 24B
  - 24 milliarder parametre i alt
  - 24 milliarder parametre aktiveres for hvert token (dense)
  - Grouped-query attention: ca. 1.25 GB VRAM for 8k context

# Alternativ
- GPU: RTX PRO 6000 Blackwell Max-q
  - Memory: 96 GB (20% mere end A100)
  - Bandwidth 1.8 TB/s (10% mindre end A100)
  - Compute: 3511 TOPS (462% mere end A100)
  - Native nvfp4, dvs. hvert parameter fylder ½ byte (4 bit)
- LLM: Qwen3-Next-80B-A3B
  - 80 milliarder parametre i alt
  - 3.9 milliarder vigtigste parametre aktiveres for hvert token (MoE)
  - Hybrid Gated DeltaNet attention: ca. 0.2 GB VRAM for 8k context

## Bandwidth bottleneck:

- AarhusAI: 2.000 / (24 * 2) = ca. 42 tok/s
- Alternativ: 1800 / (3.9 * 0.5) = ca. 923 tok/s (2097% mere)

## Concurrency bottleneck:

- AarhusAI: (80 - 24 * 2) / 1.25 = ca. 25 samtidige sessions
- Alternativ: (96 - 80 * 0.5) / 0.2 = ca. 280 samtidige sessions

## Uafhængige benchmark-resultater:

- GPQA (Google-proof Q&A, higher is better)
  - Mistral: 46.1%
  - Qwen3: 77.2% (57.6% færre fejl)
  - Gemini 3 Pro: 90.8% (#1)
- Artificial Analysis Intelligence (higher is better)
  - Mistral: 15
  - Qwen3: 24 (60% “klogere”)
  - GPT-5.2: 51 (#1)
- LMarena (lower is better)
  - Mistral: #96
  - Qwen3: #57
  - Gemini 3 Pro: #1
- EuroEval Danish (lower is better)
  - Mistral: #19
  - Qwen3: #38 (50% ringere placering, kan ikke finde aggregeret score)
  - Claude Sonnet 4.5: #1

## PRIS (!!!):
- AarhusAI: ?? kr./mnd (ved Computerome)
- Alternativ: 6.700 kr./mnd (ved Hetzner siden 11. december)

## Disclaimer
Husk, at det her er simple teoretiske beregninger uden medtaget overhead, og bottlenecks er ikke det samme som endelig performance, men det er typiske øvre grænser i klassiske arkitekturer.
Compute (TOPS) er endnu en grænse, og det er det også, at nogle af de nye teknologier, de anvender, måske ikke kan udnyttes helt godt nok endnu i alle frameworks. Realistisk gætter jeg på 5-10 x tokens/sek, men det bliver spændende at se.

Bemærk derudover det lave teoretiske VRAM-aftryk for Qwens kv-cache (hvis jeg har regnet rigtigt), der skyldes deres hybrid attention. Den er designet til lang context på alle ledder og kanter. Skru den op til 256k og du kan give den en bog, eller hold den på 32k og behold solid concurrency.

## Tests på Ada 20 GB GPU

- ### forhåndstestet mistral3.2 vs. qwen3-30b-a3b
  (bemærk 30, ikke 80, både med og uden VL) begge i Q4 på mit Ada 20 GB, mistral3.2 kan være 100% i memory, qwen3 offloades delvist til CPU med overhead til følge = alligevel outputter qwen3 konsekvent tokens ca. 5 gange så hurtigt (bemærk, de har begge q4 effekten, så mistral er her relativt langt hurtigere end den ville have været i fp16, hvis jeg havde plads på gpu)
- ### qwen3-next-80b-a3b
  i Q4 som overhovedet ikke kan være i VRAM, så der er KÆMPE overhead, hvilket resulterer i lang prefill (lang tid til first token - det er bl.a. det som blackwell-gpu'ens x5 compute vs a100'eren hjælper på!), men når den først går i gang, er outputter den stadig hurtigere end mistral
