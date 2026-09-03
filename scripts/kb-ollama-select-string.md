PS D:\git_public\base-conocimiento-sandbox> docker logs kb-ollama | Select-String "discovering|inference compute|no compatible|library|cuda|vulkan"
time=2026-09-02T16:51:59.107Z level=INFO source=routes.go:1951 msg="server config" env="map[CUDA_VISIBLE_DEVICES: GGML_VK_VISIBLE_DEVICES: GPU_DEVICE_ORDINAL: HIP_VISIBLE_DEVICES: HSA_OVERRIDE_GFX_VERSION: HTTPS_PROXY: HTTP_PROXY: LLAMA_ARG_FIT: LLAMA_ARG_FIT_TARGET: NO_PROXY: OLLAMA_CONTEXT_LENGTH:0 OLLAMA_DEBUG:INFO OLLAMA_DEBUG_LOG_REQUESTS:false OLLAMA_EDITOR: OLLAMA_FLASH_ATTENTION:false OLLAMA_GO_TEMPLATE:true OLLAMA_GPU_OVERHEAD:0 OLLAMA_HOST:http://0.0.0.0:11434 OLLAMA_IGPU_ENABLE: OLLAMA_KEEP_ALIVE:1h0m0s OLLAMA_KV_CACHE_TYPE: OLLAMA_LLM_LIBRARY: OLLAMA_LOAD_TIMEOUT:5m0s OLLAMA_MAX_LOADED_MODELS:2 OLLAMA_MAX_QUEUE:512 OLLAMA_MAX_TRANSFER_STREAMS:4 OLLAMA_MODELS:/root/.ollama/models OLLAMA_NOHISTORY:false OLLAMA_NOPRUNE:false OLLAMA_NO_CLOUD:false OLLAMA_NUM_PARALLEL:2 OLLAMA_ORIGINS:[http://localhost https://localhost http://localhost:* https://localhost:* http://127.0.0.1 https://127.0.0.1 http://127.0.0.1:* https://127.0.0.1:* http://0.0.0.0 https://0.0.0.0 http://0.0.0.0:* https://0.0.0.0:* app://* file://* tauri://* vscode-webview://* vscode-file://*] OLLAMA_REMOTES:[ollama.com] OLLAMA_SCHED_SPREAD:false OLLAMA_VULKAN:true ROCR_VISIBLE_DEVICES: http_proxy: https_proxy: no_proxy:]"
time=2026-09-02T16:51:59.108Z level=INFO source=routes.go:1953 msg="Ollama cloud disabled: false"
time=2026-09-02T16:51:59.225Z level=INFO source=images.go:919 msg="total blobs: 13"
time=2026-09-02T16:51:59.282Z level=INFO source=images.go:926 msg="total unused blobs removed: 0"
time=2026-09-02T16:51:59.346Z level=INFO source=routes.go:2008 msg="Listening on [::]:11434 (version 0.33.2)"
time=2026-09-02T16:51:59.354Z level=INFO source=runner.go:60 msg="discovering available GPUs..."
time=2026-09-02T16:51:59.475Z level=INFO source=model_list_cache.go:112 msg="model list cache hydration complete" models=3 failures=0 elapsed=128.514391ms
time=2026-09-02T16:51:59.718Z level=INFO source=model_recommendations.go:177 msg="model recommendations cache sleep scheduled" wait=4h26m41.247824658s consecutive_failures=0
time=2026-09-02T16:52:02.246Z level=WARN source=cuda_compat.go:65 msg="NVIDIA driver too old" device="NVIDIA GeForce RTX 3060 Laptop GPU" compute=8.6 driver=546 required_driver="550 or newer"
time=2026-09-02T16:52:02.859Z level=WARN source=model_show_cache.go:362 msg="failed to hydrate cloud model show cache" model=gpt-oss:20b error="Post \"https://ollama.com:443/api/show?ts=1788367919\": context deadline exceeded"
time=2026-09-02T16:52:04.750Z level=INFO source=types.go:50 msg="inference compute" id=cpu library=cpu compute="" name=cpu description=cpu libdirs=ollama driver="" pci_id="" type="" total="6.7 GiB" available="6.0 GiB"
time=2026-09-02T16:52:04.751Z level=INFO source=routes.go:2058 msg="vram-based default context" total_vram="0 B" default_num_ctx=4096
time=2026-09-02T16:52:34.597Z level=INFO source=sched.go:1147 msg="disabling mmap for llama-server load by default" model=/root/.ollama/models/blobs/sha256-9ed150d4367e68df0ac8e1540f6ddc65b42d0ee26378329d1ecbca60f93fc5f8 reason=cpu
time=2026-09-02T16:52:34.597Z level=INFO source=server.go:109 msg="using llama-server for model" model=/root/.ollama/models/blobs/sha256-9ed150d4367e68df0ac8e1540f6ddc65b42d0ee26378329d1ecbca60f93fc5f8
time=2026-09-02T16:52:34.600Z level=INFO source=llama_server.go:433 msg="starting llama-server" cmd="/usr/lib/ollama/llama-server --model /root/.ollama/models/blobs/sha256-9ed150d4367e68df0ac8e1540f6ddc65b42d0ee26378329d1ecbca60f93fc5f8 --port 44067 --host 127.0.0.1 --no-webui --offline -c 8192 -np 2 --log-verbosity 4 --no-log-prefix --no-log-timestamps --mmproj /root/.ollama/models/blobs/sha256-70ca26e41ecea9da997e2887462ad68b848fbf64da011ff4107bbe7353519ee0 --load-mode none --flash-attn auto -b 1024 -ub 1024 --context-shift --keep 4"
time=2026-09-02T16:52:34.601Z level=INFO source=sched.go:613 msg="system memory" total="6.7 GiB" free="6.1 GiB" free_swap="1.9 GiB"
time=2026-09-02T16:52:34.601Z level=INFO source=llama_server.go:1048 msg="loading model via llama-server" model=/root/.ollama/models/blobs/sha256-9ed150d4367e68df0ac8e1540f6ddc65b42d0ee26378329d1ecbca60f93fc5f8
time=2026-09-02T16:52:34.601Z level=INFO source=llama_server.go:1295 msg="waiting for llama-server to start responding"
time=2026-09-02T16:52:34.601Z level=INFO source=llama_server.go:1350 msg="waiting for llama-server to become available" status="llm server not responding"
cmn  common_param: common_params_print_info: build 1 (d222767c7) with GNU 13.3.1 for Linux x86_64
cmn  common_param: common_params_print_info: verbosity = 4 (adjust with the `-lv N` CLI arg)
cmn  common_param: device_info:
cmn  common_param:   - CPU     : AMD Ryzen 7 5800H with Radeon Graphics (6858 MiB, 6858 MiB free)
cmn  common_param: system_info: n_threads = 8 (n_threads_batch = 8) / 16 | CPU : SSE3 = 1 | SSSE3 = 1 | AVX = 1 | AVX2 = 1 | F16C = 1 | FMA = 1 | BMI2 = 1 | LLAMAFILE = 1 | REPACK = 1 |
srv          init: using 15 threads for HTTP server
srv          init: The UI is disabled
srv          init: Use --ui/--no-ui (or deprecated --webui/--no-webui) to enable/disable
srv  llama_server: -----------------
srv  llama_server: CORS is set to allow all origins ('*') and no API key is set
srv  llama_server: this can be a security risk (cross-origin attacks)
srv  llama_server: more info: https://github.com/ggml-org/llama.cpp/pull/25655
srv  llama_server: -----------------
srv         start: binding port with default address family
srv    load_model: loading model '/root/.ollama/models/blobs/sha256-9ed150d4367e68df0ac8e1540f6ddc65b42d0ee26378329d1ecbca60f93fc5f8'
srv    load_model: local path '/root/.ollama/models/blobs/sha256-9ed150d4367e68df0ac8e1540f6ddc65b42d0ee26378329d1ecbca60f93fc5f8'
srv    load_model: [mtmd] estimated worst-case memory usage of mmproj is 867.00 MiB (took 25.71 ms)
cmn  common_init_: fitting params to device memory ...
cmn  common_init_: (for bugs during this step try to reproduce them with -fit off, or provide --verbose logs if the bug only occurs with -fit on)
common_params_fit_impl: getting device memory data for initial parameters:
time=2026-09-02T16:52:34.854Z level=INFO source=llama_server.go:1350 msg="waiting for llama-server to become available" status="llm server loading model"
common_memory_breakdown_print: | memory breakdown [MiB] | total   free    self   model   context   compute    unaccounted |
common_memory_breakdown_print: |   - Host               |                 1635 =   635 +     832 +     168                |
common_memory_breakdown_print: |   - CPU_REPACK         |                 1404 =  1404 +       0 +       0                |
common_params_fit_impl: projected to use 1635 MiB of host memory vs. 6858 MiB of total host memory
common_params_fit_impl: will leave 5222 >= 2693 MiB of system memory, no changes needed
common_fit_params: successfully fit params to free device memory
common_fit_params: fitting params to free memory took 0.70 seconds
llama_model_loader: loaded meta data with 53 key-value pairs and 236 tensors from /root/.ollama/models/blobs/sha256-9ed150d4367e68df0ac8e1540f6ddc65b42d0ee26378329d1ecbca60f93fc5f8 (version GGUF V3 (latest))
llama_model_loader: Dumping metadata keys/values. Note: KV overrides do not apply in this output.
llama_model_loader: - kv   0:                       general.architecture str              = mistral3
llama_model_loader: - kv   1:                               general.type str              = model
llama_model_loader: - kv   2:                               general.name str              = ministral-3B-Instruct-2512
llama_model_loader: - kv   3:                            general.version str              = 3.0
llama_model_loader: - kv   4:                       general.organization str              = Mistral AI
llama_model_loader: - kv   5:                           general.finetune str              = 2512
llama_model_loader: - kv   6:                           general.basename str              = Ministral
llama_model_loader: - kv   7:                        general.description str              = The Ministral 3 family is designed fo...
llama_model_loader: - kv   8:                         general.size_label str              = 3B
llama_model_loader: - kv   9:                            general.license str              = apache-2.0
llama_model_loader: - kv  10:                                general.url str              = https://huggingface.co/mistralai/Mini...
llama_model_loader: - kv  11:                           general.repo_url str              = https://huggingface.co/mistralai/Mini...
llama_model_loader: - kv  12:                   general.base_model.count u32              = 1
llama_model_loader: - kv  13:                  general.base_model.0.name str              = Ministral 3 3B Base 2512
llama_model_loader: - kv  14:               general.base_model.0.version str              = 2512
llama_model_loader: - kv  15:          general.base_model.0.organization str              = Mistralai
llama_model_loader: - kv  16:              general.base_model.0.repo_url str              = https://huggingface.co/mistralai/Mini...
llama_model_loader: - kv  17:                               general.tags arr[str,1]       = ["mistral-common"]
llama_model_loader: - kv  18:                          general.languages arr[str,11]      = ["en", "fr", "es", "de", "it", "pt", ...
llama_model_loader: - kv  19:                       mistral3.block_count u32              = 26
llama_model_loader: - kv  20:                    mistral3.context_length u32              = 262144
llama_model_loader: - kv  21:                  mistral3.embedding_length u32              = 3072
llama_model_loader: - kv  22:               mistral3.feed_forward_length u32              = 9216
llama_model_loader: - kv  23:              mistral3.attention.head_count u32              = 32
llama_model_loader: - kv  24:           mistral3.attention.head_count_kv u32              = 8
llama_model_loader: - kv  25:                    mistral3.rope.freq_base f32              = 1000000.000000
llama_model_loader: - kv  26:  mistral3.attention.layer_norm_rms_epsilon f32              = 0.000010
llama_model_loader: - kv  27:              mistral3.attention.key_length u32              = 128
llama_model_loader: - kv  28:            mistral3.attention.value_length u32              = 128
llama_model_loader: - kv  29:              mistral3.rope.dimension_count u32              = 128
llama_model_loader: - kv  30:                 mistral3.rope.scaling.type str              = yarn
llama_model_loader: - kv  31:               mistral3.rope.scaling.factor f32              = 16.000000
llama_model_loader: - kv  32:       mistral3.rope.scaling.yarn_beta_fast f32              = 32.000000
llama_model_loader: - kv  33:       mistral3.rope.scaling.yarn_beta_slow f32              = 1.000000
llama_model_loader: - kv  34:  mistral3.rope.scaling.yarn_log_multiplier f32              = 1.000000
llama_model_loader: - kv  35: mistral3.rope.scaling.original_context_length u32              = 16384
llama_model_loader: - kv  36:       mistral3.attention.temperature_scale f32              = 0.100000
llama_model_loader: - kv  37:                       tokenizer.ggml.model str              = gpt2
llama_model_loader: - kv  38:                         tokenizer.ggml.pre str              = tekken
llama_model_loader: - kv  39:                      tokenizer.ggml.merges arr[str,269443]  = ["Ġ Ġ", "Ġ t", "e r", "i n", "Ġ �...
llama_model_loader: - kv  40:                tokenizer.ggml.bos_token_id u32              = 1
llama_model_loader: - kv  41:                tokenizer.ggml.eos_token_id u32              = 2
llama_model_loader: - kv  42:            tokenizer.ggml.unknown_token_id u32              = 0
llama_model_loader: - kv  43:            tokenizer.ggml.padding_token_id u32              = 11
llama_model_loader: - kv  44:                      tokenizer.ggml.tokens arr[str,131072]  = ["<unk>", "<s>", "</s>", "[INST]", "[...
llama_model_loader: - kv  45:                      tokenizer.ggml.scores arr[i32,131072]  = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ...
llama_model_loader: - kv  46:                  tokenizer.ggml.token_type arr[i32,131072]  = [3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, ...
llama_model_loader: - kv  47:                        mistral3.vocab_size u32              = 131072
llama_model_loader: - kv  48:               tokenizer.ggml.add_bos_token bool             = true
llama_model_loader: - kv  49:               tokenizer.ggml.add_eos_token bool             = false
llama_model_loader: - kv  50:                    tokenizer.chat_template str              = {#- Default system message if no syst...
llama_model_loader: - kv  51:               general.quantization_version u32              = 2
llama_model_loader: - kv  52:                          general.file_type u32              = 15
llama_model_loader: - type  f32:   53 tensors
llama_model_loader: - type q4_K:  156 tensors
llama_model_loader: - type q6_K:   27 tensors
print_info: file format = GGUF V3 (latest)
print_info: file type   = Q4_K - Medium
print_info: file size   = 1.99 GiB (4.99 BPW)
load: 0 unused tokens
load: printing all EOG tokens:
load:   - 2 ('</s>')
load: special tokens cache size = 1000
load: token to piece cache size = 0.8498 MB
print_info: arch                  = mistral3
print_info: vocab_only            = 0
print_info: no_alloc              = 0
print_info: n_ctx_train           = 262144
print_info: n_embd_inp            = 3072
print_info: n_embd                = 3072
print_info: n_embd_out            = 3072
print_info: n_layer               = 26
print_info: n_layer_all           = 26
print_info: n_head                = 32
print_info: n_head_kv             = 8
print_info: n_rot                 = 128
print_info: n_swa                 = 0
print_info: is_swa_any            = 0
print_info: n_embd_head_k         = 128
print_info: n_embd_head_v         = 128
print_info: n_gqa                 = 4
print_info: n_embd_k_gqa          = 1024
print_info: n_embd_v_gqa          = 1024
print_info: f_norm_eps            = 0.0e+00
print_info: f_norm_rms_eps        = 1.0e-05
print_info: f_clamp_kqv           = 0.0e+00
print_info: f_max_alibi_bias      = 0.0e+00
print_info: f_logit_scale         = 0.0e+00
print_info: f_attn_scale          = 0.0e+00
print_info: f_attn_value_scale    = 0.0000
print_info: n_ff                  = 9216
print_info: n_expert              = 0
print_info: n_expert_used         = 0
print_info: n_expert_groups       = 0
print_info: n_group_used          = 0
print_info: causal attn           = 1
print_info: pooling type          = -1
print_info: rope type             = 0
print_info: rope scaling          = yarn
print_info: freq_base_train       = 1000000.0
print_info: freq_scale_train      = 0.0625
print_info: n_ctx_orig_yarn       = 16384
print_info: rope_yarn_log_mul     = 1.0000
print_info: rope_finetuned        = unknown
print_info: model type            = 3B
print_info: model params          = 3.43 B
print_info: general.name          = ministral-3B-Instruct-2512
print_info: vocab type            = BPE
print_info: n_vocab               = 131072
print_info: n_merges              = 269443
print_info: BOS token             = 1 '<s>'
print_info: EOS token             = 2 '</s>'
print_info: UNK token             = 0 '<unk>'
print_info: PAD token             = 11 '<pad>'
print_info: LF token              = 1010 'Ċ'
print_info: EOG token             = 2 '</s>'
print_info: max token length      = 150
load_tensors: loading model tensors, this can take a while... (load_mode = none)
load_tensors:          CPU model buffer size =   635.54 MiB
load_tensors:   CPU_REPACK model buffer size =  1404.00 MiB
cmn  common_init_: added </s> logit bias = -inf
llama_context: constructing llama_context
llama_context: setting new yarn_attn_factor = 1.0000 (mscale == 1.0, mscale_all_dim = 1.0)
llama_context: n_seq_max             = 2
llama_context: n_ctx                 = 8192
llama_context: n_ctx_seq             = 4096
llama_context: n_batch               = 1024
llama_context: n_ubatch              = 1024
llama_context: causal_attn           = 1
llama_context: flash_attn            = auto
llama_context: kv_unified            = false
llama_context: freq_base             = 1000000.0
llama_context: freq_scale            = 0.0625
llama_context: n_rs_seq              = 0
llama_context: n_outputs_max         = 2
llama_context: n_outputs_max_per_seq = 1
llama_context: n_ctx_seq (4096) < n_ctx_train (262144) -- the full capacity of the model will not be utilized
llama_context:        CPU  output buffer size =     1.00 MiB
llama_kv_cache:        CPU KV buffer size =   832.00 MiB
llama_kv_cache: size =  832.00 MiB (  4096 cells,  26 layers,  2/2 seqs), K (f16):  416.00 MiB, V (f16):  416.00 MiB
llama_kv_cache: attn_rot_k = 0, n_embd_head_k_all = 128
llama_kv_cache: attn_rot_v = 0, n_embd_head_k_all = 128
sched_reserve: reserving ...
resolve_fused_ops: Flash Attention enabled
resolve_fused_ops: resolving fused Gated Delta Net support:
resolve_fused_ops: fused Gated Delta Net (autoregressive) enabled
resolve_fused_ops: fused Gated Delta Net (chunked) enabled
resolve_fused_ops: resolving fused Lightning Indexer support:
resolve_fused_ops: Lightning Indexer enabled
resolve_fused_ops: resolving fused DeepSeek V4 HC support:
resolve_fused_ops: fused DeepSeek V4 HC pre enabled
resolve_fused_ops: fused DeepSeek V4 HC comb enabled
resolve_fused_ops: fused DeepSeek V4 HC post enabled
sched_reserve:        CPU compute buffer size =   168.03 MiB
sched_reserve: graph nodes  = 890
sched_reserve: graph splits = 1
sched_reserve: reserve took 5.37 ms, sched copies = 1
cmn          init: llama threadpool init, n_threads = 8
cmn  common_init_: warming up the model with an empty run - please wait ... (--no-warmup to disable)
clip_model_loader: model name:   ministral-3B-Instruct-2512
clip_model_loader: description:  The Ministral 3 family is designed for edge deployment, capable of running on a wide range of hardware. This model is the 3B instruct post-trained version in FP8, fine-tuned for instruction tasks, making it ideal for chat and instruction based use cases.
clip_model_loader: GGUF version: 3
clip_model_loader: alignment:    32
clip_model_loader: n_tensors:    223
clip_model_loader: n_kv:         36

clip_model_loader: has vision encoder
clip_ctx: CLIP using CPU backend
load_hparams: projector:          pixtral
load_hparams: n_embd:             1024
load_hparams: n_head:             16
load_hparams: n_ff:               4096
load_hparams: n_layer:            24
load_hparams: ffn_op:             silu
load_hparams: projection_dim:     9216

--- vision hparams ---
load_hparams: image_size:         1540
load_hparams: patch_size:         14
load_hparams: has_llava_proj:     0
load_hparams: minicpmv_version:   0
load_hparams: n_merge:            2
load_hparams: n_wa_pattern: 0
load_hparams: image_min_pixels:   6272
load_hparams: image_max_pixels:   802816

load_hparams: model size:         802.50 MiB
load_hparams: metadata size:      0.08 MiB
get_dummy_batch: warmup with image size = 448 x 448
get_dummy_batch: warmup with image size = 448 x 448
reserve_compute_meta:        CPU compute buffer size =    64.49 MiB
reserve_compute_meta: graph splits = 1, nodes = 703
warmup: flash attention is enabled
srv    load_model: loaded multimodal model, '/root/.ollama/models/blobs/sha256-70ca26e41ecea9da997e2887462ad68b848fbf64da011ff4107bbe7353519ee0'
srv    load_model: ctx_shift is not supported by multimodal, it will be disabled
srv    load_model: initializing, n_slots = 2, n_ctx_slot = 4096, kv_unified = 'false'
spec common_specu: no implementations specified for speculative decoding
slot   load_model: id  0 | task -1 | new slot, n_ctx = 4096
slot   load_model: id  1 | task -1 | new slot, n_ctx = 4096
srv    load_model: prompt cache is enabled, size limit: 8192 MiB
srv    load_model: use `--cache-ram 0` to disable the prompt cache
srv    load_model: for more info see https://github.com/ggml-org/llama.cpp/pull/16391
srv    load_model: context checkpoints enabled, max = 32, min spacing = 8192
srv          init: idle slots will be saved to prompt cache upon starting a new task
srv          init: init: chat template, example_format: '[SYSTEM_PROMPT]You are a helpful assistant[/SYSTEM_PROMPT][INST]Hello[/INST]Hi there</s>[INST]How are you?[/INST]'
srv          init: init: chat template, thinking = 1
srv  llama_server: model loaded
srv  llama_server: listening on http://127.0.0.1:44067
srv  update_slots: all slots are idle
time=2026-09-02T16:53:11.045Z level=INFO source=llama_server.go:1362 msg="llama-server started in 36.45 seconds"
time=2026-09-02T16:53:11.206Z level=INFO source=images.go:381 msg="template selection" model=hf.co/mistralai/Ministral-3-3B-Instruct-2512-GGUF:Q4_K_M selected=gguf_chat_template renderer="" parser="" go_template="[completion vision]" chat_template="[tools completion vision]" harmony=null renderer_parser=null
time=2026-09-02T16:53:11.206Z level=INFO source=sched.go:728 msg="loaded runners" count=1
time=2026-09-02T16:53:11.206Z level=INFO source=llama_server.go:1295 msg="waiting for llama-server to start responding"
time=2026-09-02T16:53:11.207Z level=INFO source=llama_server.go:1362 msg="llama-server started in 36.61 seconds"
srv  server_strea: conv_id= (empty=1)
srv    operator(): chat format: peg-native
slot get_availabl: id  0 | task -1 |  - skipping, slot is empty
slot get_availabl: id  1 | task -1 |  - skipping, slot is empty
slot get_availabl: id  1 | task -1 | selected slot by LRU, t_last = -1
srv  get_availabl: updating prompt cache
srv          load:  - looking for better prompt, base f_keep = -1.000, f_sim = 0.000
srv        update:  - cache state: 0 prompts, 0.000 MiB (limits: 8192.000 MiB, 8192 tokens, 8589934592 est)
srv  get_availabl: prompt cache update took 0.03 ms
slot launch_slot_: id  1 | task -1 | sampler chain: logits -> ?penalties -> ?dry -> ?top-n-sigma -> top-k -> ?typical -> ?top-p -> ?min-p -> ?xtc -> temp-ext -> dist
slot launch_slot_: id  1 | task -1 | sampler params:
        repeat_last_n = 64, repeat_penalty = 1.000, frequency_penalty = 0.000, presence_penalty = 0.000
        dry_multiplier = 0.000, dry_base = 1.750, dry_allowed_length = 2, dry_penalty_last_n = 64
        top_k = 40, top_p = 1.000, min_p = 0.000, xtc_probability = 0.000, xtc_threshold = 0.100, typical_p = 1.000, top_n_sigma = -1.000, temp = 0.200
        mirostat = 0, mirostat_lr = 0.100, mirostat_ent = 5.000, adaptive_target = -1.000, adaptive_decay = 0.900
slot launch_slot_: id  1 | task 0 | processing task, is_child = 0
slot process_sing: id  0 | task -1 | saving idle slot to prompt cache
slot   operator(): id  1 | task 0 | new prompt, n_ctx_slot = 4096, n_keep = 4, task.n_tokens = 871
slot   operator(): id  1 | task 0 | cached n_tokens = 0, memory_seq_rm [0, end)
slot init_sampler: id  1 | task 0 | init sampler, took 0.15 ms, tokens: text = 871, total = 871
slot print_timing: id  1 | task 0 | prompt eval time =   13734.80 ms /   871 tokens (   15.77 ms per token,    63.42 tokens per second)
slot print_timing: id  1 | task 0 |        eval time =    3523.21 ms /    42 tokens (   85.93 ms per token,    11.64 tokens per second)
slot print_timing: id  1 | task 0 |       total time =   17258.01 ms /   913 tokens
slot print_timing: id  1 | task 0 |    graphs reused =          0
slot      release: id  1 | task 0 | stop processing: n_tokens = 912, truncated = 0
srv  update_slots: all slots are idle
time=2026-09-02T16:53:29.343Z level=INFO source=sched.go:1147 msg="disabling mmap for llama-server load by default" model=/root/.ollama/models/blobs/sha256-daec91ffb5dd0c27411bd71f29932917c49cf529a641d0168496c3a501e3062c reason=cpu
time=2026-09-02T16:53:29.343Z level=INFO source=server.go:109 msg="using llama-server for model" model=/root/.ollama/models/blobs/sha256-daec91ffb5dd0c27411bd71f29932917c49cf529a641d0168496c3a501e3062c
time=2026-09-02T16:53:29.344Z level=INFO source=llama_server.go:433 msg="starting llama-server" cmd="/usr/lib/ollama/llama-server --model /root/.ollama/models/blobs/sha256-daec91ffb5dd0c27411bd71f29932917c49cf529a641d0168496c3a501e3062c --port 39083 --host 127.0.0.1 --no-webui --offline -c 4096 -np 1 --log-verbosity 4 --no-log-prefix --no-log-timestamps --load-mode none --flash-attn auto --embedding -b 2048 -ub 2048 --context-shift --keep 4"
time=2026-09-02T16:53:29.345Z level=INFO source=sched.go:613 msg="system memory" total="6.7 GiB" free="2.6 GiB" free_swap="1.9 GiB"
time=2026-09-02T16:53:29.345Z level=INFO source=llama_server.go:1048 msg="loading model via llama-server" model=/root/.ollama/models/blobs/sha256-daec91ffb5dd0c27411bd71f29932917c49cf529a641d0168496c3a501e3062c
time=2026-09-02T16:53:29.345Z level=INFO source=llama_server.go:1295 msg="waiting for llama-server to start responding"
time=2026-09-02T16:53:29.345Z level=INFO source=llama_server.go:1350 msg="waiting for llama-server to become available" status="llm server not responding"
cmn  common_param: common_params_print_info: build 1 (d222767c7) with GNU 13.3.1 for Linux x86_64
cmn  common_param: common_params_print_info: verbosity = 4 (adjust with the `-lv N` CLI arg)
cmn  common_param: device_info:
cmn  common_param:   - CPU     : AMD Ryzen 7 5800H with Radeon Graphics (6858 MiB, 6858 MiB free)
cmn  common_param: system_info: n_threads = 8 (n_threads_batch = 8) / 16 | CPU : SSE3 = 1 | SSSE3 = 1 | AVX = 1 | AVX2 = 1 | F16C = 1 | FMA = 1 | BMI2 = 1 | LLAMAFILE = 1 | REPACK = 1 |
srv          init: using 15 threads for HTTP server
srv          init: The UI is disabled
srv          init: Use --ui/--no-ui (or deprecated --webui/--no-webui) to enable/disable
srv  llama_server: -----------------
srv  llama_server: CORS is set to allow all origins ('*') and no API key is set
srv  llama_server: this can be a security risk (cross-origin attacks)
srv  llama_server: more info: https://github.com/ggml-org/llama.cpp/pull/25655
srv  llama_server: -----------------
srv         start: binding port with default address family
srv    load_model: loading model '/root/.ollama/models/blobs/sha256-daec91ffb5dd0c27411bd71f29932917c49cf529a641d0168496c3a501e3062c'
srv    load_model: local path '/root/.ollama/models/blobs/sha256-daec91ffb5dd0c27411bd71f29932917c49cf529a641d0168496c3a501e3062c'
cmn  common_init_: fitting params to device memory ...
cmn  common_init_: (for bugs during this step try to reproduce them with -fit off, or provide --verbose logs if the bug only occurs with -fit on)
common_params_fit_impl: getting device memory data for initial parameters:
time=2026-09-02T16:53:29.597Z level=INFO source=llama_server.go:1350 msg="waiting for llama-server to become available" status="llm server loading model"
common_memory_breakdown_print: | memory breakdown [MiB] | total   free    self   model   context   compute    unaccounted |
common_memory_breakdown_print: |   - Host               |                 1161 =  1097 +       0 +      64                |
common_params_fit_impl: projected to use 1161 MiB of host memory vs. 6858 MiB of total host memory
common_params_fit_impl: will leave 5696 >= 1024 MiB of system memory, no changes needed
common_fit_params: successfully fit params to free device memory
common_fit_params: fitting params to free memory took 1.02 seconds
llama_model_loader: loaded meta data with 33 key-value pairs and 389 tensors from /root/.ollama/models/blobs/sha256-daec91ffb5dd0c27411bd71f29932917c49cf529a641d0168496c3a501e3062c (version GGUF V3 (latest))
llama_model_loader: Dumping metadata keys/values. Note: KV overrides do not apply in this output.
llama_model_loader: - kv   0:                       general.architecture str              = bert
llama_model_loader: - kv   1:                               general.type str              = model
llama_model_loader: - kv   2:                         general.size_label str              = 567M
llama_model_loader: - kv   3:                            general.license str              = mit
llama_model_loader: - kv   4:                               general.tags arr[str,4]       = ["sentence-transformers", "feature-ex...
llama_model_loader: - kv   5:                           bert.block_count u32              = 24
llama_model_loader: - kv   6:                        bert.context_length u32              = 8192
llama_model_loader: - kv   7:                      bert.embedding_length u32              = 1024
llama_model_loader: - kv   8:                   bert.feed_forward_length u32              = 4096
llama_model_loader: - kv   9:                  bert.attention.head_count u32              = 16
llama_model_loader: - kv  10:          bert.attention.layer_norm_epsilon f32              = 0.000010
llama_model_loader: - kv  11:                          general.file_type u32              = 1
llama_model_loader: - kv  12:                      bert.attention.causal bool             = false
llama_model_loader: - kv  13:                          bert.pooling_type u32              = 2
llama_model_loader: - kv  14:                       tokenizer.ggml.model str              = t5
llama_model_loader: - kv  15:                         tokenizer.ggml.pre str              = default
llama_model_loader: - kv  16:                      tokenizer.ggml.tokens arr[str,250002]  = ["<s>", "<pad>", "</s>", "<unk>", ","...
llama_model_loader: - kv  17:                      tokenizer.ggml.scores arr[f32,250002]  = [0.000000, 0.000000, 0.000000, 0.0000...
llama_model_loader: - kv  18:                  tokenizer.ggml.token_type arr[i32,250002]  = [3, 3, 3, 2, 1, 1, 1, 1, 1, 1, 1, 1, ...
llama_model_loader: - kv  19:            tokenizer.ggml.add_space_prefix bool             = true
llama_model_loader: - kv  20:            tokenizer.ggml.token_type_count u32              = 1
llama_model_loader: - kv  21:    tokenizer.ggml.remove_extra_whitespaces bool             = true
llama_model_loader: - kv  22:        tokenizer.ggml.precompiled_charsmap arr[u8,237539]   = [0, 180, 2, 0, 0, 132, 0, 0, 0, 0, 0,...
llama_model_loader: - kv  23:                tokenizer.ggml.bos_token_id u32              = 0
llama_model_loader: - kv  24:                tokenizer.ggml.eos_token_id u32              = 2
llama_model_loader: - kv  25:            tokenizer.ggml.unknown_token_id u32              = 3
llama_model_loader: - kv  26:          tokenizer.ggml.seperator_token_id u32              = 2
llama_model_loader: - kv  27:            tokenizer.ggml.padding_token_id u32              = 1
llama_model_loader: - kv  28:                tokenizer.ggml.cls_token_id u32              = 0
llama_model_loader: - kv  29:               tokenizer.ggml.mask_token_id u32              = 250001
llama_model_loader: - kv  30:               tokenizer.ggml.add_bos_token bool             = true
llama_model_loader: - kv  31:               tokenizer.ggml.add_eos_token bool             = true
llama_model_loader: - kv  32:               general.quantization_version u32              = 2
llama_model_loader: - type  f32:  244 tensors
llama_model_loader: - type  f16:  145 tensors
print_info: file format = GGUF V3 (latest)
print_info: file type   = F16
print_info: file size   = 1.07 GiB (16.25 BPW)
load: model vocab missing newline token, using special_pad_id instead
load: 0 unused tokens
load: printing all EOG tokens:
load:   - 2 ('</s>')
load: special tokens cache size = 4
load: token to piece cache size = 2.1668 MB
print_info: arch                  = bert
print_info: vocab_only            = 0
print_info: no_alloc              = 0
print_info: n_ctx_train           = 8192
print_info: n_embd_inp            = 1024
print_info: n_embd                = 1024
print_info: n_embd_out            = 1024
print_info: n_layer               = 24
print_info: n_layer_all           = 24
print_info: n_head                = 16
print_info: n_head_kv             = 16
print_info: n_rot                 = 64
print_info: n_swa                 = 0
print_info: is_swa_any            = 0
print_info: n_embd_head_k         = 64
print_info: n_embd_head_v         = 64
print_info: n_gqa                 = 1
print_info: n_embd_k_gqa          = 1024
print_info: n_embd_v_gqa          = 1024
print_info: f_norm_eps            = 1.0e-05
print_info: f_norm_rms_eps        = 0.0e+00
print_info: f_clamp_kqv           = 0.0e+00
print_info: f_max_alibi_bias      = 0.0e+00
print_info: f_logit_scale         = 0.0e+00
print_info: f_attn_scale          = 0.0e+00
print_info: f_attn_value_scale    = 0.0000
print_info: n_ff                  = 4096
print_info: n_expert              = 0
print_info: n_expert_used         = 0
print_info: n_expert_groups       = 0
print_info: n_group_used          = 0
print_info: causal attn           = 0
print_info: pooling type          = 2
print_info: rope type             = 2
print_info: rope scaling          = linear
print_info: freq_base_train       = 10000.0
print_info: freq_scale_train      = 1
print_info: n_ctx_orig_yarn       = 8192
print_info: rope_yarn_log_mul     = 0.0000
print_info: rope_finetuned        = unknown
print_info: model type            = 335M
print_info: model params          = 566.70 M
print_info: general.name          = n/a
print_info: vocab type            = UGM
print_info: n_vocab               = 250002
print_info: n_merges              = 0
print_info: BOS token             = 0 '<s>'
print_info: EOS token             = 2 '</s>'
print_info: UNK token             = 3 '<unk>'
print_info: SEP token             = 2 '</s>'
print_info: PAD token             = 1 '<pad>'
print_info: MASK token            = 250001 '[PAD250000]'
print_info: LF token              = 0 '<s>'
print_info: EOG token             = 2 '</s>'
print_info: max token length      = 48
load_tensors: loading model tensors, this can take a while... (load_mode = none)
load_tensors:          CPU model buffer size =  1097.52 MiB
cmn  common_init_: added </s> logit bias = -inf
llama_context: constructing llama_context
llama_context: n_seq_max             = 1
llama_context: n_ctx                 = 4096
llama_context: n_ctx_seq             = 4096
llama_context: n_batch               = 2048
llama_context: n_ubatch              = 2048
llama_context: causal_attn           = 0
llama_context: flash_attn            = auto
llama_context: kv_unified            = false
llama_context: freq_base             = 10000.0
llama_context: freq_scale            = 1
llama_context: n_rs_seq              = 0
llama_context: n_outputs_max         = 2048
llama_context: n_outputs_max_per_seq = 1
llama_context: n_ctx_seq (4096) < n_ctx_train (8192) -- the full capacity of the model will not be utilized
llama_context:        CPU  output buffer size =     0.96 MiB
sched_reserve: reserving ...
resolve_fused_ops: Flash Attention enabled
resolve_fused_ops: resolving fused Gated Delta Net support:
resolve_fused_ops: fused Gated Delta Net (autoregressive) enabled
resolve_fused_ops: fused Gated Delta Net (chunked) enabled
resolve_fused_ops: resolving fused Lightning Indexer support:
resolve_fused_ops: Lightning Indexer enabled
resolve_fused_ops: resolving fused DeepSeek V4 HC support:
resolve_fused_ops: fused DeepSeek V4 HC pre enabled
resolve_fused_ops: fused DeepSeek V4 HC comb enabled
resolve_fused_ops: fused DeepSeek V4 HC post enabled
sched_reserve:        CPU compute buffer size =    64.02 MiB
sched_reserve: graph nodes  = 779
sched_reserve: graph splits = 1
sched_reserve: reserve took 4.55 ms, sched copies = 1
cmn          init: llama threadpool init, n_threads = 8
cmn  common_init_: KV cache shifting is not supported for this context, disabling KV cache shifting
cmn  common_init_: warming up the model with an empty run - please wait ... (--no-warmup to disable)
srv    load_model: speculative decoding not supported by this context
srv    load_model: initializing, n_slots = 1, n_ctx_slot = 4096, kv_unified = 'false'
slot   load_model: id  0 | task -1 | new slot, n_ctx = 4096
srv    load_model: prompt cache is enabled, size limit: 8192 MiB
srv    load_model: use `--cache-ram 0` to disable the prompt cache
srv    load_model: for more info see https://github.com/ggml-org/llama.cpp/pull/16391
srv    load_model: context checkpoints enabled, max = 32, min spacing = 8192
srv          init: idle slots will be saved to prompt cache upon starting a new task
srv          init: init: chat template, example_format: '<|im_start|>system
You are a helpful assistant<|im_end|>
<|im_start|>user
Hello<|im_end|>
<|im_start|>assistant
Hi there<|im_end|>
<|im_start|>user
How are you?<|im_end|>
<|im_start|>assistant
'
srv          init: init: chat template, thinking = 0
srv  llama_server: model loaded
srv  llama_server: listening on http://127.0.0.1:39083
srv  update_slots: all slots are idle
time=2026-09-02T16:53:44.432Z level=INFO source=llama_server.go:1362 msg="llama-server started in 15.09 seconds"
time=2026-09-02T16:53:44.438Z level=INFO source=images.go:381 msg="template selection" model=registry.ollama.ai/library/bge-m3:latest selected=none renderer="" parser="" go_template=null chat_template=null harmony=null renderer_parser=null
time=2026-09-02T16:53:44.438Z level=INFO source=sched.go:728 msg="loaded runners" count=2
time=2026-09-02T16:53:44.440Z level=INFO source=llama_server.go:1295 msg="waiting for llama-server to start responding"
time=2026-09-02T16:53:44.440Z level=INFO source=llama_server.go:1362 msg="llama-server started in 15.10 seconds"
slot get_availabl: id  0 | task -1 |  - skipping, slot is empty
slot get_availabl: id  0 | task -1 | selected slot by LRU, t_last = -1
slot launch_slot_: id  0 | task 0 | processing task, is_child = 0
slot   operator(): id  0 | task 0 | new prompt, n_ctx_slot = 4096, n_keep = 0, task.n_tokens = 14
slot   operator(): id  0 | task 0 | cached n_tokens = 0, memory_seq_rm [0, end)
slot      release: id  0 | task 0 | stop processing: n_tokens = 14, truncated = 0
srv  update_slots: all slots are idle
slot get_availabl: id  0 | task -1 |  - checking sim = 1.000 (14/14) > 0.100
slot get_availabl: id  0 | task -1 | selected slot by LCP similarity, f_sim_best = 1.000 (> 0.100 thold), f_keep = 1.000
slot launch_slot_: id  0 | task 2 | processing task, is_child = 0
slot   operator(): id  0 | task 2 | new prompt, n_ctx_slot = 4096, n_keep = 0, task.n_tokens = 14
slot   operator(): id  0 | task 2 | cached n_tokens = 0, memory_seq_rm [0, end)
slot      release: id  0 | task 2 | stop processing: n_tokens = 14, truncated = 0
srv  update_slots: all slots are idle
srv  server_strea: conv_id= (empty=1)
srv    operator(): chat format: peg-native
slot get_availabl: id  0 | task -1 |  - skipping, slot is empty
slot get_availabl: id  1 | task -1 |  - checking sim = 0.002 (5/2921) > 0.100
slot get_availabl: id  0 | task -1 | selected slot by LRU, t_last = -1
srv  get_availabl: updating prompt cache
srv          load:  - looking for better prompt, base f_keep = -1.000, f_sim = 0.000
srv        update:  - cache state: 0 prompts, 0.000 MiB (limits: 8192.000 MiB, 8192 tokens, 8589934592 est)
srv  get_availabl: prompt cache update took 0.01 ms
slot launch_slot_: id  0 | task -1 | sampler chain: logits -> penalties -> ?dry -> ?top-n-sigma -> top-k -> ?typical -> ?top-p -> ?min-p -> ?xtc -> temp-ext -> dist
slot launch_slot_: id  0 | task -1 | sampler params:
        repeat_last_n = 64, repeat_penalty = 1.000, frequency_penalty = 0.000, presence_penalty = 0.100
        dry_multiplier = 0.000, dry_base = 1.750, dry_allowed_length = 2, dry_penalty_last_n = 64
        top_k = 40, top_p = 1.000, min_p = 0.000, xtc_probability = 0.000, xtc_threshold = 0.100, typical_p = 1.000, top_n_sigma = -1.000, temp = 0.200
        mirostat = 0, mirostat_lr = 0.100, mirostat_ent = 5.000, adaptive_target = -1.000, adaptive_decay = 0.900
slot launch_slot_: id  0 | task 43 | processing task, is_child = 0
slot process_sing: id  1 | task -1 | saving idle slot to prompt cache
srv   prompt_save:  - saving prompt with length 912, total state size = 92.636 MiB (draft: 0.000 MiB)
srv        update:  - cache state: 1 prompts, 92.636 MiB (limits: 8192.000 MiB, 8192 tokens, 80650 est)
srv        update:    - prompt 0x32b7c670:     912 tokens, checkpoints:  0,    92.636 MiB
slot   operator(): id  0 | task 43 | new prompt, n_ctx_slot = 4096, n_keep = 4, task.n_tokens = 2921
slot   operator(): id  0 | task 43 | cached n_tokens = 0, memory_seq_rm [0, end)
