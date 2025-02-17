<div align="center">

<h1>GPT-SoVITS-WebUI</h1>
パワフルな数発音声変換・音声合成 WebUI。<br><br>

[![madewithlove](https://img.shields.io/badge/made_with-%E2%9D%A4-red?style=for-the-badge&labelColor=orange
)](https://github.com/RVC-Boss/GPT-SoVITS)

<img src="https://counter.seku.su/cmoe?name=gptsovits&theme=r34" /><br>

[![Licence](https://img.shields.io/badge/LICENSE-MIT-green.svg?style=for-the-badge)](https://github.com/RVC-Boss/GPT-SoVITS/blob/main/LICENSE)
[![Huggingface](https://img.shields.io/badge/🤗%20-Models%20Repo-yellow.svg?style=for-the-badge)](https://huggingface.co/lj1995/GPT-SoVITS/tree/main)

[**English**](../../README.md) | [**中文简体**](../cn/README.md) | [**日本語**](./README.md)

</div>

------



> [デモ動画](https://www.bilibili.com/video/BV12g4y1m7Uw)をチェック！

https://github.com/RVC-Boss/GPT-SoVITS/assets/129054828/05bee1fa-bdd8-4d85-9350-80c060ab47fb

## 機能:
1. **ゼロショット TTS:** 5秒間のボーカルサンプルを入力すると、即座にテキストから音声に変換されます。

2. **数ショット TTS:** わずか1分間のトレーニングデータでモデルを微調整し、音声の類似性とリアリズムを向上。

3. **多言語サポート:** 現在、英語、日本語、中国語をサポートしています。

4. **WebUI ツール:** 統合されたツールには、音声伴奏の分離、トレーニングセットの自動セグメンテーション、中国語 ASR、テキストラベリングが含まれ、初心者がトレーニングデータセットと GPT/SoVITS モデルを作成するのを支援します。

## 環境の準備

Windows ユーザーであれば（win>=10 にてテスト済み）、prezip 経由で直接インストールできます。[prezip](https://huggingface.co/lj1995/GPT-SoVITS-windows-package/resolve/main/GPT-SoVITS-beta.7z?download=true) をダウンロードして解凍し、go-webui.bat をダブルクリックするだけで GPT-SoVITS-WebUI が起動します。

### Python と PyTorch のバージョン
- Python 3.9, PyTorch 2.0.1, CUDA 11
- Python 3.10.13, PyTorch 2.1.2, CUDA 12.3
- Python 3.9, PyTorch 2.3.0.dev20240122, macOS 14.3 (Apple silicon, GPU)

_注記: numba==0.56.4 は py<3.11 が必要です_

### Macユーザーへ
如果あなたがMacユーザーである場合、GPUを使用してトレーニングおよび推論を行うために以下の条件を満たしていることを確認してください：
- AppleシリコンまたはAMD GPUを搭載したMacコンピューター
- macOS 12.3以降
- `xcode-select --install`を実行してインストールされたXcodeコマンドラインツール

_その他のMacはCPUのみで推論を行うことができます。_

次に、以下のコマンドを使用してインストールします：
#### 環境作成
```bash
conda create -n GPTSoVits python=3.9
conda activate GPTSoVits
```
#### Pip パッケージ
```bash
pip install -r requirements.txt
pip uninstall torch torchaudio
pip3 install --pre torch torchaudio --index-url https://download.pytorch.org/whl/nightly/cpu
```
_注記: UVR5を使用して前処理を行う場合は、[オリジナルプロジェクトのGUIをダウンロード](https://github.com/Anjok07/ultimatevocalremovergui)して、「GPU Conversion」を選択することをお勧めします。さらに、特に推論時にメモリリークの問題が発生する可能性があります。推論webUIを再起動することでメモリを解放することができます。_
### Conda によるクイックインストール

```bash
conda create -n GPTSoVits python=3.9
conda activate GPTSoVits
bash install.sh
```
### 手動インストール

#### Pip パッケージ

```bash
pip install -r requirementx.txt
```

#### FFmpeg

##### Conda ユーザー
```bash
conda install ffmpeg
```

##### Ubuntu/Debian ユーザー

```bash
sudo apt install ffmpeg
sudo apt install libsox-dev
conda install -c conda-forge 'ffmpeg<7'
```

##### MacOS ユーザー

```bash
brew install ffmpeg
```

##### Windows ユーザー

[ffmpeg.exe](https://huggingface.co/lj1995/VoiceConversionWebUI/blob/main/ffmpeg.exe) と [ffprobe.exe](https://huggingface.co/lj1995/VoiceConversionWebUI/blob/main/ffprobe.exe) をダウンロードし、GPT-SoVITS のルートディレクトリに置きます。

### Dockerの使用

#### docker-compose.yamlの設定

1. 環境変数：
    - `is_half`：半精度／倍精度の制御。"SSL抽出"ステップ中に`4-cnhubert/5-wav32k`ディレクトリ内の内容が正しく生成されない場合、通常これが原因です。実際の状況に応じてTrueまたはFalseに調整してください。

2. ボリューム設定：コンテナ内のアプリケーションのルートディレクトリは`/workspace`に設定されます。デフォルトの`docker-compose.yaml`には、アップロード／ダウンロードの内容の実例がいくつか記載されています。
3. `shm_size`：WindowsのDocker Desktopのデフォルトの利用可能メモリが小さすぎるため、異常な動作を引き起こす可能性があります。状況に応じて適宜設定してください。
4. `deploy`セクションのGPUに関連する内容は、システムと実際の状況に応じて慎重に設定してください。

#### docker composeで実行する
```markdown
docker compose -f "docker-compose.yaml" up -d
```

#### dockerコマンドで実行する

上記と同様に、実際の状況に基づいて対応するパラメータを変更し、次のコマンドを実行します：
```markdown
docker run --rm -it --gpus=all --env=is_half=False --volume=G:\GPT-SoVITS-DockerTest\output:/workspace/output --volume=G:\GPT-SoVITS-DockerTest\logs:/workspace/logs --volume=G:\GPT-SoVITS-DockerTest\SoVITS_weights:/workspace/SoVITS_weights --workdir=/workspace -p 9870:9870 -p 9871:9871 -p 9872:9872 -p 9873:9873 -p 9874:9874 --shm-size="16G" -d breakstring/gpt-sovits:dev-20240123.03
```


### 事前訓練済みモデル


[GPT-SoVITS Models](https://huggingface.co/lj1995/GPT-SoVITS) から事前訓練済みモデルをダウンロードし、`GPT_SoVITSpretrained_models` に置きます。

中国語 ASR（追加）については、[Damo ASR Model](https://modelscope.cn/models/damo/speech_paraformer-large_asr_nat-zh-cn-16k-common-vocab8404-pytorch/files)、[Damo VAD Model](https://modelscope.cn/models/damo/speech_fsmn_vad_zh-cn-16k-common-pytorch/files)、[Damo Punc Model](https://modelscope.cn/models/damo/punc_ct-transformer_zh-cn-common-vocab272727-pytorch/files) からモデルをダウンロードし、`tools/damo_asr/models` に置いてください。

UVR5 (Vocals/Accompaniment Separation & Reverberation Removal, additionally) の場合は、[UVR5 Weights](https://huggingface.co/lj1995/VoiceConversionWebUI/tree/main/uvr5_weights) からモデルをダウンロードして `tools/uvr5/uvr5_weights` に置きます。


## データセット形式

TTS アノテーション .list ファイル形式:

```
vocal_path|speaker_name|language|text
```

言語辞書:

- 'zh': 中国語
- 'ja': 日本語
- 'en': 英語

例:

```
D:\GPT-SoVITS\xxx/xxx.wav|xxx|en|I like playing Genshin.
```
## Todo リスト

- [ ] **優先度 高:**
   - [ ] 日本語と英語でのローカライズ。
   - [ ] ユーザーガイド。
   - [ ] 日本語データセットと英語データセットのファインチューニングトレーニング。

- [ ] **機能:**
   - [ ] ゼロショット音声変換（5秒）／数ショット音声変換（1分）。
   - [ ] TTS スピーキングスピードコントロール。
   - [ ] TTS の感情コントロールの強化。
   - [ ] SoVITS トークン入力を語彙の確率分布に変更する実験。
   - [ ] 英語と日本語のテキストフロントエンドを改善。
   - [ ] 小型と大型の TTS モデルを開発する。
   - [ ] Colab のスクリプト。
   - [ ] トレーニングデータセットを拡張する（2k→10k）。
   - [ ] より良い sovits ベースモデル（音質向上）
   - [ ] モデルミックス

## クレジット

以下のプロジェクトとコントリビューターに感謝します:

- [ar-vits](https://github.com/innnky/ar-vits)
- [SoundStorm](https://github.com/yangdongchao/SoundStorm/tree/master/soundstorm/s1/AR)
- [vits](https://github.com/jaywalnut310/vits)
- [TransferTTS](https://github.com/hcy71o/TransferTTS/blob/master/models.py#L556)
- [Chinese Speech Pretrain](https://github.com/TencentGameMate/chinese_speech_pretrain)
- [contentvec](https://github.com/auspicious3000/contentvec/)
- [hifi-gan](https://github.com/jik876/hifi-gan)
- [Chinese-Roberta-WWM-Ext-Large](https://huggingface.co/hfl/chinese-roberta-wwm-ext-large)
- [fish-speech](https://github.com/fishaudio/fish-speech/blob/main/tools/llama/generate.py#L41)
- [ultimatevocalremovergui](https://github.com/Anjok07/ultimatevocalremovergui)
- [audio-slicer](https://github.com/openvpi/audio-slicer)
- [SubFix](https://github.com/cronrpc/SubFix)
- [FFmpeg](https://github.com/FFmpeg/FFmpeg)
- [gradio](https://github.com/gradio-app/gradio)

## すべてのコントリビューターに感謝します
<a href="https://github.com/RVC-Boss/GPT-SoVITS/graphs/contributors" target="_blank">
  <img src="https://contrib.rocks/image?repo=RVC-Boss/GPT-SoVITS" />
</a>
