# 聂莫黎全量资源清单与用途

> 整理时间：2026-11-05 18:45
> 背景：GPU 服务器（RTX 4060 Ti 16G）当日 24:00 回收，盘上资产会全部清空。本文档说明**每一个资源是什么、干什么用、接手人怎么用**，并标明哪些是必需的、哪些只是归档。
> 所有资产均为《纸嫁衣》聂莫黎相关语音/歌声数据，仅作个人技术研究，不得商用、不得二次分发音频。

---

## 0. 一句话结论

- **语音模型（GPT-SoVITS）**：已训练完成并发布，明天可直接加载使用。
- **虚拟歌姬声库（DiffSinger）**：训练数据、标注、配置**全部齐备**，但训练**一步都没跑**，**没有任何声库包**。
- **本次归档**：把 GPU 机上剩余约 14GB 全部推到 GitHub，保证明天换机器后不丢东西。

---

## 1. 仓库地图

| 仓库 | 内容 | Release |
|---|---|---|
| `ceshi3hao/niemoli-gpt-sovits` | 语音模型成品权重（2 个） | `v1.0` |
| `ceshi3hao/niemoli-voice-dataset` | 对白语音素材 + 干净训练集 | `v1.0`（原始 1.27GB）、`v2.0-clean`（1349 条） |
| `ceshi3hao/niemoli-song-dataset` | **歌声数据集**：599 条标注 + 12 首分离人声 + 配置/字典/脚本 | `v1.0-song-assets`、`backup-20261105-verified` |
| `ceshi3hao/niemoli-full-archive` | **本次全量归档**：对白中间数据 + 全部权重 + 全部脚本 | `v2.0-all-assets`（logs/pado/gsv_data/gsv_weights_v2pro/pb8 分卷）、`v2.1-v2proplus`（v2ProPlus 权重单独分卷） |

> **注意**：v2ProPlus 权重（约 6.1G / 68 卷）因单个 Release 资产过多会触发 GitHub 上传卡住，单独放在 `v2.1-v2proplus`。**两个 Release 都要下载**。仓库根目录 `README.md` 与 Release 资产 `ASSETS_GUIDE.md` 均为本文档。

---

## 2. 三条链路，先分清哪个是哪个

这是最容易搞混的地方，接手人务必先看这一节。

### 链路 A：语音模型 / 对白（GPT-SoVITS、RVC）
目标是让角色「说话」。数据来源是游戏对白。产物是 `niemoli2-e20.ckpt` + `niemoli2_e20_s1040.pth`。
**这条链路已经做完**，GitHub 上的 v1.0 可直接用于推理。

### 链路 B：虚拟歌姬声库 / 唱歌（DiffSinger）
目标是让角色「唱歌」。数据来源是 12 首 QQ 音乐 AI 翻唱，经 UVR-MDX 分离人声、切片、ASR 标注。
产物应当是 DiffSinger acoustic + variance 模型 + 声码器 → OpenUtau 声库包。
**这条链路数据齐、训练 0 步、无成品。** 明天要继续做的就是这条。

### 链路 C：原始/中间素材（归档用）
`gsv_data`、`pb8`、`niemoli-dataset` 属于对白链路的中间产物和原始素材。
它们**对链路 B（声库）没有直接用途**，但保留了「如果语音模型要重训/换版本/做 A/B」的全部输入。之前未上传，本次一并归档，不做删减。

---

## 3. 逐项资产表

### 3.1 已在 GitHub（此前上传）

| 资源 | 体积 | 内容 | 用途 | 状态 |
|---|---|---|---|---|
| `niemoli2-e20.ckpt` | 148.1 MB | GPT-SoVITS v2Pro GPT 权重，epoch 20 | 语音模型推理必需 | ✅ `niemoli-gpt-sovits` v1.0 |
| `niemoli2_e20_s1040.pth` | 128.7 MB | SoVITS v2Pro 声学权重，e20 s1040 | 语音模型推理必需 | ✅ 同上 |
| 对白素材包 v1.0 | 1.27 GB / 14 分卷 | 17 份音频 / 211 分钟，纸嫁衣系列原始语音 | 链路 A 原始素材 | ✅ `niemoli-voice-dataset` v1.0 |
| 干净训练集 v2.0-clean | 847 MB / 10 分卷 | A 类 1349 条可用 WAV（高音质 382 / 普通 967）+ 分组试听 36 条 + 审查报告 + `gpu_train/bootstrap.sh` | 链路 A 重训时的干净输入 | ✅ 同上 v2.0-clean |
| `A_ds_labels_599.tar.gz` | 192.5 MB | 599 条 `.ds` 标注 + 599 个 16k 单声道切片 wav（`ds/s01_L000.ds` … `s12_Lxxx`） | **链路 B 二值化直接输入，最重要** | ✅ `niemoli-song-dataset` |
| `B_sep_vocals_12.tar.zst` | 210.4 MB | 12 首歌 UVR-MDX 分离人声，无损 FLAC 44.1k（`s01_vocals.flac` … `s12_vocals.flac`） | 链路 B 音源，重跑分离的备份 | ✅ 同上 |
| `C_configs_scripts.tar.gz` | 48 KB | `acoustic.yaml` / `variance.yaml` / `duration.yaml`、`niemoli.txt` 字典（601 行）、`transcriptions.csv`（599 行）、全流程 py/sh、11 首 lrc | 链路 B 二值化 + 训练配置 | ✅ 同上（仓库内也有平铺 31 个文件） |
| `metadata.tar.gz` | 1.3 MB | 标注/配置/字典/切片元数据校验备份 | 校验用 | ✅ `backup-20261105-verified` |

