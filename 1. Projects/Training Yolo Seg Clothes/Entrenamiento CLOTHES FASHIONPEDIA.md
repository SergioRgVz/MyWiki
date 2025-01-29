Para lanzar un entrenamiento con FashionPedia, primero tenemos que limpiar el dataset tal cual lo descargamos, ya que viene en formato de etiquetado de COCO y en un solo fichero JSON.
El dataset lo podemos encontrar en este repositorio: [FashionPedia](https://github.com/cvdfoundation/fashionpedia#annotation-format)

Estas son todas sus categorías:

category_mapping = {
    0: "shirt, blouse",
    1: "top, t-shirt, sweatshirt",
    2: "sweater",
    3: "cardigan",
    4: "jacket",
    5: "vest",
    6: "pants",
    7: "shorts",
    8: "skirt",
    9: "coat",
    10: "dress",
    11: "jumpsuit",
    12: "cape",
    13: "glasses",
    14: "hat",
    15: "headband, head covering, hair accessory",
    16: "tie",
    17: "glove",
    18: "watch",
    19: "belt",
    20: "leg warmer",
    21: "tights, stockings",
    22: "sock",
    23: "shoe",
    24: "bag, wallet",
    25: "scarf",
    26: "umbrella",
    27: "hood",
    28: "collar",
    29: "lapel",
    30: "epaulette",
    31: "sleeve",
    32: "pocket",
    33: "neckline",
    34: "buckle",
    35: "zipper",
    36: "applique",
    37: "bead",
    38: "bow",
    39: "flower",
    40: "fringe",
    41: "ribbon",
    42: "rivet",
    43: "ruffle",
    44: "sequin",
    45: "tassel"
}

Para limpiar el dataset, primero dividiremos las etiquetas según imagen a la que se refiere ya que vienen relacionadas de la siguiente forma:

```JSON
{
 "info": info,
 "categories": [category],
 "attributes": [attribute],
 "images": [image],
 "annotations": [annotation],
 "licenses": [license]
}

info{
  "year" : int,
  "version" : str,
  "description" : str,
  "contributor" : str,
  "url" : str,
  "date_created" : datetime,
}

category{
  "id" : int,
  "name" : str,
  "supercategory" : str,  # parent of this label
  "level": int,           # levels in the taxonomy
  "taxonomy_id": string,
}

attribute{
  "id" : int,
  "name" : str,
  "supercategory" : str,  # parent of this label
  "level": int,           # levels in the taxonomy
  "taxonomy_id": string,
}

image{
  "id" : int,
  "width" : int,
  "height" : int,
  "file_name" : str,
  "license" : int,
  "time_captured": string,
  "original_url": string,
  "isstatic": int, 0: the original_url is not a static url,
  "kaggle_id": str,
}

annotation{
  "id" : int,
  "image_id" : int,
  "category_id" : int,
  "attribute_ids": [int],
  "segmentation" : [polygon] or [rle]
  "bbox" : [x,y,width,height], # int
  "area" : int
  "iscrowd": int (1 or 0)
}
polygon: [x1, y1, x2, y2, ...], where x, y are the coordinates of vertices, int
rle: {"size", (height, widht), "counts": str}

license{
  "id" : int,
  "name" : str,
  "url" : str
}
```

Para crear un fichero único por imagen con todas sus etiquetas (`create_unique_files.py`):
```Python
import json
import pandas as pd
import os
from tqdm import tqdm

def load_json_to_dataframes(file_path):
    with open(file_path, 'r') as f:
        data = json.load(f)
    
    images_df = pd.DataFrame(data['images'])
    annotations_df = pd.DataFrame(data['annotations'])
    categories_df = pd.DataFrame(data['categories'])
    
    return images_df, annotations_df, categories_df

def create_label_files(images_df, annotations_df, categories_df, output_dir):
    os.makedirs(output_dir, exist_ok=True)
    
    category_map = dict(zip(categories_df['id'], categories_df['name']))
    
    grouped_annotations = annotations_df.groupby('image_id')
    
    for index, image in tqdm(images_df.iterrows(), total=len(images_df), desc="Procesando imágenes"):
        image_id = image['id']
        file_name = image['file_name']
        
        image_annotations = grouped_annotations.get_group(image_id) if image_id in grouped_annotations.groups else pd.DataFrame()
        
        labels = []
        for _, annotation in image_annotations.iterrows():
            category_id = annotation['category_id']
            category_name = category_map.get(category_id, f"Unknown_{category_id}")
            
            label = {
                'category': category_id,
                'bbox': annotation['bbox'],
                'area': annotation['area'],
                'iscrowd': annotation['iscrowd'],
                'attribute_ids': annotation.get('attribute_ids', [])
            }
            
            if 'segmentation' in annotation:
                label['segmentation'] = annotation['segmentation']
            
            labels.append(label)
        
        image_data = {
            'file_name': file_name,
            'width': image['width'],
            'height': image['height'],
            'id': image_id,
            'license': image.get('license'),
            'time_captured': image.get('time_captured'),
            'original_url': image.get('original_url'),
            'isstatic': image.get('isstatic'),
            'kaggle_id': image.get('kaggle_id'),
            'labels': labels
        }
        
        output_file_name = os.path.splitext(file_name)[0] + '.json'
        output_file = os.path.join(output_dir, output_file_name)
        with open(output_file, 'w') as f:
            json.dump(image_data, f, indent=2)

file_path = '/home/sergio/workspace/herta/Train_Yolo8_PaperFashion/dataset/clean_dataset/instances_attributes_val2020.json'
output_dir = 'labels/'

print("Cargando datos...")
images_df, annotations_df, categories_df = load_json_to_dataframes(file_path)

print("Creando archivos de etiquetas...")
create_label_files(images_df, annotations_df, categories_df, output_dir)

print("Proceso completado. Los archivos de etiquetas se han guardado en:", output_dir)
```

Ahora tendremos los ficheros en un directorio labels únicos. Ahora convertiremos las etiquetas de estos ficheros a formato YOLO (`class x_center y_center width height`), para ello usaremos el fichero `convert_to_yolo.py`:

```Python
import json
import os
import glob
from tqdm import tqdm

def normalize_coordinates(coords, width, height):
    return [
        coords[i] / width if i % 2 == 0 else coords[i] / height
        for i in range(len(coords))
    ]

def process_json_file(json_file_path, output_dir):
    with open(json_file_path, 'r') as f:
        data = json.load(f)

    # Image dimensions
    width, height = data['width'], data['height']

    # Generate YOLO segmentation format
    yolo_labels = []
    for obj in data['labels']:
        class_id = obj['category']  # Usar directamente la categoría original
        
        # Handle different segmentation formats
        if 'segmentation' in obj:
            if isinstance(obj['segmentation'], list):
                if obj['segmentation'] and isinstance(obj['segmentation'][0], list):
                    coords = obj['segmentation'][0]
                elif obj['segmentation'] and isinstance(obj['segmentation'][0], (int, float)):
                    coords = obj['segmentation']
                else:
                    print(f"Unexpected segmentation format in {json_file_path}. Skipping this object.")
                    continue
            elif isinstance(obj['segmentation'], dict):
                # Handle RLE format if present
                print(f"RLE segmentation format found in {json_file_path}. Skipping this object.")
                continue
            else:
                print(f"Unexpected segmentation format in {json_file_path}. Skipping this object.")
                continue
        else:
            print(f"No segmentation data found in {json_file_path}. Skipping this object.")
            continue
        
        normalized_coords = normalize_coordinates(coords, width, height)
        yolo_line = f"{class_id} " + " ".join(map(str, normalized_coords))
        yolo_labels.append(yolo_line)

    # Write to label file in the output directory
    label_file_name = os.path.splitext(os.path.basename(json_file_path))[0] + '.txt'
    output_path = os.path.join(output_dir, label_file_name)
    with open(output_path, 'w') as f:
        f.write("\n".join(yolo_labels))

input_dir = 'labels'
output_dir = 'labels_converted'

os.makedirs(output_dir, exist_ok=True)

json_files = glob.glob(os.path.join(input_dir, '*.json'))

for json_file in tqdm(json_files, desc="Processing files", unit="file"):
    try:
        process_json_file(json_file, output_dir)
    except Exception as e:
        tqdm.write(f"Error processing {json_file}: {str(e)}")

print("\nAll files have been processed.")
```

Para poder entrenar correctamente, nos quedaremos con las clases menores que 27, ya que no nos interesa detectar las partes de la ropa, nos interesan las prendas completas:

```Python
import os
import glob
from tqdm import tqdm
import shutil

def filter_labels(input_path, output_path, max_class_id=26):
    with open(input_path, 'r') as f:
        lines = f.readlines()
    
    filtered_lines = [line for line in lines if int(line.split()[0]) <= max_class_id]
    
    with open(output_path, 'w') as f:
        f.writelines(filtered_lines)
    
    return len(filtered_lines) != len(lines)

def process_directory(input_directory, output_directory):
    if not os.path.exists(output_directory):
        os.makedirs(output_directory)
    
    label_files = glob.glob(os.path.join(input_directory, '*.txt'))
    
    modified_count = 0
    for label_file in tqdm(label_files, desc="Processing files"):
        output_file = os.path.join(output_directory, os.path.basename(label_file))
        if filter_labels(label_file, output_file):
            modified_count += 1
        else:
            # If the file wasn't modified, we still copy it to maintain the complete dataset
            shutil.copy2(label_file, output_file)
    
    print(f"Processed {len(label_files)} files.")
    print(f"Modified {modified_count} files.")
    print(f"All files (original and modified) have been saved to {output_directory}")

# Directorio que contiene los archivos de etiquetas originales
input_directory = '../val_cleaned'  # Cambia esto al directorio correcto de entrada

# Directorio donde se guardarán los archivos filtrados
output_directory = '../val_filtered'  # Cambia esto al directorio deseado de salida

# Procesar el directorio
process_directory(input_directory, output_directory)
```

Para probar si funciona correctamente, pintaremos las detecciones con un fichero de ejemplo del dataset:

```Python
import cv2
import numpy as np
import matplotlib.pyplot as plt
import os
from matplotlib.colors import LinearSegmentedColormap
from obj import category_mapping

# Create a custom colormap
colors = plt.cm.tab20(np.linspace(0, 1, 20))
custom_cmap = LinearSegmentedColormap.from_list("custom", colors, N=20)

def read_yolo_seg(txt_path, img_shape):
    with open(txt_path, 'r') as f:
        lines = f.readlines()
    
    masks = []
    for line in lines:
        data = line.strip().split()
        class_id = int(data[0])
        polygon = np.array([float(x) for x in data[1:]])
        polygon = polygon.reshape(-1, 2)
        polygon[:, 0] *= img_shape[1]  # scale x
        polygon[:, 1] *= img_shape[0]  # scale y
        polygon = polygon.astype(int)
        masks.append((class_id, polygon))
    
    return masks

def draw_masks(image, masks, alpha=0.5):
    overlay = image.copy()
    for class_id, polygon in masks:
        color = custom_cmap(class_id / 20)[:3]
        color = [int(c * 255) for c in color]
        cv2.fillPoly(overlay, [polygon], color)
        
        # Calculate centroid of the polygon
        M = cv2.moments(polygon)
        if M['m00'] != 0:
            cx = int(M['m10'] / M['m00'])
            cy = int(M['m01'] / M['m00'])
        else:
            cx, cy = polygon.mean(axis=0).astype(int)

        # Get category name
        category_name = category_mapping.get(class_id, f"Unknown ({class_id})")
        
        # Draw class ID and name
        text = f"{class_id}: {category_name}"
        cv2.putText(overlay, text, (cx, cy), cv2.FONT_HERSHEY_SIMPLEX, 
                    0.5, (255, 255, 255), 2, cv2.LINE_AA)
        cv2.putText(overlay, text, (cx, cy), cv2.FONT_HERSHEY_SIMPLEX, 
                    0.5, (0, 0, 0), 1, cv2.LINE_AA)
    
    return cv2.addWeighted(overlay, alpha, image, 1 - alpha, 0)

def visualize_segmentation(image_path, label_path, output_path):
    # Read image
    image = cv2.imread(image_path)
    if image is None:
        print(f"Error: Unable to read image from {image_path}")
        return
    image = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)

    # Read YOLO segmentation data
    masks = read_yolo_seg(label_path, image.shape)

    # Draw masks on image
    result = draw_masks(image, masks)

    # Save the visualization
    plt.figure(figsize=(12, 8))
    plt.imshow(result)
    plt.axis('off')
    plt.title(f'Segmentation Visualization: {os.path.basename(image_path)}')
    plt.savefig(output_path, bbox_inches='tight', pad_inches=0.1, dpi=300)
    plt.close()
    print(f"Visualization saved to {output_path}")

# Specific image and label paths
image_folder = './'
label_folder = 'labels_filtered'
image_filename = '0017a7bb9e2d8a0f44ce040ed164eea6.jpg'
label_filename = '0017a7bb9e2d8a0f44ce040ed164eea6.txt'

image_path = os.path.join(image_folder, image_filename)
label_path = os.path.join(label_folder, label_filename)
output_filename = f'visualization_{os.path.splitext(image_filename)[0]}.png'
output_path = output_filename  # Save in the current directory

# Check if both files exist
if os.path.exists(image_path) and os.path.exists(label_path):
    print(f"Processing {image_filename}")
    visualize_segmentation(image_path, label_path, output_path)
else:
    if not os.path.exists(image_path):
        print(f"Error: Image file not found at {image_path}")
    if not os.path.exists(label_path):
        print(f"Error: Label file not found at {label_path}")
```

Ahora bien que tenemos el dataset limpio, nos clonaremos el repositorio de Yolov8 de [Ultralytics](https://github.com/ultralytics/ultralytics)

Lanzaremos el contenedor de Docker con el siguiente comando:

`sudo docker run -it --ipc=host --gpus all -v [directorio_local_dataset]:/home/dataset/ ultralytics/ultralytics:latest`

Dentro del contenedor, abrimos una terminal nueva con `screen -S [Nombre_terminal]`,
y ejecutamos el siguiente comando:
`yolo segment train data=train_test.yaml model=yolov8n-seg.pt epochs=100 imgsz=640 batch=16`.

Para salir de la terminal screen, usamos ctrl + D, para volver a la terminal screen -r \[Nombre_Terminal].