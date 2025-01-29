```python
dls = DataBlock(
		blocks=(ImageBlock, CategoryBlock),
		get_items=get_image_files,
		splitter=RandomSplitter(valid_pct=0.2, seed=42),
		get_y=parent_label,
		item_tmfs=[Resize(192, method='squish')]
).dataloaders(path)
```

This class from FastAI allows you to load all the data from the dataset in an easy way.
- blocks: we tell the type of block we are going to use and its type of model
- get_items: from where to take the images
- splitter: the way to split the validation set in our dataset
- get_y: the way that we are going to get the label from each image
- item_tmfs: the preprocessing for each image before loading into the model
The dataloaders is from PyTorch funcionalities to load the dataset in GPU.

There are special Dataloaders for other type of models, for example: SegmentationDataLoaders.

https://docs.fast.ai/tutorial.html