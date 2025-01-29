https://www.aitude.com/annotation-converters-for-object-detection/
https://learnopencv.com/train-yolov8-on-custom-dataset/
https://leimao.github.io/blog/Inspecting-COCO-Dataset-Using-COCO-API/

https://towardsdatascience.com/trian-yolov8-instance-segmentation-on-your-data-6ffa04b2debd
https://docs.ultralytics.com/datasets/segment/#ultralytics-yolo-format
https://freedium.cfd/https://towardsdatascience.com/master-the-coco-dataset-for-semantic-image-segmentation-part-1-of-2-732712631047
https://docs.ultralytics.com/integrations/tensorboard/#usage

# Prerequisites
- Have Docker installed in your machine and nvidia-container-toolkit
- Have a dataset labeled in [[YOLO format]]

## Running the docker container:

First of all, we need the latest image of ultralytics from Docker hub in our system:
`sudo docker pull ultralytics/ultralytics:latest`

Now, for our training we are going to run the container with these parameters:
```bash
sudo docker run -d --name Train_Yolov8 --gpus all 
-p 6006:6006 --shm-size=8g
-v /home/herty/workspace/training_clothes_segmentation/datasets:/home/datasets -v /home/herty/workspace/training_clothes_segmentation/tra
inings:/home/trainings ultralytics/ultralytics:latest sleep infinity
```

- --gpus all: For asigning all the gpus to the container
- The first volume must be /path/to/your/datasets/in/local/machine:/home/datasets
- The second volume is for saving the weights of our training: /path/to/save/your/trainings:/home/trainings
- ultralytics/ultralytics:image : the ultralytics image
- sleep infinity: for having the container up at the most time training
- -p 6006:6006: forwarding the port 6066, used by ultralytics to launch Tensorboard to your local machine
- --shm-size=8g: for assigning more shared memmory to the container, the training needs a lot of resources

## Prepare your dataset
Ultralytics use a YAML file as configuration file, this is an example for the COCO dataset:
```YAML
# Train/val/test sets as 1) dir: path/to/imgs, 2) file: path/to/imgs.txt, or 3) list: [path/to/imgs1, path/to/imgs2, ..]
path: ../datasets/coco128 # dataset root dir
train: images/train2017 # train images (relative to 'path') 128 images
val: images/train2017 # val images (relative to 'path') 128 images
test: # test images (optional)

# Classes (80 COCO classes)
names:
    0: person
    1: bicycle
    2: car
    # ...
    77: teddy bear
    78: hair drier
    79: toothbrush
```

# Training scripts

In ultralytics there are two ways to launch a training, launch by CLI or launching via script, we are training with the script run_training.py in this repo. In this file we have some configurations in the CONFIG object as it follows:

```Python
CONFIG = {
    'model': 'yolov8n-seg.pt',
    'data': '/home/trainings/COCO_Segment_Clothes.yaml',
    'epochs': 100,
    'imgsz': 640,
    'batch': 16,
    'save_period': 1,
    'project': 'COCO_Segment_Clothes',
    'name': f'test_{datetime.now().strftime("%d-%m-%Y_%H_%M_%S")}',
    'workers': 16,
    'optimizer': 'AdamW', 
    'lr0': 0.002,  
    'lrf': 0.01,  
    'val': True, 
    'amp': True,
    'cos_lr': True
}
```

Here's a list of the possible configurations you can do for your training in this object:

