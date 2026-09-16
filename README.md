# OKO Model Configs

Tested, ready-to-use model configurations for the **OKO OS model runner**.

These configs are provided as working presets for different models. They include the recommended `llama-server` launch parameters and OKO runner settings for each model.

## Usage

1. Find the model you want to run.
2. Open its YAML config.
3. Copy the configuration.
4. Paste it into your OKO model configuration.
5. Run the model.

That's it.

Example:

```yaml
drts:
  prio: 50
  alias:
    - sf_organic

  cmd: >-
    llama-server
    -m models/model.gguf
    -fa on
    -np 1

  think: on
  reasoning-effort: high
```

## About the configs

Configs in this repository are:

* tested with OKO OS;
* ready to copy and paste;
* tuned for the specified model;
* configured with appropriate runtime and generation parameters.

Where a model requires special settings such as MTP/speculative decoding, multimodal support, reasoning configuration, or specific `llama.cpp` options, they are already included in its config.

Unless you know what you are changing, start with the config as provided.

## Requirements

These configurations are intended for the **OKO OS** https://oko.satisfanly.com.

Model weights are not included in this repository and can be downloaded using *oko-dl* utility.
