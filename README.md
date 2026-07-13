<p align="center">
  
  <a href="https://docs.cloud.google.com/vertex-ai/generative-ai/docs/models/gemini/2-5-flash">
    <img src="https://img.shields.io/badge/Model-Gemini--2.5--Flash-2EAF37" alt="Gemini 2.5 Flash">
  </a>
  <a href="https://docs.cloud.google.com/vertex-ai/generative-ai/docs/models/gemini/3-1-pro">
    <img src="https://img.shields.io/badge/Model-Gemini--3.1--Pro-2EAF37" alt="Gemini 3.1 Pro">
  </a>
  <a href="https://huggingface.co/meta-llama/Llama-3.2-11B">
    <img src="https://img.shields.io/badge/Model-LLaMA--3.2--11B-2EAF37" alt="LLaMA 3.2 11B">
  </a>
  <a href="https://huggingface.co/Qwen/Qwen2-7B-Instruct">
    <img src="https://img.shields.io/badge/Model-Qwen2--7B-2EAF37" alt="Qwen2 7B">
  </a>
  <a href="https://huggingface.co/llava-hf/llava-1.5-7b-hf">
    <img src="https://img.shields.io/badge/Model-Llava1.5--7B-2EAF37" alt="Llava1.5 7B">
  </a>
  <a href="https://huggingface.co/llava-hf/llava-1.5-13b-hf">
    <img src="https://img.shields.io/badge/Model-Llava1.5--13B-2EAF37" alt="Llava1.5 13B">
  </a>

  <!-- Dataset Badge -->
  <br><br>
  <a href="https://huggingface.co/datasets/Aikyam-Lab/Synoptic-Bench">
    <img src="https://img.shields.io/badge/%F0%9F%A4%97%20Datasets-SynopticBench-E9C300?labelColor=444444" alt="SynopticBench Dataset">
  </a>
<a href="https://arxiv.org/abs/2604.16451">
  <img src="https://img.shields.io/badge/📄%20arXiv-2604.16451-B31B1B?labelColor=444444" alt="arXiv Paper">
</a>

</p>

<img src="./Images/VLM_Fig1.png" width="500px"></img>

## Abstract

Recent advances in visual-language models (VLMs) have led to significant improvements in a plethora of complex multimodal tasks like image captioning, report generation, and visual perception. However, generating text from meteorological data is highly challenging because the atmosphere is a chaotic system that is rapidly changing at various spatial and temporal scales. Given the complexity of atmospheric phenomena, it is critical to verifiably quantify the effectiveness of existing VLMs on weather forecasting data. In this work, we present Synoptic-Bench, a high-quality dataset consisting of 1,367,041 text samples of Advanced Forecast Discussions created by the National Weather Service over the continental United States paired to images of 500mb geopotential height, 2 meter temperature, and 850mb wind velocity in weather forecasts. We also present Synoptic Phenomena Alignment and Coverage Evaluation (SPACE), a novel evaluation framework that can be used to effectively estimate the quality of text descriptions of synoptic weather phenomena. Extensive experiments on generating forecast discussions using state-of-the-art VLMs show the sensitivity of existing evaluation metrics in this domain and enable further exploration into synoptic weather and climate text generation.

## Key Contributions

1. **Comprehensive Multimodal Weather Discussion Dataset:** We introduce the largest multimodal atmospheric dataset to date by pairing National Weather Service Area Forecast Discussions (AFDs) to Global Forecast System (GFS) images. The dataset contains 1,367,041 text samples.
2. **Synoptic Phenomena Evaluation Framework:** We created an evaluation framework: Synoptic Phase Alignment and Coverage Evaluation (SPACE) to evaluate the ability of generated text to describe the correct polarity and location of synoptic weather phenomena
3. **Model Benchmarking:** We finetune four open-source Vision Language models to generate forecast discussions and evaluate them along with base model versions and four different baselines: Gemini-3.1-Pro, Nearest Neighbor, Climatology, and Blind LLM

## SynopticBench vs Other Benchmarks

<img src="./Images/Table1.png" width="500px"></img>

1. SynopticBench is the first multimodal benchmark to use images of atmospheric conditions predicted into the future.
2. There are a total of 1,367,041 text samples, which exceeds the number of samples in all other studies.
3. Many other benchmarks focus on extreme events while SynopticBench focuses on day-to-day weather conditions.

## Installation

### Prerequisites
- Python 3.8 or higher
- CUDA-capable GPU (recommended for model inference)
- Git

### Setup

1. Clone the repository:
```bash
git clone https://github.com/timbhiggins/Synoptic-Bench
cd Synoptic-Bench
```

2. Create a virtual environment (recommended):
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. Install dependencies:
```bash
pip install -r requirements.txt
```

**Note:** For CUDA support with PyTorch, you may need to install PyTorch separately based on your CUDA version. Visit [PyTorch's official website](https://pytorch.org/get-started/locally/) for installation instructions.


## Contents
- [Data and Weights](#Pretrained_models)
- [Train](#train)
- [SPACE Evaluation](#SPACE)
- [Traditional Evaluation](#Traditional_Eval)
- [Preprocessing Setup](#preprocessing)



## Data and Model Weights

The full dataset including saved model weights can be found at https://huggingface.co/datasets/Aikyam-Lab/Synoptic-Bench.

## Train

We finetune LLaVA-v1.5-7B, LLaVA-v1.5-13B, Qwen2.5-VL-7B, and LLaMA-3.2-11B with 1 NVIDIA H200 GPU. The code and training parameters to train each model is in the "train" folder.

## Evaluation

We use Synoptic Phenomena Alignment and Coverage Evaluation (SPACE) for evaluation. The code to run SPACE is in the "SPACE" folder.

## Preprocessing Setup

Step 1: Download one or more of the .hdf5 files from https://huggingface.co/datasets/Aikyam-Lab/Synoptic-Bench

Step 2: Run the prepare_dataset_in_parallel_synoptic.py script. It is recommended that this is run in parallel due to the size of the dataset. The climatology_means.h5 file can be used as the climatology file. This will create a .json file with text and the paths to a folder containing .png images.

Step 3: Run Add_locations.py to add the specific locations into the text prompts.

Step 4: Run qc_filter.py, qc_temporal.py, and quality_control.py on the .json file to ensure that the dataset fits the desired purpose.

