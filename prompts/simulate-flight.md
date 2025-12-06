Write function simulate_flight_flow in @algo/simulation_flight.py that will send requests to detector and localizer endpoints (inspect functions in file @algo/simulation_tests.py to understand how to send appropriate requests and what endpoints are available). Use the same console and client implementations as in @algo/simulation_tests.py.
If you don't find endpoint for specific action, just replace call to it with comment that you make a call.
Instead of commanding drone to fly in some point in 3D grom which we will predict for prompts, we will just specify a list of frame ids on which we will predict for visual and spatial prompts. These frame ids should be saved in some type of config file in one folder with script.
There is an algorithm of flight that we simulate:

Actual detection steps:

1. Detect house numbers ⇒ Classify numbers “X”
    
    Use house numbers “X” as an anchor around which target object will be detected
    
    Model: YOLO
    
    FPS: high
    
2. Estimate house numbers location in 3D space
    
    Necessary to move around our anchor point
    
3. Estimate 3D map of the scene as close as possible
    
    Not to crash the drone during flights around
    
4. Fly around the house number and detect all objects Y
    
    Fly around the house number to look at the scene from different angles because sometimes objects can be detected only from specific angle due to model’s imperfections. Or object can be occluded from specific angle by bushes, for example
    
    And detect all objects Y (for example, all persons) around this house (without filtering by any specific characteristics at this step) to build a list of target object proposals
    
    In each position take a few frames of the video with a pause to filter objects Y and leave only static objects Y. To this end, compare bboxes of objects. If on most of the frames bboxes don’t overlay with ≥ 80%, but exist nearby, then drop these objects
    
    Model: GD-B
    
    FPS: low
    
5. Identify objects locations in 3D space
    
    To later estimate distance to them to be not too close and not to far when detecting spatial and visual characteristics
    
6. For each filtered object Y detect visual characteristics
    1. Fly as close as possible to better detect visual characteristics (~8 m to the object)
    2. Look at object from different angles while detecting current characteristics provided in the prompt. It’s also worth just to stay at each position looking at object because object may change its position somehow over time. Collect detection results for each processed frame. Remove similarities (a.k.a. detection confidences) outliers. Calculate mean of the left results. Save for further processing
        
        For multi-level features we will need to build hierarchy tree (e.g. “person in jacket” ⇒ “red jacket”)
        
    3. Filter objects that have mean similarity (without 25% of removed outliers) for any of current characteristics < SPATIAL_THRESHOLD (0.2). Preserve results for current characteristics for final decision
    
    Model: GD-B
    
    FPS: low
    
7. For each filtered object Y detect spatial characteristics
    1. Fly as close as possible, but such that it is still possible to capture spatial characteristics, (~10 m to the object)
    2. Look at object from different angles while detecting current characteristics provided in the prompt. It will be perfect if we fly from right to left and back looking at the current object. It’s also worth just to stay at each position looking at object because object may change its position somehow over time. Collect detection results for each processed frame. Remove similarities (a.k.a. detection confidences) outliers. Calculate mean of the left results. Save for further processing
        
        For multi-level features we will need to build hierarchy tree (e.g. “person near bicycle” ⇒ “red bicycle”)
        
    3. Filter objects that have mean similarity (without 25% of removed outliers) for any of current characteristics < SPATIAL_THRESHOLD (0.2). Preserve results for current characteristics for final decision
    
    Model: GD-B
    
    FPS: low
    
8. For each filtered object Y calculate distance characteristics
    1. Fly as close as possible, but such that it is still possible to capture distance characteristics, (~10 m to the object)
    2. Detect objects mentioned in prompt (e.g. “… closest to the *van*”). Do it from different angles and repeat all other steps mentioned in previous detection processes. Use Localizer to localize them in 3D space. Then calculate distances between required pairs of objects
    3. Select objects that satisfies the condition of the prompt (e.g. objects with the greatest distance if prompt specified “the most distant from”)
9. Select objects which has similarity for each characteristics > MEAN_THRESHOLD (0.5)
10. Among filtered objects Y find one - closest to the numbers of house X
    
    Use 3D map to calculate distance between each pair of house numbers and object Y