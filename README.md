## Overview

This is the PyTorch implementation of paper [Data Augmentation of Bridging the Delay Gap for DL-based Massive MIMO CSI Feedback]().

## Requirements

To use this project, you need to ensure the following requirements are installed.

- Python >= 3.7
- [PyTorch >= 1.2](https://pytorch.org/get-started/locally/)
- [thop](https://github.com/Lyken17/pytorch-OpCounter)

## Project Preparation

#### A. Data Preparation

The channel state information (CSI) matrix is generated from [COST2100](https://ieeexplore.ieee.org/document/6393523) model. 
You can download it from [Baidu Netdisk](https://pan.baidu.com/s/1MCNrmmGShwHuttPMxcr_YA?pwd=9b0l). Datasets inculde mode-change and range-change datasets, whose meanings and details can be found in our paper.
You can generate your own dataset according to the [open source library of COST2100](https://github.com/cost2100/cost2100) as well. The details of data pre-processing can be also found in our paper.

#### B. Checkpoints Downloading

The model checkpoints should be downloaded if you would like to reproduce our result. All the checkpoints files can be downloaded from [Baidu Netdisk](https://pan.baidu.com/s/1rtAA-vXOHUCf3wXsfoR-4g?pwd=st0i)

#### C. Project Tree Arrangement

We recommend you to arrange the project tree as follows.

```
home
├── CRNet_Aug  # The cloned CRNet repository
│   ├── dataset
│   ├── models
│   ├── utils
│   ├── main.py
├── dataset_COST2100  # The data folder
│   ├── range_change_train_in.mat
│   ├── ...
├── Experiments
│   ├── checkpoints  # The checkpoints folder
│   │     ├── range_in_4.pth
│   │     ├── ...
│   ├── run.sh  # The bash script
...
```

## Train CRNet_Aug from Scratch

An example of run.sh is listed below. Simply use it with `sh run.sh`. It will start advanced scheme aided CRNet training from scratch. Change scenario using `--scenario` and change compression ratio with `--cr`.

``` bash
python /home/CRNet_Aug/main.py \
  --data-dir '/home/dataset_COST2100' \
  --scenario 'in' \
  --epochs 2500 \
  --batch-size 200 \
  --workers 0 \
  --cr 4 \
  --scheduler cosine \
  --gpu 0 \
  2>&1 | tee log.out
```

## Results and Reproduction

The main results reported in our paper are presented as follows. All the listed results can be found in Table1 and Table2 of our paper. They are achieved from training CRNet with our proposed B-S and R-G data augmentation approaches. 


Datasets | Scenario | Compression Ratio | Augmentation | NMSE | Checkpoints
:--: | :--: | :--: | :--: | :--: | :--:
mode-change | indoor | 1/4 | B-S | -28.22 | mode_in_4.pth
mode-change | indoor | 1/8 | B-S | -17.73 | mode_in_8.pth
mode-change | indoor | 1/16 | B-S | -13.02 | mode_in_16.pth
mode-change | indoor | 1/32 | B-S | -10.55 | mode_in_32.pth
mode-change | indoor | 1/64 | B-S | -6.65 | mode_in_64.pth
mode-change | outdoor | 1/4 | B-S&R-G | -9.76 | mode_out_4.pth
mode-change | outdoor | 1/8 | B-S&R-G | -7.32 | mode_out_8.pth
mode-change | outdoor | 1/16 | B-S&R-G | -5.56 | mode_out_16.pth
mode-change | outdoor | 1/32 | B-S&R-G | -3.79 | mode_out_32.pth
mode-change | outdoor | 1/64 | B-S&R-G | -1.45 | mode_out_64.pth
range-change | indoor | 1/4 | B-S | -26.97 | range_in_4.pth
range-change | indoor | 1/8 | B-S | -16.10 | range_in_8.pth
range-change | indoor | 1/16 | B-S | -9.52 | range_in_16.pth
range-change | indoor | 1/32 | B-S | -8.37 | range_in_32.pth
range-change | indoor | 1/64 | B-S | -5.03 | range_in_64.pth
range-change | outdoor | 1/4 | B-S | -7.29 | range_out_4.pth
range-change | outdoor | 1/8 | B-S | -4.92 | range_out_8.pth
range-change | outdoor | 1/16 | B-S | -2.01 | range_out_16.pth
range-change | outdoor | 1/32 | B-S | -1.20 | range_out_32.pth
range-change | outdoor | 1/64 | B-S | -0.69 | range_out_64.pth

As aforementioned, we provide model checkpoints for the best results in Table1 and Table2. Our code library supports easy inference. 

**To reproduce all these results, simple add `--evaluate` to `run.sh` and pick the corresponding pre-trained model with `--pretrained`.** An example is shown as follows.

``` bash
python /home/CRNet_Aug/main.py \
  --data-dir '/home/dataset_COST2100' \
  --scenario 'in' \
  --pretrained './checkpoints/mode_in_4.pth' \
  --evaluate \
  --batch-size 200 \
  --workers 0 \
  --cr 4 \
  --cpu \
  2>&1 | tee log.out
```

## Acknowledgment

Thank Chao-Kai Wen and Shi Jin group again for providing the pre-processed COST2100 dataset, you can find their related work named CsiNet in [Github-Python_CsiNet](https://github.com/sydney222/Python_CsiNet) 
