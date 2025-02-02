# Bias Detection Scanner

This scanner is designed to inspect the outputs generated by Language Learning Models (LLMs) to detect and evaluate
potential biases. Its primary function is to ensure that LLM outputs remain neutral and don't exhibit unwanted or
predefined biases.

## Attack scenario

In the age of AI, it's pivotal that machine-generated content adheres to neutrality. Biases, whether intentional or
inadvertent, in LLM outputs can be misrepresentative, misleading, or offensive. The `Bias` scanner serves to address
this by detecting and quantifying biases in generated content.

## How it works

The scanner utilizes a model from
HuggingFace: [valurank/distilroberta-bias](https://huggingface.co/valurank/distilroberta-bias). This model is
specifically trained to detect biased statements in text. By examining a text's classification and score against a
predefined threshold, the scanner determines whether it's biased.

!!! note

    Supported languages: English

## Usage

```python
from llm_guard.output_scanners import Bias
from llm_guard.output_scanners.bias import MatchType

scanner = Bias(threshold=0.5, match_type=MatchType.FULL)
sanitized_output, is_valid, risk_score = scanner.scan(prompt, model_output)
```

## Optimization Strategies

[Read more](../usage/optimization.md)

## Benchmarks

Test setup:

- Platform: Amazon Linux 2
- Python Version: 3.11.6
- Input length: 128
- Test times: 5

Run the following script:

```sh
python benchmarks/run.py output Bias
```

Results:

| Instance                         | Latency Variance | Latency 90 Percentile | Latency 95 Percentile | Latency 99 Percentile | Average Latency (ms) | QPS      |
|----------------------------------|------------------|-----------------------|-----------------------|-----------------------|----------------------|----------|
| AWS m5.xlarge                    | 2.96             | 111.97                | 139.15                | 160.88                | 57.55                | 2224.21  |
| AWS m5.xlarge with ONNX          | 0.00             | 17.51                 | 17.87                 | 18.16                 | 16.77                | 7633.97  |
| AWS g5.xlarge GPU                | 32.51            | 275.34                | 365.39                | 437.44                | 94.85                | 1349.48  |
| AWS g5.xlarge GPU with ONNX      | 0.01             | 6.69                  | 8.22                  | 9.45                  | 3.59                 | 35633.81 |
| Azure Standard_D4as_v4           | 3.91             | 126.54                | 157.68                | 182.60                | 63.81                | 2006.08  |
| Azure Standard_D4as_v4 with ONNX | 0.03             | 29.55                 | 31.41                 | 32.89                 | 23.36                | 5479.92  |