|     Argument    | Default |                                                                                                 Description                                                                                                 |
|:---------------:|:-------:|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| model           | None    | Specifies the model file for training. Accepts a path to either a .pt pretrained model or a .yaml configuration file. Essential for defining the model structure or initializing weights.                   |
| data            | None    | Path to the dataset configuration file (e.g., coco8.yaml). This file contains dataset-specific parameters, including paths to training and validation data, class names, and number of classes.             |
| epochs          | 100     | Total number of training epochs. Each epoch represents a full pass over the entire dataset. Adjusting this value can affect training duration and model performance.                                        |
| time            | None    | Maximum training time in hours. If set, this overrides the epochs argument, allowing training to automatically stop after the specified duration. Useful for time-constrained training scenarios.           |
| patience        | 100     | Number of epochs to wait without improvement in validation metrics before early stopping the training. Helps prevent overfitting by stopping training when performance plateaus.                            |
| batch           | 16      | Batch size, with three modes: set as an integer (e.g., batch=16), auto mode for 60% GPU memory utilization (batch=-1), or auto mode with specified utilization fraction (batch=0.70).                       |
| imgsz           | 640     | Target image size for training. All images are resized to this dimension before being fed into the model. Affects model accuracy and computational complexity.                                              |
| save            | True    | Enables saving of training checkpoints and final model weights. Useful for resuming training or model deployment.                                                                                           |
| save_period     | -1      | Frequency of saving model checkpoints, specified in epochs. A value  of -1 disables this feature. Useful for saving interim models during  long training sessions.                                          |
| cache           | False   | Enables caching of dataset images in memory (True/ram), on disk (disk), or disables it (False). Improves training speed by reducing disk I/O at the cost of increased memory usage.                         |
| device          | None    | Specifies the computational device(s) for training: a single GPU (device=0), multiple GPUs (device=0,1), CPU (device=cpu), or MPS for Apple silicon (device=mps).                                           |
| workers         | 8       | Number of worker threads for data loading (per RANK  if Multi-GPU training). Influences the speed of data preprocessing and  feeding into the model, especially useful in multi-GPU setups.                 |
| project         | None    | Name of the project directory where training outputs are saved. Allows for organized storage of different experiments.                                                                                      |
| name            | None    | Name of the training run. Used for creating a subdirectory within  the project folder, where training logs and outputs are stored.                                                                          |
| exist_ok        | False   | If True, allows overwriting of an existing project/name directory.  Useful for iterative experimentation without needing to manually clear  previous outputs.                                               |
| pretrained      | True    | Determines whether to start training from a pretrained model. Can be  a boolean value or a string path to a specific model from which to load  weights. Enhances training efficiency and model performance. |
| optimizer       | 'auto'  | Choice of optimizer for training. Options include SGD, Adam, AdamW, NAdam, RAdam, RMSProp etc., or auto for automatic selection based on model configuration. Affects convergence speed and stability.      |
| verbose         | False   | Enables verbose output during training, providing detailed logs and  progress updates. Useful for debugging and closely monitoring the  training process.                                                   |
| seed            | 0       | Sets the random seed for training, ensuring reproducibility of results across runs with the same configurations.                                                                                            |
| deterministic   | True    | Forces deterministic algorithm use, ensuring reproducibility but may  affect performance and speed due to the restriction on  non-deterministic algorithms.                                                 |
| single_cls      | False   | Treats all classes in multi-class datasets as a single class during  training. Useful for binary classification tasks or when focusing on  object presence rather than classification.                      |
| rect            | False   | Enables rectangular training, optimizing batch composition for  minimal padding. Can improve efficiency and speed but may affect model  accuracy.                                                           |
| cos_lr          | False   | Utilizes a cosine learning rate  scheduler, adjusting the learning rate following a cosine curve over  epochs. Helps in managing learning rate for better convergence.                                      |
| close_mosaic    | 10      | Disables mosaic data augmentation in the last N epochs to stabilize training before completion. Setting to 0 disables this feature.                                                                         |
| resume          | False   | Resumes training from the last saved checkpoint. Automatically loads  model weights, optimizer state, and epoch count, continuing training  seamlessly.                                                     |
| amp             | True    | Enables Automatic Mixed Precision (AMP) training, reducing memory usage and possibly speeding up training with minimal impact on accuracy.                                                                  |
| fraction        | 1.0     | Specifies the fraction of the dataset to use for training. Allows  for training on a subset of the full dataset, useful for experiments or  when resources are limited.                                     |
| profile         | False   | Enables profiling of ONNX and TensorRT speeds during training, useful for optimizing model deployment.                                                                                                      |
| freeze          | None    | Freezes the first N layers of the model or specified layers by  index, reducing the number of trainable parameters. Useful for  fine-tuning or transfer learning.                                           |
| lr0             | 0.01    | Initial learning rate (i.e. SGD=1E-2, Adam=1E-3) . Adjusting this value is crucial for the optimization process, influencing how rapidly model weights are updated.                                         |
| lrf             | 0.01    | Final learning rate as a fraction of the initial rate = (lr0 * lrf), used in conjunction with schedulers to adjust the learning rate over time.                                                             |
| momentum        | 0.937   | Momentum factor for SGD or beta1 for Adam optimizers, influencing the incorporation of past gradients in the current update.                                                                                |
| weight_decay    | 0.0005  | L2 regularization term, penalizing large weights to prevent overfitting.                                                                                                                                    |
| warmup_epochs   | 3.0     | Number of epochs for learning rate warmup, gradually increasing the  learning rate from a low value to the initial learning rate to stabilize  training early on.                                           |
| warmup_momentum | 0.8     | Initial momentum for warmup phase, gradually adjusting to the set momentum over the warmup period.                                                                                                          |
| warmup_bias_lr  | 0.1     | Learning rate for bias parameters during the warmup phase, helping stabilize model training in the initial epochs.                                                                                          |
| box             | 7.5     | Weight of the box loss component in the loss function, influencing how much emphasis is placed on accurately predicting bounding box coordinates.                                                           |
| cls             | 0.5     | Weight of the classification loss in the total loss function,  affecting the importance of correct class prediction relative to other  components.                                                          |
| dfl             | 1.5     | Weight of the distribution focal loss, used in certain YOLO versions for fine-grained classification.                                                                                                       |
| pose            | 12.0    | Weight of the pose loss in models trained for pose estimation, influencing the emphasis on accurately predicting pose keypoints.                                                                            |
| kobj            | 2.0     | Weight of the keypoint objectness loss in pose estimation models, balancing detection confidence with pose accuracy.                                                                                        |
| label_smoothing | 0.0     | Applies label smoothing, softening hard labels to a mix of the  target label and a uniform distribution over labels, can improve  generalization.                                                           |
| nbs             | 64      | Nominal batch size for normalization of loss.                                                                                                                                                               |
| overlap_mask    | True    | Determines whether segmentation masks should overlap during training, applicable in instance segmentation tasks.                                                                                            |
| mask_ratio      | 4       | Downsample ratio for segmentation masks, affecting the resolution of masks used during training.                                                                                                            |
| dropout         | 0.0     | Dropout rate for regularization in classification tasks, preventing overfitting by randomly omitting units during training.                                                                                 |
| val             | True    | Enables validation during training, allowing for periodic evaluation of model performance on a separate dataset.                                                                                            |
| plots           | False   | Generates and saves plots of training and validation metrics, as  well as prediction examples, providing visual insights into model  performance and learning progression.                                  |

