# ML Model TensorRT Export
This repository contains the necessary configuration and scripts to relabel a dataset with a Zero Shot Object Detector (OwlVit v2 in this case), and to segment the detections with the SAM model, obtaining COCO format labels.

## 🔧 Development Environment

This project uses DevContainer to ensure a consistent development environment across all contributors. This means you can develop either:
- Using VS Code with the Remote Containers extension
- Using Dockerfile directly

### Prerequisites

- Docker installed on your system
- VS Code with Remote Containers extension (if using VS Code)
- Git

### 🐳 Container Setup

1. Clone the repository:
```bash
git clone http://gitlab.hertasecurity.com/research/little-codes/yolo.git
cd relabel_zeroshot
```

2. If using VS Code:
   - Open VS Code
   - Press `Ctrl+Shift+P` and select "Remote-Containers: Open Folder in Container"
   - Select the project folder

The DevContainer will automatically:
- Set up the CUDA environment
- Install all required Python packages
- Configure the development environment

## 📁 Project Structure

```
.
├── .devcontainer/
│   ├── devcontainer.json     # DevContainer configuration
│   └── Dockerfile           # Development environment definition
├── scripts/
│   ├── segment_owltrt_samtrt.py       # Relabeler script
│   ├── owlvit_sam_predictor.py       # Relabeler script without the need of TRT models
│   └── test_coco_predictions.py     # Example script for using it
├── data/
│   └── mobile_sam_mask_decoder.onnx            # Model mobile SAM
│   └── resnet18_image_encoder.onnx             # Model resnet18 for SAM
└── README.md
```

## 🔄 Input Format

The script expects some input data in order to label the images:

- Text_prompt: list of classes that we want to detect
```python
["man", "woman", "toddler", "kid", "teenager", "adult", "grandparent",
"shirt", "blouse", "t-shirt", "sweater", "jacket", "vest", "sweatshirt", "hoodie", "blazer", "polo shirt","trousers", "pants", "chinos", "jeans", "shorts", "skirt", "leggings", "dress", "jumpsuit", "coat", "parka", "raincoat", "trench coat", "peacoat", "anorak", "bra", "underwear", "boxers", "briefs", "pajamas", "nightgown", "swimsuit", "bikini", "swim trunks", "suit", "waistcoat", "athletic shorts", "cycling shorts", "shoe", "sneakers", "loafers", "oxford shoes", "boots", "sandals", "flip-flops", "high heels", "flats", "boat shoes", "glasses", "hat", "cap", "tie", "glove", "watch", "belt", "sock", "scarf", "bag", "backpack", "suitcase", "mask", "necklace", "bracelet", "earrings", "handbag", "wallet", "sunglasses", "smartphone", "cell phone", "beanie", "earmuffs", "mittens", "ski goggles", "lab coat", "scrubs", "uniform"]
```

- Thresholds: list of thresholds we want to apply to each class
This mush have the same lenght as text_prompt
```python
[
0.3, 0.3, 0.3, 0.3, 0.3, 0.3, 0.3, # "man", "woman", "toddler", "kid", "teenager", "adult", "grandparent",
0.3, 0.4, 0.5, 0.8, 0.2, 0.5, 0.5, 0.5, # "shirt", "blouse", "t-shirt", "sweater", "jacket", "vest", "sweatshirt", "hoodie",
0.2, 0.5, # "blazer", "polo shirt",
0.3, 0.3, 0.6, 0.4, 0.3, 0.5, 0.3, # "trousers", "pants", "chinos", "jeans", "shorts", "skirt", "leggings",
0.4, 0.8, # "dress", "jumpsuit",
0.2, 0.3, 0.3, 0.2, 0.3, 0.2, # "coat", "parka", "raincoat", "trench coat", "peacoat", "anorak",
0.6, 0.6, 0.6, 0.6, 0.6, 0.6, # "bra", "underwear", "boxers", "briefs", "pajamas", "nightgown",
0.5, 0.3, 0.6, # "swimsuit", "bikini", "swim trunks",
0.4, 0.5, # "suit", "waistcoat",
0.4, 0.4, # "athletic shorts", "cycling shorts",
0.3, 0.35, 0.35, 0.35, 0.35, 0.35, 0.35, # "shoe", "sneakers", "loafers", "oxford shoes", "boots", "sandals", "flip-flops",
0.35, 0.35, 0.35, 0.35, 0.35, # "high heels", "flats", "boat shoes",
0.3, 0.4, 0.4, 0.3, 0.3, 0.3, 0.3, 0.3, 0.4, # "glasses", "hat", "cap", "tie", "glove", "watch", "belt", "sock", "scarf",
0.2, 0.3, 0.2, 0.3, 0.3, 0.3, 0.4, # "bag", "backpack", "suitcase", "mask", "necklace", "bracelet", "earrings",
0.4, 0.3, 0.4, 0.5, 0.5, # "handbag", "wallet", "sunglasses", "smartphone", "cell phone",
0.4, 0.3, 0.2, 0.5, # "beanie", "earmuffs", "mittens", "ski goggles",
0.3, 0.2, 0.4 # "lab coat", "scrubs", "uniform"
]
```

