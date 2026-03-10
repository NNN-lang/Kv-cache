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