### 3.2 本次新增归档（原 GPU 机盘上，24:00 会消失）

| 包 | 源路径 | 原始体积 | 内容 | 用途 |
|---|---|---|---|---|
| `gsv_data` | `/data/gsv_data` | 1.6 G | GPT-SoVITS 8 套训练数据：`cw` 59M、`nm` 56M、`niemoli` 164M、`nm_v8` 183M、`nm_v8_raw` 184M、`nm2` 219M、`nm2_raw` 220M、`mid` 459M，各含音频 + `list*.txt` / `labels*.json` | 链路 A 重训/复现实验。**声库用不到** |
| `pb8` | `/data/pb8` | 3.5 G | 纸8 对白素材：`raw` 2.5G + `sep` 507M + `seg` 507M，另有 `align*.json`、`asr_pb8.json`、28 个 `.py` | 链路 A 对齐/ASR/切分全流程留档。**声库用不到** |
| `pado_raw` | `/data/pado/raw` | 266 M | 12 首歌原始文件（24 个 `.m4a` + `.wav`） | 链路 B 原始音源，若怀疑分离质量可重跑 UVR |
| `gsv_weights_v2pro` | `/data/GPT-SoVITS/{GPT,SoVITS}_weights_v2Pro/` | 2.3 G | 16 个权重：GPT 8 个 ckpt（`niemoli_raw-e{5,10,15,20}`、`niemoli2-e{5,10,15,20}`）、SoVITS 8 个 pth（同两组） | 链路 A 训练轨迹/选点。**只有 e20 两个是对外发布的**，其余供 A/B |
| `v2proplus_weights` | `/data/niemoli-run-20261105/experiment-recovery-1/weights/s2/` + `experiment/features/logs_s2_v2ProPlus/G_233333333333.pth` | 约 6.1 G | **30 个** v2ProPlus checkpoint（`niemoli_experiment_e1_s96` … `e30_s2880`，各约 165MB）+ 主产物 `G_233333333333.pth` 1.18G | 链路 A 新架构实验成果，**从未发布、从未与 v2Pro e20 做 A/B**，是最值得验证的一项 |
| `scripts_all` | `/data/*.py`(12) + `/data/pb8/*.py`(28) + `/data/pado/*.py,*.sh` + `/data/bk/*.tar.gz` + 训练日志 | 数 MB | `gsv_pipeline.py` / `gsv_prep.py` / `gsv_asr.py` / `gsv_train.py`、`align_pb8.py`、`asr_pb8.py`、`make_ds_song.py`、`patch_song.py`、`dur.py`、`fixname.py`、`split_ds.py` 等全量脚本；`method.tar.gz` 423KB（三份配置+脚本+字典+训练日志）、`ds_labels.tar.gz` 631KB | **体积最小、价值密度最高**：复现全部数据处理流程。此前只在文档里写"应最先拉走"却没传 |

> 注：交接文档 4.10 曾写 v2ProPlus 有 9 个权重，实测为 **30 个**（4.9G），以本表为准。

### 3.3 不需要搬迁

`/data/ds-venv` 6.4G、`/data/miniconda` 6.6G、`/data/sepenv`、`/data/modelscope` 1.1G、`/data/ms_cache` 2.1G、`/data/rvc-niemoli` 1.3G（**只有预训练底模 `f0G40k.pth`/`f0D40k.pth`，无训练成果**）——都是环境，重装即可。

---

## 4. 还原方法

分卷为纯字节切分（每卷约 90 MB，`v2.1` 实际每卷 92 MB / 末卷不足一卷），按顺序拼接后 zstd 解压。**先把同一个包的所有分卷下到同一目录**，文件名按 `part_000` 递增，shell 通配符顺序即正确顺序：

```bash
# 以 gsv_data 为例，其他包把前缀换掉即可
cat gsv_data.part_* > gsv_data.tar.zst
zstd -d gsv_data.tar.zst          # -> gsv_data.tar
tar -xf gsv_data.tar
```

各包前缀与所在 Release：