Once you have configured your training, just do the following process to launch your training (in the directory of python file):

```bash
screen -S [Your_training_name]
python3 run_training.py
```

# Monitorize the training

For monitorize the training, Ultralytics have Tensorboard configuration inside its scripts, to run it we just need to execute the following line in another terminal in our machine: 

`tensorboard --logdir path/to/our/training`

And then we can go to http://localhost:6006/ in our machine to check if the training is going alright or not.

The three key sections of the TensorBoard are Time Series, Scalars and Graphs.

## Time Series

This section offers a dynamic and detailed perspective of various training metrics over time for YOLO models. It focuses on the progression and trends of metrics accross training epochs. Here's an example of what can you expect to see.
![[Pasted image 20241004093520.png]]

The Time Series section lets you track the metrics in real time to promptly identify and solve issues. It also offers a detailed view of each metrics progression, which is crucial for fine-tuning the model and enhancing its performance.

## Scalars

Scalars lets us see the plotting and analyzing simple metrics like loss and accuracy during the training. They offer a clear view of how these metrics evolve with each training epoch, providing insights into the model's learning effectiveness and stability. Here's an example:
![[Pasted image 20241004093813.png]]
### Key Features of Scalars in Tensorboard
- **Learning Rate (lr) Tags**: These tags show the variations in the learning rate accross different segments (e.g., p0, pg1, pg2). This helps us understand the impact of learning rate adjustments on the training process.
- **Metrics tags**: scalars include perfomance indicatos such as:
	- `map50 (B)`: Mean Average Precision at 50% Intersection over Union (IoU), crucial for assesing comprehensive detection accuracy.
	- `map50-95 (B)`: Mean Average Precision calculated over a range of IoU thresholds, offering a more comprehensive evaluation of accuracy.
	- `Precision (B)`: Indicates the ratio of correctly predicted positive observations, key to understanding prediction accuracy.
	- `Recall (B)`: important for models where missing a detection is significant, this metric measures the ability to detect all relevant instances.
- **Training and Validations Tags (train, val)**: these tags display metrics specifically for the training validation datasets, allowing for a comparative analysis of model performance across different data sets.

## Graphs

The graphs section of the TensorBoard visualizes the computation graph of our YOLO model, showing how operations and data flow within the model. It's a powerful tool for understanding the model's structure, ensuring that all layers are connected correctly, and for identifying any potential bottlenecks in data flow.
![[Pasted image 20241004094453.png]]