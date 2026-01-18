We need to: generate one spatial prompt for each main object that uniquely selects it, and doesn't describe other main objects.
Rewrite code to generate these prompts:
- For each main object send the whole image with overlaid box and cropped image with bounding box (as it is already sent) to the Gemini. In request, ask Gemini to generate all possible spatial prompts for this main object with every other objects. Generate prompts using relation words only from specified list [
    near
    next to
    in front of
    behind
    to the left of
    to the right of
    under
    on
    inside
    between
    at the corner of
    to the side of
]. Generate prompt with every relation word that is possible to use for current pair of main and other object.
System prompt for request is described in @prompt_all_phrases_rev1.md.
Output from the model should be in format [prompt1, prompt2, prompt3, ...].
Collect list of phrases for each object like: {'main_object_0': [prompt1, prompt2, prompt3, ...], 'main_object_1': [...], ...}.
- Then for each main object send the whole image with overlaid box and cropped image with bounding box (as it is already sent) to the Gemini. Append list of possible prompts for this target object. And separately lists for all other main objects. Ask to select one prompt among the list of phrases for this main object. Selected prompt must uniquely select current main object and mustn't describe any other main objects.
System prompt for request is described in @prompt_select_phrase_rev1.md.
Output from the model should be in format 'prompt', without any additional information, explanation or formatting.
- Assign field data["main_objects_prompts"] to list of selected prompts with None's inserted in appropriate positions, such that ordering of phrases correspond to initial ordering of boxes.
---------------------------------
- Inspect files with system prompts to better understand what is already told to the model, and what is needed to add in the script.