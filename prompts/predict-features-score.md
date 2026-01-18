Add endpoint in @app/main.py that accept N frames + poses one-by-one for total features score prediction. There will one target object at current N frames. Number N should be read from env varibles as FEATURES_SCORE_NUM_FRAMES. Replicate the behaviour specified below for them.
But instead of iterating over frames, process each accepted frame separately:
- Predict all required objects on frame (for visual and spatial features) and save detections (or there absence) in some variable (we can use in-memory variables because we need to preserve results for small number of predictions).
- When all N frames are collected, find position of referent object and check spatial relation as it is done in code below.
Other required changes:
- Localizer now accepts drone poses + bounding boxes obtained from these poses to predict object position.
- Don't print any debug information that is printed in provided code.
- Add debug prints to track algorithm execution.
- Preserve saving debug images, such that I can check detection results.
Finally, calculate total score.

Earlier behaviour is in @algo/simulation_flight.py , starting from "Console.section("Step: Detect visual and spatial characteristics")", ending in "total_scores = np.array(total_scores)".