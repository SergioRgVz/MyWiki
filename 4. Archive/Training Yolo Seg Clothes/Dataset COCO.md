COCO annotations are inspired by the [Common Objects in Context (COCO) dataset](http://cocodataset.org/). 
COCO is a large-scale object detection, segmentation, and captioning dataset. COCO has several features: Object segmentation, Recognition in context, Superpixel stuff segmentation, 330K images (>200K labeled), 1.5 million object instances, 80 object categories, 91 stuff categories, 5 captions per image, 250,000 people with keypoints.
# Format

The COCO dataset is formatted in JSON and is a collection of “info”, “licenses”, “images”, “annotations”, “categories” (in most cases), and “segment info” (in one case).

```JSON
{
    "info": {...},
    "licenses": [...],
    "images": [...],
    "annotations": [...],
    "categories": [...], <-- Not in Captions annotations
    "segment_info": [...] <-- Only in Panoptic annotations
}
```

## Info Section

The “info” section contains high level information about the dataset. If you are creating your own dataset, you can fill in whatever is appropriate.

```JSON
"info": {
    "description": "COCO 2017 Dataset",
    "url": "http://cocodataset.org",
    "version": "1.0",
    "year": 2017,
    "contributor": "COCO Consortium",
    "date_created": "2017/09/01"
}
```

## Licenses Section

The “licenses” section contains a list of image licenses that apply to images in the dataset. If you are sharing or selling your dataset, you should make sure your licenses are correctly specified and that you are not infringing on copyright.

```JSON
"licenses": [
    {
        "url": "http://creativecommons.org/licenses/by-nc-sa/2.0/",
        "id": 1,
        "name": "Attribution-NonCommercial-ShareAlike License"
    },
    {
        "url": "http://creativecommons.org/licenses/by-nc/2.0/",
        "id": 2,
        "name": "Attribution-NonCommercial License"
    },
    ...
]
```

## Images Section

The “images” section contains the complete list of images in your dataset. There are no labels, bounding boxes, or segmentations specified in this part, it's simply a list of images and information about each one. Note that coco_url, flickr_url, and date_captured are just for reference. Your deep learning application probably will only need the file_name.

- id – (Required) A unique identifier for the image. The id field maps to the id field in the annotations array (where bounding box information is stored).
- license – (Not Required) Maps to the license array.
- coco_url – (Optional) The location of the image.
- flickr_url – (Not required) The location of the image on Flickr.
- width – (Required) The width of the image.
- height – (Required) The height of the image.
- file_name – (Required) The image file name. In this example, file_name and id match, but this is not a requirement for COCO datasets.
- date_captured –(Required) the date and time the image was captured.


```JSON
"images": [
    {
        "license": 4,
        "file_name": "000000397133.jpg",
        "coco_url": "http://images.cocodataset.org/val2017/000000397133.jpg",
        "height": 427,
        "width": 640,
        "date_captured": "2013-11-14 17:02:52",
        "flickr_url": "http://farm7.staticflickr.com/6116/6255196340_da26cf2c9e_z.jpg",
        "id": 397133
    },
    {
        "license": 1,
        "file_name": "000000037777.jpg",
        "coco_url": "http://images.cocodataset.org/val2017/000000037777.jpg",
        "height": 230,
        "width": 352,
        "date_captured": "2013-11-14 20:55:31",
        "flickr_url": "http://farm9.staticflickr.com/8429/7839199426_f6d48aa585_z.jpg",
        "id": 37777
    },
    ...
]
```

## Categories Section

The “categories” object contains a list of categories (e.g. dog, boat) and each of those belongs to a supercategory (e.g. animal, vehicle). The original COCO dataset contains 90 categories. You can use the existing COCO categories or create an entirely new list of your own. Each category id must be unique (among the rest of the categories).

- supercategory – (Not required) The parent category for a label.
- id – (Required) The label identifier. The id field maps to the category_id field in an annotation object. In the following example, The identifier for an echo dot is 2.
- name – (Required) the label name.
```JSON
"categories": [
    {"supercategory": "person","id": 1,"name": "person"},
    {"supercategory": "vehicle","id": 2,"name": "bicycle"},
    {"supercategory": "vehicle","id": 3,"name": "car"},
    {"supercategory": "vehicle","id": 4,"name": "motorcycle"},
    {"supercategory": "vehicle","id": 5,"name": "airplane"},
    ...
    {"supercategory": "indoor","id": 89,"name": "hair drier"},
    {"supercategory": "indoor","id": 90,"name": "toothbrush"}
]
```

## Annotations (Bounding Boxes) section

Bounding box information for all objects on all images is stored the annotations list. A single annotation object contains bounding box information for a single object's label on an image. There is an annotation object for each instance of an object on an image.

In the following example, note the following information and which fields are required to create your dataset.

- id – (Not required) The identifier for the annotation.
- image_id – (Required) Corresponds to the image id in the images array.
- category_id – (Required) The identifier for the label that identifies the object within a bounding box. It maps to the id field of the categories array.
- iscrowd – (Not required) Specifies if the image contains a crowd of objects.
- segmentation – (Not required) Segmentation information for objects on an image. Amazon Rekognition Custom Labels doesn't support segmentation.
- area – (Not required) The area of the annotation.
- bbox – (Required) Contains the coordinates, in pixels, of a bounding box around an object on the image.

```JSON
{
    "id": 1409619,
    "category_id": 1,
    "iscrowd": 0,
    "segmentation": [
        [86.0, 238.8,..........382.74, 241.17]
    ],
    "image_id": 245915,
    "area": 3556.2197000000015,
    "bbox": [86, 65, 220, 334]
}
```


