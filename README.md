# Setup

Install Ollama from https://ollama.com

## Pull Models

```bash
ollama pull llama3.2:3b
ollama pull mistral
ollama pull deepseek-r1:8b
ollama pull qwen2.5:7b
ollama pull phi3:mini
```

## Install Python Dependencies

```bash
pip install -r requirements.txt
```

# Metrics Reference

This section explains fields in `metrics_few_shot.json`.

## Top-Level Metadata

- `model`: Model name used.
- `prompt_variant`: Prompt template used, for example `few_shot`.
- `generated_at`: Timestamp when metrics were written.
- `dataset_file`: Dataset path used for the run.
- `results_file_json`: Per-sample JSON output file.
- `results_file_csv`: Per-sample CSV output file.

## Performance

- `total_runtime_seconds`: Total wall-clock time for this model+prompt run.
- `processed_this_run`: Number of samples newly inferred in this run.
- `skipped_this_run`: Samples skipped because they already existed in results.
- `total_samples_in_file`: Total rows present in results after run.
- `throughput_samples_per_second`: `processed_this_run / total_runtime_seconds`.
- `success_count`: Samples with no request error.
- `error_count`: Samples with request error.
- `success_rate`: `success_count / total_samples_in_file`.
- `error_rate`: `error_count / total_samples_in_file`.

### Latency Stats (`latency_seconds`)

- `count`: Number of latency samples.
- `mean`: Average latency.
- `median`: Median latency.
- `min`: Fastest sample latency.
- `max`: Slowest sample latency.
- `p95`: 95th percentile latency.
- `p99`: 99th percentile latency.

### Token Stats (`tokens`)

- `prompt_eval_count_sum`: Sum of prompt tokens across samples.
- `eval_count_sum`: Sum of completion tokens across samples.
- `prompt_eval_count_avg`: Average prompt tokens per sample.
- `eval_count_avg`: Average completion tokens per sample.
- `eval_duration_seconds_avg`: Average model eval duration.

### Resource Stats (`resources`)

- `cpu_percent`: Distribution stats of per-sample CPU percent.
- `ram_mb_avg`: Average sampled RAM in MB.
- `ram_mb_peak`: Peak process RAM in MB.
- `gpu_util_percent`: Distribution stats from `nvidia-smi` GPU utilization samples.
- `vram_used_mb`: Distribution stats from `nvidia-smi` VRAM usage samples.
- `gpu_power_watts`: Distribution stats from `nvidia-smi` power samples.

## Quality

- `json_validity_rate`: Fraction of samples where response could be parsed as JSON payload.
- `schema_validity_rate`: Fraction where predicted operations exist and each op matches schema.

Schema used:
- `action` must be `add` or `remove`.
- `product` must be a string.
- `quantity` must be an integer.

- `operation_precision`: `TP / (TP + FP)` at operation level.
- `operation_recall`: `TP / (TP + FN)` at operation level.
- `operation_f1`: Harmonic mean of operation precision and recall.
- `full_sample_exact_match_ordered_rate`: Fraction where predicted operation list exactly equals ground truth list in the same order.
- `full_sample_exact_match_unordered_rate`: Fraction where operation multisets match ignoring order.
- `operation_count_exact_match_rate`: Fraction where predicted operation count equals ground-truth count.
- `operation_count_mae`: Mean absolute error of operation count.
- `action_accuracy`: Field-wise action match rate at aligned positions.
- `product_accuracy`: Field-wise product match rate at aligned positions.
- `quantity_accuracy`: Field-wise quantity match rate at aligned positions.
- `tp`: Total true positive operations.
- `fp`: Total false positive operations.
- `fn`: Total false negative operations.