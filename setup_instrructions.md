# Setup Instructions - HD Diagnosis Project

**Quick setup guide for Automated Huntington's Disease Diagnosis**

---

## Prerequisites

* **OS**: Ubuntu 24.04 LTS (or similar Linux)
* **GPU**: NVIDIA with 4GB+ VRAM (GTX 1650 Ti or better)
* **RAM**: 16GB minimum
* **Storage**: 200GB free space
* **Python**: 3.10

---

## Quick Start (5 Steps)

### Step 1: Clone Repository

```bash
git clone https://github.com/SayedZabiulla/HD-MRI-Diagnosis.git
cd HD-MRI-Diagnosis
```

### Step 2: Setup Environment

```bash
# Create virtual environment
python3.10 -m venv hd_env
source hd_env/bin/activate

# Upgrade pip
pip install --upgrade pip
```

### Step 3: Install Dependencies

```bash
# Install PyTorch with CUDA
pip install torch==2.2.2 torchvision==0.17.2 --index-url https://download.pytorch.org/whl/cu121

# Install other packages
pip install monai==1.3.1 nibabel==5.2.0 numpy==1.24.3 pandas==2.0.3 scikit-learn==1.3.0 matplotlib==3.7.2 seaborn==0.12.2 opencv-python==4.8.0.76 pillow==10.0.0 grad-cam==1.4.8 tqdm==4.66.1
```

**OR**

```bash
pip install -r requirements.txt
```

### Step 4: Verify Installation

```bash
python -c "import torch; print(f'PyTorch: {torch.__version__}\nCUDA: {torch.cuda.is_available()}\nGPU: {torch.cuda.get_device_name(0) if torch.cuda.is_available() else \"N/A\"}')"
```

Expected Output:

```
PyTorch: 2.2.2
CUDA: True
GPU: NVIDIA GeForce GTX 1650 Ti
```

### Step 5: Download Dataset

1. Visit https://www.oasis-brains.org/
2. Register and download OASIS-1 Cross-Sectional dataset
3. Download all 12 discs (~150GB total)
4. Extract to `data/raw/`

Verify dataset:

```bash
ls data/raw/
```

Should show:

```
oasis_cross-sectional.csv
disc1
disc2
...
disc12
```

---

## Usage

### Preprocessing (One-time)

```bash
python scripts/create_labels.py
python scripts/extract_slices.py
python scripts/create_dataset.py
```

### Training

```bash
# Train ResNet50
python scripts/train.py

# Train DenseNet121
python scripts/train_densenet.py

# Train EfficientNet-B0
python scripts/train_efficientnet.py
```

### Evaluation

```bash
python scripts/evaluate.py
python scripts/evaluate_densenet.py
python scripts/evaluate_efficientnet.py
python scripts/compare_all_three_models.py
```

### Visualization

```bash
python scripts/visualize_gradcam.py
```

---

## Troubleshooting

### CUDA Not Available

```bash
nvidia-smi
pip uninstall torch torchvision
pip install torch==2.2.2 torchvision==0.17.2 --index-url https://download.pytorch.org/whl/cu121
```

### Out of Memory

Edit training scripts:

```python
batch_size = 8
```

### Dataset Not Found

```bash
ls -la data/raw/
```

---

## Directory Structure

```
HD-MRI-Diagnosis/
├── data/
│   ├── raw/
│   └── processed/
├── models/
├── results/
├── scripts/
└── requirements.txt
```

---

## Performance Benchmarks

Hardware: GTX 1650 Ti (4GB VRAM), 16GB RAM

| Task                 | Time     |
| -------------------- | -------- |
| Preprocessing        | ~15 min  |
| Training (per model) | ~2-3 hrs |
| Evaluation           | ~5 min   |

---

## Key Results

| Model           | Accuracy   | Recall   | F1 Score   | Parameters |
| --------------- | ---------- | -------- | ---------- | ---------- |
| ResNet50        | 86.39%     | 64.13%   | 70.09%     | 24.5M      |
| **DenseNet121** | **86.83%** | **100%** | **77.53%** | 7.5M       |
| EfficientNet-B0 | TBD        | TBD      | TBD        | 5.3M       |

Winner: **DenseNet121**

---

## Support

GitHub: https://github.com/SayedZabiulla/HD-MRI-Diagnosis
Issues: https://github.com/SayedZabiulla/HD-MRI-Diagnosis/issues
Email: sayedzabeulla@gmail.com(mailto:sayedzabeulla@gamil.com)

---

## Quick Reference

```bash
source hd_env/bin/activate

python scripts/extract_slices.py
python scripts/create_dataset.py

python scripts/train_densenet.py

python scripts/evaluate_densenet.py
python scripts/compare_all_three_models.py
```

---

Last Updated: January 2025
Version: 1.0
