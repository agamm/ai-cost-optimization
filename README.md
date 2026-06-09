# AI Cost Optimization Techniques

## Table of Contents
- [Prompt Reduction Techniques](#prompt-reduction-techniques)
- [Prompt Caching](#prompt-caching)
- [Reasoning Budget Controls](#reasoning-budget-controls)
- [Batch Processing](#batch-processing)
- [Flex Processing](#flex-processing)
- [Context Compaction and Editing](#context-compaction-and-editing)
- [Deferred Tool Loading](#deferred-tool-loading)
- [LLM Routing](#llm-routing)
- [Cascading LLM Approach](#cascading-llm-approach)
- [Video-to-Text Preprocessing](#video-to-text-preprocessing)
- [Audio Acceleration](#audio-acceleration)
- [Model Optimization](#model-optimization)

## Prompt Reduction Techniques

Optimize prompt structure and length to reduce token usage while maintaining output quality. For token-priced APIs, fewer input tokens usually reduce cost roughly linearly; latency and compute savings depend on the model and serving stack.

**Research Links:**
- [LLMLingua: Prompt Compression - Microsoft Research](https://www.microsoft.com/en-us/research/blog/llmlingua-innovating-llm-efficiency-with-prompt-compression/)
- [Top 6 Strategies to Optimize Token Costs for LLM APIs](https://blog.typingmind.com/optimize-token-costs-for-chatgpt-and-llm-api/)
- [Efficient Prompting Methods for LLMs: A Survey](https://arxiv.org/html/2404.01077v1)
- [Learn Prompting: Guide to Communicating with AI](https://learnprompting.org/docs/introduction)
- [Agent variables](https://www.reddit.com/r/AI_Agents/comments/1pbfjru/we_cut_agent_token_usage_and_speed_by_82_with_one/)

**Tools:**
- [Prompt Optimizer](https://github.com/vaibkumr/prompt-optimizer) - Minimize token complexity
- [LLMLingua](https://www.mongodb.com/developer/products/atlas/prompt_compression/) - Prompt compression via LangChain
- [TOON](https://github.com/toon-format/toon) - Compact alternative to JSON for uniform, tabular data in prompts
- Skeleton-of-Thought (SoT) prompting - Reported 2.39x faster generation in its evaluation

## Prompt Caching

Reuse stable prompt prefixes such as system instructions, examples, long documents, and tool definitions. Put shared content first and request-specific content last to improve cache-hit rates, then monitor cached-token usage and account for provider-specific minimums, expiration times, and cache-write costs.

**Provider Documentation:**
- [OpenAI Prompt Caching](https://developers.openai.com/api/docs/guides/prompt-caching)
- [Claude Prompt Caching](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)
- [Gemini Context Caching](https://ai.google.dev/gemini-api/docs/caching)
- [Amazon Bedrock Prompt Caching](https://docs.aws.amazon.com/bedrock/latest/userguide/prompt-caching.html)

## Reasoning Budget Controls

Match reasoning effort to task complexity instead of using the maximum setting for every request. Start with lower effort for routine classification, extraction, and lookup tasks, and increase it only when evaluations show a meaningful quality improvement.

**Provider Documentation:**
- [OpenAI Reasoning Effort](https://developers.openai.com/api/docs/guides/reasoning#reasoning-effort)
- [Claude Effort](https://platform.claude.com/docs/en/build-with-claude/effort)
- [Gemini Thinking Budgets](https://ai.google.dev/gemini-api/docs/thinking#thinking-budgets)

## Batch Processing

Process non-urgent AI requests asynchronously in batches. Many providers offer batch pricing at about 50% of standard rates, but discounts, model support, limits, and completion windows vary.

**Research Links:**
- [Azure OpenAI Global Batch Offering](https://techcommunity.microsoft.com/blog/azure-ai-services-blog/announcing-azure-openai-global-batch-offering-efficient-processing-at-scale-with/4210929)
- [Together AI Batch API](https://www.together.ai/blog/batch-api)
- [OpenAI Batch API](https://developers.openai.com/api/docs/guides/batch)

**Tools:**
- Azure OpenAI Batch API
- Together AI Batch API
- Google Gemini Batch API
- Mistral Batch API
- [agamm/batchata](https://github.com/agamm/batchata) - Unified Python API for batch requests

## Flex Processing

Use lower-cost, slower processing tiers for workloads that do not need predictable latency. OpenAI Flex processing is intended for non-production or lower-priority tasks that can tolerate longer responses, occasional resource-unavailable errors, and retry logic.

**Provider Documentation:**
- [OpenAI Flex Processing](https://developers.openai.com/api/docs/guides/flex-processing)

## Context Compaction and Editing

Reduce the repeated input cost of long-running conversations by compacting older history or clearing stale tool results and reasoning blocks. Preserve decisions, user requirements, and unresolved work while removing verbose intermediate data that no longer affects the task.

**Provider Documentation:**
- [OpenAI Compaction](https://developers.openai.com/api/docs/guides/compaction)
- [Claude Context Editing](https://platform.claude.com/docs/en/build-with-claude/context-editing)

## Deferred Tool Loading

Avoid sending every tool or MCP definition with every request. Load tool definitions only when they are relevant, especially for agents with large catalogs, while keeping frequently used tools immediately available when the search overhead would not be worthwhile.

**Provider Documentation:**
- [OpenAI Tool Search](https://developers.openai.com/api/docs/guides/tools-tool-search)
- [Claude Tool Search](https://platform.claude.com/docs/en/agents-and-tools/tool-use/tool-search-tool)

## LLM Routing

Route requests to the most cost-effective model likely to meet each task's quality and latency requirements. Reported savings vary substantially by workload, model pool, and routing accuracy.

**Research Links:**
- [RouteLLM: Open-Source Framework for Cost-Effective LLM Routing](https://lmsys.org/blog/2024-07-01-routellm/)
- [LLM Providers Performance vs Cost Analysis](https://nelsonauner.com/data/2024/04/15/empirical-results-of-LLM-scoring.html)

**Tools:**
- [OpenRouter provider routing](https://openrouter.ai/docs/guides/routing/provider-selection) (use `<model>:floor` to prioritize the lowest-priced provider)
- [Requesty](https://www.requesty.ai/)
- RouteLLM framework

## Cascading LLM Approach

Start with cheaper models and escalate only when a confidence check, evaluator, or task policy indicates that a stronger model is needed. Savings depend on escalation rate and evaluator quality.

**Research Links:**
- [Cascade AI: Full LLM Power, 60% Cheaper](https://domino.ai/blog/full-llm-power-60-percent-cheaper)
- [Model Cascading for Code Generation](https://arxiv.org/abs/2405.15842)
- [LLM Cost Saving with Decision Maker](https://medium.com/@mehrdad-/llm-cost-saving-llm-cascade-with-a-decision-maker-7d80269779b5)
- [IBM Research LLM Routing](https://research.ibm.com/blog/LLM-routers)

**Tools:**
- FrugalGPT
- Mixture of Thought (MoT) frameworks
- Custom cascade implementations

## Video-to-Text Preprocessing

For transcription or speech-focused analysis of video, extract the audio and optionally accelerate it before transcription. This can reduce effective duration-based costs, but savings and accuracy depend on the provider, model, audio quality, and playback speed.

**Research Links:**
- [OpenAI Charges by the Minute, So Make the Minutes Shorter](https://george.mand.is/2025/06/openai-charges-by-the-minute-so-make-the-minutes-shorter/)
- [Token-Efficient Long Video Understanding for Multimodal LLMs](https://arxiv.org/abs/2503.04130)
- [STORM: Spatiotemporal Token Reduction](https://research.nvidia.com/labs/lpr/storm/)

**Tools:**
- ffmpeg with the `atempo` filter for accelerating extracted audio
- STORM (reported up to 8x lower compute cost for long-video understanding)
- LLM-VTP (reported 80-90% token reduction in evaluated settings)

## Audio Acceleration

Accelerate audio before transcription when the selected API's billing decreases with effective audio duration or token count. Benchmark transcription quality on representative audio before using this in production.

**Research Links:**
- [OpenAI Charges by the Minute, So Make the Minutes Shorter](https://george.mand.is/2025/06/openai-charges-by-the-minute-so-make-the-minutes-shorter/)
- [AIx Speed: Playback Speed Optimization Using Speech Recognition](https://arxiv.org/abs/2403.02938)

**Tools:**
- ffmpeg: `ffmpeg -i audio.m4a -filter:a "atempo=3.0" -ac 1 -b:a 64k audio-3x.mp3`
- AIx Speed optimization system
- Audio compression algorithms

## Model Optimization

Reduce self-hosted inference costs through model compression and optimization while measuring the effect on quality, latency, throughput, and hardware compatibility.

**Research Links:**
- [LLM Compression Research Papers](https://github.com/HuangOwen/Awesome-LLM-Compression)
- [AWQ: Activation-aware Weight Quantization](https://github.com/mit-han-lab/llm-awq) - MLSys 2024 Best Paper
- [Contemporary Model Compression on LLMs](https://arxiv.org/abs/2409.01990)

**Tools:**
- AWQ low-bit weight quantization (INT3/INT4)
- Model pruning libraries
- Knowledge distillation frameworks