- Translations: it is an optional list of the translated labels, it works only for visualization functionalities
- categories: dictionary that maps the text_prompt labels to the label that we would want in the final annotation
```python
{
    "person": ["man", "woman", "toddler", "kid", "teenager", "adult", "grandparent"], 
    "shirt": ["shirt", "blouse", "t-shirt", "polo shirt"],
    "jersey": ["sweater", "sweatshirt", "hoodie", "vest"],
    "coat": ["jacket", "blazer", "coat", "parka", "raincoat", "trench coat", "peacoat", "anorak"],
    "trousers": ["trousers", "pants", "chinos"],
    "jeans": ["jeans"],
    "shorts": ["shorts", "athletic shorts", "cycling shorts"],
    "skirt": ["skirt"],
    "leggings": ["leggings"],
    "full_body": ["dress", "jumpsuit", "suit"],
    "underwear": ["bra", "underwear", "boxers", "briefs", "pajamas", "nightgown"],
    "swimwear": ["swimsuit", "bikini", "swim trunks"],
    "shoes": ["sneakers", "sandals", "flip-flops", "boat shoes"],
    "formal": ["loafers", "oxford shoes"],
    "heels": ["high heels", "flats"],
    "boots": ["boots"],
    "hat": [ "hat", "cap", "beanie", "earmuffs"],
    "glasses": ["glasses"],
    "sunglasses": ["ski goggles", "sunglasses"],
    "hand": ["glove", "watch", "bracelet", "mittens"],
    "jewelry": ["necklace", "earrings"],
    "scarf": ["scarf"],
    "tie": ["tie"],
    "belt": ["belt"],
    "mask": ["mask"],
    "bag": ["bag", "backpack", "suitcase", "handbag"],
    "wallet": ["wallet"],
    "phone": ["smartphone", "cell phone"],
    "uniform": ["lab coat", "scrubs", "uniform"]
}
```
## 📤 Exporting to TensorRT

This repo is based in [NanoSAM](https://github.com/NVIDIA-AI-IOT/nanosam)and [NanoOwl](https://github.com/NVIDIA-AI-IOT/nanoowl) implementations from NVIDIA.

To export your OwlVit model to TensorRT format:

1. Place at the root of the workspace.
2. Run the export to trt script:
   ```bash
mkdir -p data
python3 -m nanoowl.build_image_encoder_engine \
    data/owl_image_encoder_patch32.engine
```

To export the SAM models to TensorRT format:
1. Run the export to onnx script:
```bash
python3 -m nanosam.tools.export_sam_mask_decoder_onnx \
    --model-type=vit_t \
    --checkpoint=assets/mobile_sam.pt \
    --output=data/mobile_sam_mask_decoder.onnx
```

3. Run the export to trt:
```bash
trtexec \
    --onnx=data/mobile_sam_mask_decoder.onnx \
    --saveEngine=data/mobile_sam_mask_decoder.engine \
    --minShapes=point_coords:1x1x2,point_labels:1x1 \
    --optShapes=point_coords:1x1x2,point_labels:1x1 \
    --maxShapes=point_coords:1x10x2,point_labels:1x10
```

## 🚀 Docker Production Deployment

Build the production container:

```bash
docker build -t nanoowl:23-01 -f Dockerfile
```