| 前缀 | 来源 Release | 卷数 |
|---|---|---|
| `logs_all` / `pado_raw` / `pado_misc` / `scripts_all` | `v2.0-all-assets` | 各 1-3 |
| `gsv_data` | `v2.0-all-assets` | 14 |
| `gsv_weights_v2pro` | `v2.0-all-assets` | 23 |
| `pb8` | `v2.0-all-assets` | 31 |
| `v2proplus_weights` | **`v2.1-v2proplus`** | 61（已校验：序号连续、全部 uploaded、总 5,877,972,357 字节与源包一致） |

校验：`cat *.part_* > x.tar.zst && zstd -t x.tar.zst && tar -tf x.tar | head`，能列出文件即完整。

已上传的 `niemoli-voice-dataset` 分卷同理（`cat niemoli-clean-train-pack.part_* > ...zip && unzip`），详见各仓库 `RESTORE.md`。

歌声数据集还原与训练摆法：

```bash
tar -xzf A_ds_labels_599.tar.gz                       # -> ds/
tar -xzf C_configs_scripts.tar.gz                     # -> configs/ dictionary/ labels/ scripts/ lyrics/
zstd -d B_sep_vocals_12.tar.zst && tar -xf B_sep_vocals_12.tar   # -> *.flac

mkdir -p data/niemoli_song/raw
cp -r ds/* data/niemoli_song/raw/
cp labels/transcriptions.csv data/niemoli_song/raw/
cp dictionary/niemoli.txt dictionaries/

python data/binarize.py --config configs/acoustic.yaml
python data/binarize.py --config configs/variance.yaml
python tasks/acoustic_task.py  --config configs/acoustic.yaml
python tasks/variance_task.py  --config configs/variance.yaml
```

---

## 5. 已知风险与未验证结论（接手人务必先看）

1. **本版本 DiffSinger 没有 `duration_task.py`**，只有 `acoustic_task.py` 和 `variance_task.py`；时长由 variance 模型一并预测。照抄网上教程会踩空。
2. **binarize 只认 `raw/` 下的 `transcriptions.csv` + 同级 wav**，直接丢一堆 `.ds` 会报错。
3. **声码器缺失**：`pc_nsf_hifigan_44.1k_hop512_128bin_2025.02/model.ckpt` 与 `config.json` 需从 `github.com/openvpi/vocoders/releases` 自行下载放入 `checkpoints/`。原机器该路径是空的。
4. **字典里不能有 `AP` 行**（已删）。若 binarize 报 `AP not found`，检查这点。删后**尚未重跑二值化验证**。
5. variance 配置中 `predict_energy: false`、`predict_breathiness: false`，只出 duration + pitch。
6. **599 条标注未经人工复核**（whisperX 自动对齐），可能有错字/错切，这是已知质量风险。
7. **v2ProPlus 30 个权重从未与已发布的 v2Pro e20 做 A/B**，哪个效果更好没有结论。
8. **声库训练 0 步**，不存在任何 DiffSinger checkpoint 或 OpenUtau 声库包。文档里所有"训练步骤"均为待执行，不是已完成。

---

## 6. 明天怎么继续

**优先级 1（保住数据）**：本仓库 `v2.0-all-assets` 全部分卷下载校验通过。原 GPU 机 24:00 后不可再依赖。

**优先级 2（做声库）**：开 RTX-4090 24G（约 1.86 元/卡时），建议买 10 小时。理由：Ada 架构 sm_89，PyTorch 2.x 原生支持，环境最稳；算力约为 4060 Ti 的 3.7 倍、显存 24G 可把 batch 翻倍，10 万步约 3.5-4.5 小时。不建议 5090（Blackwell sm_120 需较新 CUDA/torch，旧项目依赖容易踩坑）。

**优先级 3（别再空转）**：训练前必须先跑 dry-run 全流程 2 步（依赖→声码器→二值化→训练→export→打包→上传），并做故障注入验证；把监督交给服务器上的常驻 Agent，不要依赖人工盯。

**优先级 4（顺带）**：v2ProPlus 与 v2Pro e20 做一次 A/B，确认哪个语音模型更好。

---

## 7. 训练监督 Agent 已验证的接入参数

- 模型接口：`POST https://ark.cn-beijing.volces.com/api/coding/v3/responses`
- 协议：`wire_api = "responses"`（**不是** `/chat/completions`）
- 模型名：`glm-5.3-flash` / `doubao-seed-2.1-pro` / `ark-code-latest`（不带日期后缀的 Model Name）
- Base URL：OpenAI 协议 `https://ark.cn-beijing.volces.com/api/coding/v3`；Anthropic 协议 `https://ark.cn-beijing.volces.com/api/coding`
- 禁止使用 `https://ark.cn-beijing.volces.com/api/v3`（不消耗套餐额度，会产生额外费用）
- function calling 已实测可用：给「最近 3 分钟 step 未增长」的上下文，模型能返回 `restart_training` 及带理由的参数
- 上传通道：GPU 机直传 `uploads.github.com`，实测 7.75 MB/s，**不需要本地中转**；单文件 2GB 上限，用 90MB 分卷
