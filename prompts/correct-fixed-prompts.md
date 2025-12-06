Write a script similar to @data/eval_prompts_gen.py that will load prompts from CSV (e.g. "/datasets2/datasets/aeroscapes/Processed/fixed_prompts.csv"), read it 50 rows at a time, and correct "phrase" column in them using GPT-5 API using the prompt below. To this end, send list of phrases to the GPT and accept list from it. Specify JSON structure to GPT to make it generate structured output. Then collect all rows with corrected phrases and save in the output csv with the same structure at specified path (e.g. "/datasets2/datasets/aeroscapes/Processed/correct_fixed_prompts.csv").
Prompt to GPT:
"""There are phrases for refference expression comprehension task.
Fix them according to the description:
1. fix typos
2. fix incorrect words ordering (e.g. "car in black" should become "black car")
3. remove articles
4. fix grammar mistakes
5. replace incorrectly used words with correct words
Phrases:
{{PHRASES_LIST}}"""