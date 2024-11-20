# Tower Insulator Conductors (TIC) Powerline Dataset with Object Detection Models
[This is the repo link to copy](https://github.com/Elizbellou/Powerline-TIC-Dataset-and-Detection-Models)
1. The TIC Dataset consists of 2056 images (512x640) of transmission line network footage in Greece (Northeast Attica) and annotations of three object classes, i.e. towers, insulators and conductors. Link to download the dataset, [here](https://drive.google.com/drive/folders/1iD9DfRdULxudy4jtru1bteRz3th3x38v?usp=sharing)

The folder contains three sub-folders, organized as follows: 

i. train dataset folder ("train"):

-"Images" file with 1483 images and a JSON file with corresponding annotations (polygons).

-"Labels" file with txt annotation files (YOLO format) for each train image. 

ii. validation dataset folder ("valid"):

-"Images" file with 366 images and a JSON file with corresponding annotations (polygons).

-"Labels" file with txt annotation files (YOLO format) for each train image.

iii. Test dataset folder ("test")":

-"Images" file with 207 images and a JSON file with corresponding annotations (polygons).

-"Labels" file with txt annotation files (YOLO format) for each train image.

2. [TICmodels_weights](https://drive.google.com/drive/folders/1iD9DfRdULxudy4jtru1bteRz3th3x38v?usp=sharing) folder consists of four files (.pt) which correspond to the pre-trained weights of each TIC-model trained on the TIC-Dataset, using YOLOv8x ([tic_xlarge.pt](https://drive.google.com/file/d/1141g8IsKIhLKzMyXYUbpjTjZDzTgUJ5R/view?usp=sharing)), YOLOv8l ([tic_large.pt](https://drive.google.com/file/d/10L-Z663rLdyA4DzDgn0rLTUme2gL1Es5/view?usp=sharing)), YOLOv8m ([tic_medium.pt](https://drive.google.com/file/d/10OjMQYiE2wV8NIQCJ0ivA4vEH0MvPdU2/view?usp=sharing)) and YOLOv8n ([tic_nano.pt](https://drive.google.com/file/d/11gQvu9kSdYeXzq8hh_P-LgO0pGs5Cf3O/view?usp=sharing)). 
   
3. [Final_TICmodel](https://drive.google.com/drive/folders/1k6ZbP7PzigV1DkXF3g3fpQwcTmT8OsBu?usp=sharing) folder provides the pre-trained weights file (.pt) of the TIC-model trained on TIC-Dataset, YOLOv8s ([tic_small.pt](https://drive.google.com/file/d/109SxkukPRckQqxaoXifwS-LMsqiiWCF1/view?usp=sharing)), as well as files related to the Results of this model, such as training and validation loss graphs, PR and F1-score curves, confusion matrix and predictions on validation images (JPEG).
 
# Model's evaluation

Our models’ performance with Tesla T4 GPU (15GB), reaching 97% mAP@0.5 for towers detection with YOLOv8x TIC-model, are displayed in the table below. Input image size 640X640 and NMS time per image ≈ 1.5 - 2ms (not included): 
| PL-models | Precision | Recall | mAP@0.5 | mAP@5:95 | fps | Size |
|-----------|-----------|--------|---------|----------|-----|------|
| Yolov8n   | 0.793     | 0.781  | 0.822   | 0.606    | 277 | 5.9MB|
| Yolov8s   | 0.813     | 0.784  | 0.838   | 0.646    | 138 |21.5MB|
| Yolov8m   | 0.833     | 0.78   | 0.84    | 0.678    | 61  |49.6MB|
| Yolov8l   | 0.841     | 0.787  | 0.852   | 0.694    | 35.7|83.6MB|
| Yolov8x   | 0.837     | 0.799  | 0.856   | 0.699    | 21.5|130MB |
|-----------|-----------|--------|---------|----------|-----|------|
| Yolov8x   | Precision | Recall | mAP@0.5 | mAP@5:95 |-----|------|
| Tower     | 0.956     | 0.891  | 0.97    | 0.916    |-----|------|
| Insulator | 0.836     | 0.865  | 0.91    | 0.696    |-----|------|
|Conductors | 0.718     | 0.641  | 0.689   | 0.486    |-----|------|

For detailed insights please read and cite :

# Preparation of the data
To train and evaluate your YOLOv8 models on TIC-Dataset a data.yaml file should be created. This file should have the form of the example data.yaml file included in this repo.

## Set Path
```
Root_Dir="/path-to-your/YOLO-directory/"
import os
Dataset_Dir=os.path.join(Root_Dir,"TIC-Dataset")
```
## Create a yaml file
```
data=""
dtpath=Datase_Dir+'/train'+"/"
data=data+'train'+": "+dtpath+"\n"

dtpath=Dataset_Dir+'/valid'+"/"
data=data+'val'+": "+dtpath+"\n"

dtpath=DatasetNet_Dir+'/test'+"/"
data=data+'test'+": "+dtpath+"\n"

CatToKeep=['Tower','Insulator','Conductors']
data=data+"\nnc: "+str(len(CatToKeep))+"\n"
data=data+'names: '+str(CatToKeep)
filename="data.yaml"
try:
    os.remove(filename)
except:
    pass
f = open(filename, "a")
f.write(data)
f.close()
```
## Confirm data.yaml file is correct
```
%cat data.yaml
```
# Training Yolov8 using TIC-model's pre-trained weights

Please refer first to [Ultralytics Yolov8](https://github.com/ultralytics/ultralytics.git) to set up the environment, requirements etc.

## Download pre-trained weights
```
pip install gdown
gdown --id 109SxkukPRckQqxaoXifwS-LMsqiiWCF1
```
## Define number of classes based on data.yaml
```
import yaml
with open("data.yaml", 'r') as stream:
    data=yaml.safe_load(stream)
    classes=data['names']
    num_classes = str(data['nc'])

print(num_classes)
print(classes)
```
## Train (CLI)
```
cd /your/Yolo_directory
yolo detect train data=data.yaml model=tic_small.pt epochs=100 lr0=0.001 imgsz=640 #train your powerline detection model using our tic_small.pt
yolo detect val model=path/to/best.pt #evaluate your model
```
## Train (Python)
```
import torch

from ultralytics import YOLO

# Load a model
model = YOLO('/path-to-weights/tic_small.pt')

# Use the model
model.train(data="/data.yaml", lr0=0.001, epochs=100)  # train the model
metrics = model.val()  # evaluate model performance on the validation set
import torch
```
# Our model's predictions on video footage
Inference on drone footage for detecting towers, insulators and conductors can be found [here](https://youtu.be/6pstz7oj2uk)

## Citation
To use our data, please cite:

Bellou, E.; Pisica, I.; Banitsas, K. Aerial Inspection of High-Voltage Power Lines Using YOLOv8 Real-Time Object Detector. Energies 2024, 17(11), 2535; https://doi.org/10.3390/en17112535

