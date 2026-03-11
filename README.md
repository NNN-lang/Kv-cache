# Kv-cache

ReasoningKVC: Block-Aware Attention Mask KV Compression for LLM Reasoning Chains

For xAI Colossus — 40% KV masked, ΔPPL +0.02, latency even better than baseline, savings up to $51M/month on 100K H100.

Results (T4 GPU, March 10, 2026)

Compression: 40% KV masked (pruned/zeroed)

ΔPPL: +0.0200 (target < 0.05)

Latency overhead: from -43.6% to -0.5% (speedup)

Throughput: +80–130% (1625 → 3803 tokens/sec)

Tests: 4K / 8K / 12K (ready for 16K–32K)

Method: Attention mask + structure-aware block detection (perplexity + keyword boundaries)

Business impact (estimate)

5K H100 → $2.59M/month

20K H100 → $10.37M/month

100K H100 (xAI Colossus) → $51.84M/month

How to use
git clone https://github.com/your-username/reasoning-kvc-attention-mask.git
cd reasoning-kvc-attention-mask

Block-aware KV cache compression for long reasoning chains in LLM inference.

## Key Idea

Reasoning chains contain natural structural boundaries:

Step 1 → Step 2 → Verification → Conclusion

ReasoningKVC detects these blocks and selectively prunes low-importance KV tokens while protecting anchors and recent blocks.

This reduces KV-cache size while preserving reasoning quality.

## Results

Model: Qwen2.5-1.5B  
Hardware: Tesla T4 (Google Colab)

| Sequence | Tokens | KV Masked | ΔPPL |
|--------|--------|--------|--------|
| ~4K | 4096 | ~38% | +0.01 |
| ~8K | 8192 | ~41% | +0.02 |
| ~12K | 12288 | ~40% | +0.01 |

Quality preserved (ΔPPL < 0.05).

## Potential Impact

Long-context reasoning models (e.g. Grok-style think traces) store massive KV caches.

Example (70B model):

32K tokens KV cache ≈ 18GB  
40% compression → 7.2GB saved per request.

This increases batch capacity and reduces GPU inference cost.

## Colab Benchmark

Run the full benchmark here:

https://colab.research.google.com/drive/1lltRreClnmzQFN8W6UoreA8Pggohwq2q#scrollTo=rbQdtDeuoR87

## Report

Full benchmark PDF included in this repo.

## Algorithm Overview

1. Hybrid segmentation
   - regex transitions
   - perplexity spike detection

2. Block-aware pruning
   - protect recent reasoning blocks
   - anchor tokens preserved
   - importance threshold

3. Safe masking
   - attention_mask zeroing
   - no positional mismatch
   - no retraining required

## Integration Target

This method is designed to integrate with inference engines such as:

- vLLM
- TensorRT-LLM
- DeepSpeed-Inference

## License

MIT
