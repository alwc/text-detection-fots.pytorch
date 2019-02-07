# [FOTS](https://arxiv.org/abs/1801.01671) based text detection ([PyTorch](https://pytorch.org/))
## Running:
Replace datasets.ICDAR2015 folder in `train.py`. It is expected that the provided folder contains unzipped `ch4_training_images` and `ch4_training_localization_transcription_gt` from [Task 4.4: End to End (2015 edition)](http://rrc.cvc.uab.es/?ch=4&com=downloads). Run `train.py`.
## DataSets
* Synth800k (train only)
* ICDAR 2015
* ICDAR 2013
* ICDAR 2017 MLT
> We use model trained on ImageNet dataset as our pre-trained model. The training process includes two steps: first we use Synth800k dataset to train the network for 10 epochs, and then real data is adopted to fine-tune the model until convergence. ... Some blurred text regions in ICDAR 2015 and ICDAR 2017 MLT datasets are labeled as “DO NOT CARE”, and we ignore them in training.
### Augmentation
> First, longer sides of images are resized from 640 pixels to 2560 pixels. Next, images are rotated in range [−10◦ , 10◦ ] randomly. Then, the heights of images are rescaled with ratio from 0.8 to 1.2 while their widths keep unchanged. Finally, 640×640 random samples are cropped from the transformed images.
## Training
Employ shrunk version of the original text regions: 0.3 from each corner according to [EAST](https://arxiv.org/abs/1704.03155v2) while the area between the bounding box and the shrunk version is considered as “NOT CARE”.
### OHEM
> For each image, 512 hard negative samples, 512 random negative samples and all positive samples are selected for classification. As a result, positive-to-negative ratio is increased from 1:60 to 1:3. And for bounding box regression, we select 128 hard positive samples and 128 random positive samples from each image for training gives 2%.

For loss info see fomulas 1, 2 and 3 in [the paper](https://arxiv.org/abs/1801.01671).
## Test
> Final detection results are produced by applying thresholding and NMS to these positive samples.

Or locality aware NMS as in [EAST](https://arxiv.org/abs/1704.03155v2)?
### Expected results (`Our Detection` line from the paper results) F-measure
* ICDAR 2015:  85.31%
* ICDAR 2017 MLT: 66.69%
* ICDAR 2013: 87.32%
