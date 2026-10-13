# Strix Halo Benchmark Results: kernel 7.2 vs 7.3.0-rc5 (PerfOpt)
Distro: OKO v0.2. 
Device: GMKtec EVO-X2 powered by AMD Ryzen™ AI Max+ 395.  
GPU: AMD Radeon 8060S Graphics (RADV STRIX_HALO)  
Benchmark: `llama-bench`.  
Settings: `-ngl 999 -b 256 -ub 256 -p 2048 -n 128`.  
Measures: Prompt Processing (PP) and Token Generation (TG).  
Kernels: linux kernel 7.2 vs 7.3.0-rc5 (PerfOpt disabled) vs 7.3.0-rc5 (PerfOpt enabled)

## Results

| Model | PP PerfOpt=0 (t/s) | PP PerfOpt=1 (t/s) | PP Linux 7.2 (t/s) | TG PerfOpt=0 (t/s) | TG PerfOpt=1 (t/s) | TG Linux 7.2 (t/s) |
|---|---:|---:|---:|---:|---:|---:|
| Qwen3-Coder-Next 80B.A3B Q4_K_M | 538.24 | 537.47 | 535.50 | 59.55 | 61.78 | 62.00 |
| GPT-OSS 20B Q8_0 | 1356.52 | 1354.60 | 1343.31 | 76.69 | 77.26 | 77.35 |
| Mistral-Small-24B-Instruct-2501-Q8_0 | 387.06 | 387.77 | 389.02 | 9.05 | 9.06 | 9.10 |
| Qwen 27B Q8_0 | 330.78 | 332.36 | 334.69 | 7.50 | 7.52 | 7.55 |
| MiniCPM4 8B Q4_K_M | 1132.44 | 1126.55 | 1126.88 | 43.32 | 43.62 | 43.86 |
| Qwen3.5 4B Q4_K_M | 2041.02 | 2048.39 | 2055.68 | 63.14 | 64.48 | 64.53 |
| Qwen3 14B Q4_K_M | 720.70 | 723.05 | 731.07 | 25.32 | 25.39 | 25.48 |

## Test configurations

- `PerfOpt=0` and `PerfOpt=1`: `linux-next` (`7.3.0-rc5-next-20261002`), with `amdgpu.iommu_perfopt=0` and `amdgpu.iommu_perfopt=1`, respectively.
- `Linux 7.2`: separate kernel run; not used as a baseline for the PerfOpt comparison.
- PP: prompt processing throughput, in tokens/second.
- TG: Token generation throughput, in tokens/second.
- Table values are the central throughput figures from the supplied `llama-bench` logs; the reported `±` variation values are omitted.

## Example
```
oko-server:~$ oko-dl https://huggingface.co/DavidAU/Qwen3.8-27B-TWIN-TURBO-Fable-Cold-Fusion-709-L-Uncensored-NM-DAU-NEO-MTP-GGUF
oko-server:~$ llama-bench -m models/DavidAU___Qwen3.8-27B-TWIN-TURBO-Fable-Cold-Fusion-709-L-Uncensored-NM-DAU-NEO-MTP-GGUF/Qwen3.8-27B-TTURBO-Fable-C-Fusion-709-L-Uncen-TOOLS-NM-DAU-NEO-MAX-MTP-Q8_0.gguf   -ngl 999   -b 256   -ub 256   -p 2048   -n 128
ggml_vulkan: Found 1 Vulkan devices:
ggml_vulkan: 0 = AMD Radeon 8060S Graphics (RADV STRIX_HALO) (radv) | uma: 1 | fp16: dot2 | bf16: 0 | fp4: 0 | warp size: 64 | shared memory: 65536 | int dot: 1 | matrix cores: KHR_coopmat
| model                          |       size |     params | backend    | ngl | n_batch | n_ubatch |            test |                  t/s |
| ------------------------------ | ---------: | ---------: | ---------- | --: | ------: | -------: | --------------: | -------------------: |
| qwen35 27B Q8_0                |  28.15 GiB |    27.32 B | Vulkan     | 999 |     256 |      256 |          pp2048 |        330.85 ± 2.16 |
| qwen35 27B Q8_0                |  28.15 GiB |    27.32 B | Vulkan     | 999 |     256 |      256 |           tg128 |          7.55 ± 0.00 |
```
