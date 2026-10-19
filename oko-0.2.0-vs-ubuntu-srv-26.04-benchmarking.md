# Strix Halo Benchmark Results: OKO v0.2.0 vs Ubuntu Server 26.04.1 LTS

## Platform

- **Device:** GMKtec EVO-X2
- **APU:** AMD Ryzen™ AI Max+ 395
- **GPU:** AMD Radeon 8060S Graphics (RADV STRIX_HALO)
- **Benchmark:** `llama-bench`
- **Common settings:** `-ngl 999 -b 256 -ub 256 -p 2048 -n 128`
- **Ubuntu:** Ubuntu Server 26.04.1 LTS; `llama.cpp-tools` / `libllama0` package version `8681+dfsg-1`; Mesa Vulkan driver `26.0.8-1ubuntu0.3`
- **OKO:** OKO v0.2.0; Linux 7.2 results shown below
- **Metric:** PP = prompt processing (tokens/s); TG = token generation (tokens/s)

> **Comparison method:** the table uses the model names from the [OKO v0.2.0 results (linux kernel 7.2)](./oko-llm-benchmarking.md) and places the corresponding Ubuntu measurements alongside them. Percentages are calculated as `(OKO / Ubuntu - 1) × 100`.

## Results

Positive percentage means OKO is faster; negative means Ubuntu is faster.

| Model | Ubuntu PP (t/s) | OKO PP (t/s) | OKO vs Ubuntu PP | Ubuntu TG (t/s) | OKO TG (t/s) | OKO vs Ubuntu TG |
|---|---:|---:|---:|---:|---:|---:|
| Qwen3-Coder-Next 80B.A3B Q4_K_M | 471.35 | 535.50 | **+13.6%** | 57.33 | 62.00 | **+8.1%** |
| GPT-OSS 20B Q8_0 | 1089.97 | 1343.31 | **+23.2%** | 74.90 | 77.35 | **+3.3%** |
| Mistral-Small-24B-Instruct-2501-Q8_0 | 337.76 | 389.02 | **+15.2%** | 8.78 | 9.10 | **+3.6%** |
| Qwen 27B Q8_0 | 294.25 | 334.69 | **+13.7%** | 7.35 | 7.55 | **+2.7%** |
| MiniCPM4 8B Q4_K_M | 947.93 | 1126.88 | **+18.9%** | 40.26 | 43.86 | **+8.9%** |
| Qwen3.5 4B Q4_K_M | 1859.88 | 2055.68 | **+10.5%** | 63.33 | 64.53 | **+1.9%** |
| Qwen3 14B Q4_K_M | 572.57 | 731.07 | **+27.7%** | 24.40 | 25.48 | **+4.4%** |

## Summary   

- **PP:** OKO is faster in all seven listed comparisons, by approximately **10.5% to 27.7%**. This matters most when processing long prompts, large codebases, RAG context, or documents.
- **TG:** OKO is faster in all seven listed comparisons, by approximately **1.9% to 8.9%**. The TG advantage is smaller than the PP advantage in these results.
- **Largest observed PP difference:** Qwen3 14B, **+27.7%** for OKO.
- **Largest observed TG difference:** MiniCPM4 8B, **+8.9%** for OKO.

## Example  
```
$ wget -c https://huggingface.co/DavidAU/Qwen3.8-27B-TWIN-TURBO-Fable-Cold-Fusion-709-L-Uncensored-NM-DAU-NEO-MTP-GGUF/resolve/main/Qwen3.8-27B-TTURBO-Fable-C-Fusion-709-L-Uncen-NM-DAU-NEO-MAX-Q8_0.gguf

$ llama-bench   -m Qwen3.8-27B-TTURBO-Fable-C-Fusion-709-L-Uncen-NM-DAU-NEO-MAX-Q8_0.gguf  -ngl 999   -b 256   -ub 256   -p 2048   -n 128
ggml_vulkan: Found 1 Vulkan devices:
ggml_vulkan: 0 = Radeon 8060S Graphics (RADV STRIX_HALO) (radv) | uma: 1 | fp16: 1 | bf16: 0 | warp size: 64 | shared memory: 65536 | int dot: 1 | matrix cores: KHR_coopmat
load_backend: loaded Vulkan backend from /usr/lib/x86_64-linux-gnu/ggml/backends0/libggml-vulkan.so
load_backend: loaded CPU backend from /usr/lib/x86_64-linux-gnu/ggml/backends0/libggml-cpu-zen4.so
| model                          |       size |     params | backend    | ngl | n_batch | n_ubatch |            test |                  t/s |
| ------------------------------ | ---------: | ---------: | ---------- | --: | ------: | -------: | --------------: | -------------------: |
| qwen35 27B Q8_0                |  27.73 GiB |    26.90 B | Vulkan     | 999 |     256 |      256 |          pp2048 |        294.25 ± 0.48 |
| qwen35 27B Q8_0                |  27.73 GiB |    26.90 B | Vulkan     | 999 |     256 |      256 |           tg128 |          7.35 ± 0.00 |
```
