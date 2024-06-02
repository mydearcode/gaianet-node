# Kendi GaiaNet düğümünüzü çalıştırın

<p align="center">
  <a href="https://discord.gg/gaianet-ai">
    <img src="https://img.shields.io/badge/chat-Discord-7289DA?logo=discord" alt="GaiaNet Discord">
  </a>
  <a href="https://twitter.com/Gaianet_AI">
    <img src="https://img.shields.io/badge/Twitter-1DA1F2?logo=twitter&amp;logoColor=white" alt="GaiaNet Twitter">
  </a>
   <a href="https://www.gaianet.ai/">
    <img src="https://img.shields.io/website?up_message=Website&url=https://www.gaianet.ai/" alt="Gaianet website">
  </a>
</p>

## Hızlı başlangıç

Mac, Linux veya Windows WSL'de tek bir komut satırıyla varsayılan düğüm yazılım yığınını yükleyin.

```bash
curl -sSfL 'https://raw.githubusercontent.com/GaiaNet-AI/gaianet-node/main/install.sh' | bash
```

Düğümü başlatın. Bu işlem `$HOME/gaianet/config.json` dosyasında belirtilen model dosyalarını ve vektör veritabanı dosyalarını indirecektir ve dosyalar büyük olduğu için birkaç dakika sürebilir.

```bash
gaianet init
```

Düğümü başlatın.

```bash
gaianet start
```

Kod, resmi düğüm adresini konsola aşağıdaki gibi yazdırır.
Düğüm bilgilerini görmek için bu URL'ye bir tarayıcı açabilir ve ardından düğümdeki AI aracısı ile sohbet edebilirsiniz.

```
... ... https://0xf63939431ee11267f4855a166e11cc44d24960c0.gaianet.xyz
```

Düğümü durdurmak için aşağıdaki komut dosyasını çalıştırabilirsiniz.

```bash
gaianet stop
```

## Kurulum kılavuzu

```bash
curl -sSfL 'https://raw.githubusercontent.com/GaiaNet-AI/gaianet-node/main/install.sh' | bash
```

<details><summary> Çıktı aşağıdaki gibi görünmelidir: </summary>

```
[+] Downloading default config file ...

[+] Downloading nodeid.json ...

[+] Installing WasmEdge with wasi-nn_ggml plugin ...

Info: Detected Linux-x86_64

Info: WasmEdge Installation at /home/azureuser/.wasmedge

Info: Fetching WasmEdge-0.13.5

/tmp/wasmedge.2884467 ~/gaianet
######################################################################## 100.0%
~/gaianet
Info: Fetching WasmEdge-GGML-Plugin

Info: Detected CUDA version:

/tmp/wasmedge.2884467 ~/gaianet
######################################################################## 100.0%
~/gaianet
Installation of wasmedge-0.13.5 successful
WasmEdge binaries accessible

    The WasmEdge Runtime wasmedge version 0.13.5 is installed in /home/azureuser/.wasmedge/bin/wasmedge.


[+] Installing Qdrant binary...
    * Download Qdrant binary
################################################################################################## 100.0%

    * Initialize Qdrant directory

[+] Downloading the rag-api-server.wasm ...
################################################################################################## 100.0%

[+] Downloading dashboard ...
################################################################################################## 100.0%
```

</details>

Varsayılan olarak `$HOME/gaianet` dizinine yüklenir. Alternatif bir dizine yüklemeyi de seçebilirsiniz.

```bash
curl -sSfL 'https://raw.githubusercontent.com/GaiaNet-AI/gaianet-node/main/install.sh' | bash -s -- --base $HOME/gaianet.alt
```

## Düğümü başlatın

```
gaianet init
```

<details><summary> Çıktı aşağıdaki gibi görünmelidir: </summary>

```bash
[+] Downloading Llama-2-7b-chat-hf-Q5_K_M.gguf ...
############################################################################################################################## 100.0%############################################################################################################################## 100.0%

[+] Downloading all-MiniLM-L6-v2-ggml-model-f16.gguf ...

############################################################################################################################## 100.0%############################################################################################################################## 100.0%

[+] Creating 'default' collection in the Qdrant instance ...

    * Start a Qdrant instance ...

    * Remove the existed 'default' Qdrant collection ...

    * Download Qdrant collection snapshot ...
############################################################################################################################## 100.0%############################################################################################################################## 100.0%

    * Import the Qdrant collection snapshot ...

    * Recovery is done successfully
```
</details>

