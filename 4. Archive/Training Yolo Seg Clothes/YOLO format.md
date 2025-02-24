The dataset label used for training YOLO segmentation model is as follows:
1. One text file per image: Each image in the dataset has a corresponding text file with the same name as the iage file and the ".txt" extension
2. One row per object: Each row in the text file corresponding to one object instance in the image
3. Object information per row: Each row contains the following information about the object instance:
   - Object class index: An integer representing the class of the object (e.g. 0, for person, 1 for car, etc.).
   - Object bounding coordinates: The bounding coordinates around the mask area, normalized between 0 and 1.

The format for a single row in the segmentation dataset file is as follows:
`<class-index> <x1> <y1> <x2> <y2> ... <xn> <yn>`

In this format, `<class-index>` is the index of the class for the object, and `<x1> <y1> <x2> <y2> ... <xn> <yn>`are the bounding coordinates of the object's segmentation mask. The coordinates are separated by spaces.

Here is an example of the YOLO dataset format for a single image with two objects made up of a 3-point segment and a 5-point segment: 

```
0 0.681 0.485 0.670 0.487 0.676 0.487
1 0.504 0.000 0.501 0.004 0.498 0.004 0.493 0.010 0.492 0.0104
```
