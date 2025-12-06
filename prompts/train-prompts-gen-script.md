Create a script “gen_train_prompts.py” in @data. Script should be similar to “gen_eval_prompts.py”. It should iterate over samples in specified in config JSON file (e.g. "/datasets2/datasets/aeroscapes/Processed/samples_filtered_boxes.json") and use GPT API to generate prompts for objects in bounding boxes.
You should also extract all variables and prompts in the config file. Config should be similar to “gen_eval_prompts_visual.yaml”.
Script should ask to generate prompts with visual characteristics. The same as in “gen_eval_prompts_visual.yaml”.
It should be possible to specify images root dir in config (e.g. “/datasets2/datasets/aeroscapes/JPEGImages”). And output JSONL file (e.g. "/datasets2/datasets/aeroscapes/Processed/prompts_gen/prompts.jsonl").
Output JSONL file should have the next structure:
{
  "filename": "<img_filename>",
  "height": <img_height>,
  "width": <img_width>,
  “crop_fn” : “<crop_filename>”,
  "grounding": {
      "regions": [
        {
          "bbox": [20,215,985,665], # [x1,y1,x2,y2]
          “box_crop_fn”: “<box_crop_filename>”,
          “crop_with_box_fn”: "<crop_with_box_filename>",
          "phrase": "person near car"
        },
        { 
          "bbox": [19,19,982,671],
          “box_crop_fn”: “<box_crop_filename>”,
          “crop_with_box_fn”: "<crop_with_box_filename>",
          "phrase": "a wire hanger"
        }
      ]
    }
}
{
  "filename": "<img_filename>",
  "height": <img_height>,
  "width": <img_width>,
  ...
}
{
  ...
}
Save cropped images in "images/crop" directory in root of prompts generation results, crops by bounding boxes in "images/box_crop", cropped images with overlaid boxes in "images/crop_with_box". Save used config in config.yaml in root of prompts generation results.
Read corresponding csv and json files to generate code that handles these files properly.