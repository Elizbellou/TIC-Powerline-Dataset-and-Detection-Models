# Tower-Insulator-Conductors-TIC-Dataset-and-Object-Detection-Models
1. The TIC Dataset consists of 2056 images (512x640) of transmission line network footage in Greece (Northeast Attica) and annotations of three object classes, i.e. towers, insulators and conductors. 
The TIC-Dataset file contains three sub-files, organized as follows: 
i. train dataset file ("train"):  -"Images" file with 1483 images and a JSON file with corresponding annotations (polygons)
                                  - "Labels" file with txt annotation files (YOLO format) for each train image.   
ii. validation dataset file ("valid"): -"Images" file with 366 images and a JSON file with corresponding annotations (polygons)
                                      - "Labels" file with txt annotation files (YOLO format) for each train image.
iii. Test dataset file ("test")": -"Images" file with 207 images and a JSON file with corresponding annotations (polygons)
                                  - "Labels" file with txt annotation files (YOLO format) for each train image.
2. TICmodels_weights file consists of four files (.pt) which correspond to the pre-trained weights of each TIC-model trained on the TIC-Dataset, using YOLOv8x ("tic_xlarge.pt"), YOLOv8l ("tic_large"), YOLOv8m (tic_medium.pt") and YOLOv8n ("tic_nano.pt").
3. Final_TICmodel file provides the pre-trained weights file (.pt) of the TIC-model trained on TIC-Dataset, YOLOv8s ("tic_small.pt"), as well as files related to the Results of this model, such as training and validation loss graphs, PR and F1-score curves, confusion matrix and predictions on validation images (JPEG).
4. Our models’ performance with Tesla T4 GPU (15GB), reaching 97% mAP@0.5 for towers detection with YOLOv8x TIC-model, are displayed in the table below. Input image size 640X640 and NMS time per image ≈ 1.5 - 2ms (not included): 
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
