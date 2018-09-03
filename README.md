# Interactive Deep Colorization in PyTorch

This is our PyTorch reimplementation for interactive image colorization. The code was written by [Richard Zhang](https://github.com/richzhang) and [Jun-Yan Zhu](https://github.com/junyanz). The original GitHub repo (in Caffe) is [here](https://richzhang.github.io/ideepcolor/).

## Prerequisites
- Linux or macOS
- Python 2 or 3
- CPU or NVIDIA GPU + CUDA CuDNN

## Getting Started
### Installation
- Install PyTorch 0.4+ and torchvision from http://pytorch.org and other dependencies (e.g., [visdom](https://github.com/facebookresearch/visdom) and [dominate](https://github.com/Knio/dominate)). You can install all the dependencies by
```bash
pip install -r requirements.txt
```
- Clone this repo:
```bash
git clone https://github.com/richzhang/colorization-pytorch
cd colorization-pytorch
```

### Dataset preparation
- Download the ILSVRC 2012 dataset and run the following script to prepare data
```python make_ilsvrc_dataset.py --in_path /PATH/TO/ILSVRC12```. This will make symlinks into the training set, and divide the ILSVRC validation set into validation and test splits for colorization.

### Training interactive colorization
- Train a model: ```bash ./scripts/train_siggraph.sh```. This is a 2 stage training process. First, the network is trained for automatic colorization using classification loss for 15 epochs. Results are in `./checkpoints/siggraph_class`. Then, the network is fine-tuned for interactive colorization using regression loss for 10 epochs. Final results are in `./checkpoints/siggraph_reg`.

- To view training results and loss plots, run `python -m visdom.server` and click the URL http://localhost:8097.`

### Testing interactive colorization
- Test the model on validation data:
```bash
python test.py --name siggraph_pretrained --phase val --load_model
```
The test results will be saved to a html file here: `./results/siggraph_reg/latest_val/index.html`. This will test (1) automatic colorization, (2) colorization with some random hints, and (3) colorization wit lots of random hints.

- Test the model by making PSNR vs number of hints plot:
```bash
python test_sweep.py --name siggraph_reg 
```

- Test the model with GUI.

Original [GitHub repo](https://github.com/junyanz/interactive-deep-colorization). Checkout the `pytorch` branch.

## Future

I hope to reimplement [Colorful Image Colorization, ECCV 2016.](https://github.com/richzhang/colorization) and [Split-Brain Autoencoders, CVPR 2017.](https://github.com/richzhang/splitbrainauto) using this codebase as well.

## Acknowledgments
This code borrows from the [pytorch-CycleGAN](https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix) repository.
