<div align="center">

<h1>Retrieval-based-Voice-Conversion-WebUI</h1>
Uma estrutura de alteração de voz (voice conversion) simples e fácil de usar baseada em VITS.<br><br>

[![madewithlove](https://img.shields.io/badge/made_with-%E2%9D%A4-red?style=for-the-badge&labelColor=orange
)](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI)

<img src="https://counter.seku.su/cmoe?name=rvc&theme=r34" /><br>

[![Open In Colab](https://img.shields.io/badge/Colab-F9AB00?style=for-the-badge&logo=googlecolab&color=525252)](https://colab.research.google.com/github/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/blob/main/Retrieval_based_Voice_Conversion_WebUI.ipynb)
[![Licence](https://img.shields.io/badge/LICENSE-MIT-green.svg?style=for-the-badge)](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/blob/main/LICENSE)
[![Huggingface](https://img.shields.io/badge/🤗%20-Spaces-yellow.svg?style=for-the-badge)](https://huggingface.co/lj1995/VoiceConversionWebUI/tree/main/)

[![Discord](https://img.shields.io/badge/RVC%20Developers-Discord-7289DA?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/HcsmBBGyVk)

[**Changelog**](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/blob/main/docs/Changelog_CN.md) | [**FAQ (Perguntas Frequentes)**](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/wiki/%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98%E8%A7%A3%E7%AD%94) | [**AutoDL·Treine um cantor AI por 5 centavos**](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/wiki/Autodl%E8%AE%AD%E7%BB%83RVC%C2%B7AI%E6%AD%8C%E6%89%8B%E6%95%99%E7%A8%8B) | [**Registro de experimentos comparativos**](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/wiki/%E5%AF%B9%E7%85%A7%E5%AE%9E%E9%AA%8C%C2%B7%E5%AE%9E%E9%AA%8C%E8%AE%B0%E5%BD%95) | [**Demo online**](https://modelscope.cn/studios/FlowerCry/RVCv2demo)

[**English**](./docs/en/README.en.md) | [**中文简体**](./README.md) | [**日本語**](./docs/jp/README.ja.md) | [**한국어**](./docs/kr/README.ko.md) ([**韓國語**](./docs/kr/README.ko.han.md)) | [**Français**](./docs/fr/README.fr.md) | [**Türkçe**](./docs/tr/README.tr.md) | [**Português**](./docs/pt/README.pt.md)

</div>

> O modelo base foi treinado com aproximadamente 50 horas do conjunto de dados VCTK de alta qualidade e código aberto; não há preocupações quanto a direitos autorais — use com confiança.

> Aguarde o RVCv3: modelo base maior, mais dados, melhor qualidade; velocidade de inferência similar e exigência menor de dados para treinar.

<table>
   <tr>
		<td align="center">Interface de treino / inferência</td>
		<td align="center">Interface de alteração de voz em tempo real</td>
	</tr>
  <tr>
		<td align="center"><img src="https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/assets/129054828/092e5c12-0d49-4168-a590-0b0ef6a4f630"></td>
    <td align="center"><img src="https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/assets/129054828/730b4114-8805-44a1-ab1a-04668f3c30a6"></td>
	</tr>
	<tr>
		<td align="center">go-web.bat</td>
		<td align="center">go-realtime-gui.bat</td>
	</tr>
  <tr>
    <td align="center">Escolha livremente a operação que deseja executar.</td>
		<td align="center">Já alcançamos latência ponta a ponta de 170ms. Utilizando dispositivos ASIO de entrada/saída é possível atingir ~90ms, porém isso depende fortemente do suporte do driver de hardware.</td>
	</tr>
</table>

## Introdução
Este repositório possui as seguintes características:
+ Substitui características da fonte de entrada por características do conjunto de treinamento usando busca top-1 para prevenir vazamento de timbre.
+ Treina rapidamente mesmo em GPUs relativamente modestas.
+ Bons resultados com poucos dados (recomenda-se coletar ao menos 10 minutos de áudio limpo e com baixo ruído).
+ É possível alterar timbres por fusão de modelos (use a aba de opções ckpt e o recurso `ckpt-merge`).
+ Interface web simples e fácil de usar.
+ Pode invocar o modelo UVR5 para separar rapidamente voz e acompanhamento.
+ Utiliza o algoritmo de extração de pitch para voz humana InterSpeech2023-RMVPE — resolve problemas de pitch ausente. É o melhor em qualidade (significativamente), porém é mais rápido e consome menos recursos que o `crepe_full`.
+ Suporte a aceleração em GPUs AMD (A cards) e Intel (I cards).

Assista ao nosso [vídeo de demonstração](https://www.bilibili.com/video/BV1pm4y1z7Gm/)!

## Configuração do ambiente
Os comandos abaixo devem ser executados em um ambiente com Python >= 3.8.

### Métodos gerais (Windows / Linux / MacOS)
Escolha um dos métodos a seguir.

#### 1. Instalar dependências via pip
1. Instale o Pytorch e dependências centrais (se já estiverem instalados, pule esta etapa). Referência: https://pytorch.org/get-started/locally/
```bash
pip install torch torchvision torchaudio
```
2. Em sistemas Windows com GPUs Nvidia Ampere (RTX30xx), de acordo com a issue #21, pode ser necessário especificar a versão do CUDA compatível com o PyTorch:
```bash
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu117
```
3. Instale dependências conforme sua placa:
- Nvidia (N-card)
```bash
pip install -r requirements.txt
```
- AMD / Intel (A-card / I-card)
```bash
pip install -r requirements-dml.txt
```
- AMD ROCm (Linux)
```bash
pip install -r requirements-amd.txt
```
- Intel IPEX (Linux)
```bash
pip install -r requirements-ipex.txt
```

#### 2. Instalar dependências com Poetry
Instale o gerenciador Poetry (se já tiver, ignore). Referência: https://python-poetry.org/docs/#installation
```bash
curl -sSL https://install.python-poetry.org | python3 -
```

Ao usar Poetry, recomenda-se Python 3.7–3.10, pois versões fora desse intervalo podem causar conflito na instalação do `llvmlite==0.39.0`.
```bash
poetry init -n
poetry env use "caminho/para/seu/python.exe"
poetry run pip install -r requirments.txt
```

### MacOS
Você pode usar o script `run.sh` para instalar dependências:
```bash
sh ./run.sh
```

## Preparação de outros pré-modelos
RVC necessita de alguns pré-modelos adicionais para inferência e treinamento.

Você pode baixar esses modelos no nosso [Hugging Face space](https://huggingface.co/lj1995/VoiceConversionWebUI/tree/main/).

### 1. Baixar assets
Abaixo está a lista de nomes de todos os pré-modelos e outros arquivos requeridos pelo RVC. Há scripts na pasta `tools` para baixar esses arquivos automaticamente.

- ./assets/hubert/hubert_base.pt

- ./assets/pretrained 

- ./assets/uvr5_weights

Se quiser usar modelos da versão v2, será necessário baixar também:

- ./assets/pretrained_v2

### 2. Instalar ffmpeg
Se `ffmpeg` e `ffprobe` já estiverem instalados, pule esta etapa.

#### Usuários Ubuntu/Debian
```bash
sudo apt install ffmpeg
```
#### Usuários MacOS
```bash
brew install ffmpeg
```
#### Usuários Windows
Baixe os executáveis e coloque-os no diretório raiz do projeto.
- Baixe [ffmpeg.exe](https://huggingface.co/lj1995/VoiceConversionWebUI/blob/main/ffmpeg.exe)

- Baixe [ffprobe.exe](https://huggingface.co/lj1995/VoiceConversionWebUI/blob/main/ffprobe.exe)

### 3. Baixar arquivos necessários para o algoritmo de extração de pitch RMVPE
Se deseja usar o algoritmo RMVPE (Melhore extração de pitch), baixe o parâmetro do modelo e coloque na raiz do RVC.

- Baixe [rmvpe.pt](https://huggingface.co/lj1995/VoiceConversionWebUI/blob/main/rmvpe.pt)

#### (Opcional) Baixar a versão ONNX do rmvpe para ambientes DML (usuários A-card / I-card)
- Baixe [rmvpe.onnx](https://huggingface.co/lj1995/Voice-Conversion-WebUI/blob/main/rmvpe.onnx)

### 4. AMD ROCm (opcional, somente Linux)
Se deseja rodar RVC com ROCm em GPUs AMD no Linux, instale os drivers conforme a documentação oficial: https://rocm.docs.amd.com/en/latest/deploy/linux/os-native/install.html

Se estiver em Arch Linux, é possível instalar via pacman:
```bash
pacman -S rocm-hip-sdk rocm-opencl-sdk
```

Para algumas GPUs (ex.: RX6700XT) pode ser necessário configurar variáveis de ambiente:
```bash
export ROCM_PATH=/opt/rocm
export HSA_OVERRIDE_GFX_VERSION=10.3.0
```
Também garanta que seu usuário pertença aos grupos `render` e `video`:
```bash
sudo usermod -aG render $USERNAME
sudo usermod -aG video $USERNAME
```

## Começando
### Inicialização direta
Use o comando abaixo para iniciar a WebUI:
```bash
python infer-web.py
```

Se você instalou dependências via Poetry, inicie com:
```bash
poetry run python infer-web.py
```

### Usando o pacote integrado (bundle)
Baixe e extraia `RVC-beta.7z`

#### Usuários Windows
Dê um duplo clique em `go-web.bat`

#### Usuários MacOS
```bash
sh ./run.sh
```

### Para usuários I-card que precisam usar IPEX (somente Linux)
```bash
source /opt/intel/oneapi/setvars.sh
```

## Projetos de referência
+ [ContentVec](https://github.com/auspicious3000/contentvec/)
+ [VITS](https://github.com/jaywalnut310/vits)
+ [HIFIGAN](https://github.com/jik876/hifi-gan)
+ [Gradio](https://github.com/gradio-app/gradio)
+ [FFmpeg](https://github.com/FFmpeg/FFmpeg)
+ [Ultimate Vocal Remover](https://github.com/Anjok07/ultimatevocalremovergui)
+ [audio-slicer](https://github.com/openvpi/audio-slicer)
+ [Extração de pitch: RMVPE](https://github.com/Dream-High/RMVPE)
  + O modelo pré-treinado foi treinado e testado por [yxlllc](https://github.com/yxlllc/RMVPE) e [RVC-Boss](https://github.com/RVC-Boss).

## Agradecimentos a todos os contribuintes
<a href="https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/graphs/contributors" target="_blank">
  <img src="https://contrib.rocks/image?repo=RVC-Project/Retrieval-based-Voice-Conversion-WebUI" />
</a>
