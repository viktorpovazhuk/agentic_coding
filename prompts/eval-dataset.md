Write a script to run evaluation on a dataset from drone view for visual grounding in ODVG format. Model is original version of Grounding DINO.
Path to dataset images (e.g. /datasets2/datasets/aeroscapes/JPEGImages) and path to annotations (e.g. /datasets2/datasets/aeroscapes/Processed/corrected_prompts_train.jsonl) will be specified.
You can use @eval/eval_model.py as an example of model inference. But pay attention that we run inference not on videos, but on separate images.
Images with overlaid phrase, boxes and scores should be saved in specified output folder (e.g. output/eval). Image filename should be in the format {image_filename}_{phrase_idx}.jpg
Read corresponding data files to generate code that handles these files properly.