init` komutu düğümü `$HOME/gaianet/config.json` dosyasına göre başlatır. Önceden ayarlanmış konfigürasyonlarımızdan bazılarını kullanabilirsiniz.

```bash
gaianet init --config https://raw.githubusercontent.com/GaiaNet-AI/node-configs/main/llama-3-8b-instruct_london/config.json
```

Önceden ayarlanmış yapılandırmaların bir listesini görmek için `gaianet init --help` yapabilirsiniz.
gaianet_docs` gibi önceden ayarlanmış yapılandırmaların yanı sıra, düğümün istediğiniz duruma başlatılması için kendi `config.json` dosyanıza bir URL de iletebilirsiniz.

Alternatif bir dizinde kurulu bir düğümü `init` etmeniz gerekiyorsa, bunu yapın.

```bash
gaianet init --base $HOME/gaianet.alt
```

## Düğümü başlat

```
gaianet start
```

<details><summary> Çıktı aşağıdaki gibi görünmelidir: </summary>

```bash
[+] Starting Qdrant instance ...

    Qdrant instance started with pid: 39762

[+] Starting LlamaEdge API Server ...

    Run the following command to start the LlamaEdge API Server:

wasmedge --dir .:./dashboard --nn-preload default:GGML:AUTO:Llama-2-7b-chat-hf-Q5_K_M.gguf --nn-preload embedding:GGML:AUTO:all-MiniLM-L6-v2-ggml-model-f16.gguf rag-api-server.wasm --model-name Llama-2-7b-chat-hf-Q5_K_M,all-MiniLM-L6-v2-ggml-model-f16 --ctx-size 4096,384 --prompt-template llama-2-chat --qdrant-collection-name default --web-ui ./ --socket-addr 0.0.0.0:8080 --log-prompts --log-stat --rag-prompt "Use the following pieces of context to answer the user's question.\nIf you don't know the answer, just say that you don't know, don't try to make up an answer.\n----------------\n"


    LlamaEdge API Server started with pid: 39796
```

</details>

Düğümü yerel kullanım için başlatabilirsiniz. Yalnızca `localhost` üzerinden erişilebilir olacak ve GaiaNet etki alanının genel URL'lerinden hiçbirinde bulunmayacaktır.

```bash
gaianet start --local-only
```

Alternatif bir temel dizinde yüklü bir düğümü de başlatabilirsiniz.

```bash
gaianet start --base $HOME/gaianet.alt
```

### Düğümü durdur

```bash
gaianet stop
```

<details><summary> Çıktı aşağıdaki gibi görünmelidir: </summary>

```bash
[+] Stopping WasmEdge, Qdrant and frpc ...
```

</details>

Alternatif bir temel dizinde yüklü bir düğümü durdurun.

```bash
gaianet stop --base $HOME/gaianet.alt
```

### Yapılandırmayı güncelle

gaianet config` alt komutunu kullanarak `config.json` dosyasında tanımlanan anahtar alanları güncelleyebilirsiniz. Konfigürasyonu güncelledikten sonra `gaianet init` komutunu tekrar çalıştırmalısınız.

Örneğin `chat` alanını güncellemek için aşağıdaki komutu kullanın:

```bash
gaianet config --chat-url "https://huggingface.co/second-state/Llama-2-13B-Chat-GGUF/resolve/main/Llama-2-13b-chat-hf-Q5_K_M.gguf"
```

Örneğin `chat_ctx_size` alanını güncellemek için aşağıdaki komutu kullanın:

```bash
gaianet config --chat-ctx-size 5120
```

Aşağıda `config` alt komutunun tüm seçenekleri yer almaktadır.

```console
$ gaianet config --help

Kullanım: gaianet config [SEÇENEKLER]

Seçenekler:
--chat-url <url>               Chat model bağlantısını günceler..
  --chat-ctx-size <val>          Chat modelin context(bağlam) boyutunu günceller.
  --embedding-url <url>          Embedding(gömme) modelinin bağlantısını günceller.
  --embedding-ctx-size <val>     Embedding(gömme) modelinin context(bağlam) boyutunu günceller.
  --prompt-template <val>        Chat modelinin Yönerge(prompt) şablonunu günceller.
  --port <val>                   LlamaEdge API sunucusunun portunu günceller.
  --system-prompt <val>          Sistem yönergelerini(prompt) günceller.
  --rag-prompt <val>             Rag yönergelerini günceller.
  --rag-policy <val>             Rag politikasını günceller [olası değerler: system-message, last-user-message].
  --reverse-prompt <val>         Ters yönergeyi(reverse prompt) günceller.
  --domain <val>                 GaiaNet node alanadını(domain) günceller.
  --snapshot <url>               Qdrant snapshot(resimleme) günceller.
  --qdrant-limit <val>           Maksimum geri dönen sonuç adetini günceller.
  --qdrant-score-threshold <val> Sonuçların minimal(en düşük) skor eşiğini günceller.
  --base <path>                  GaiaNet node temel klasör yapısı.
  --help                         Yardım mesajını gösterir.
```

İyi eğlenceler!
