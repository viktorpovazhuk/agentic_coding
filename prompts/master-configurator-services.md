Write a small FastAPI-based web app that will be used for configuring different services: Orchestrator, Detector, Localizer. It will have only one main page on which it will accept parameters for services.
Configurator should work on port "8642".
Currently, it will send only prompt, target char spans (a.k.a. tokens positive), mission type to Detector. It will accept these parameters from user in web interface fields in the Detector section.
Detector endpoint should be configurable. Choose some best practice to save configuration. By default, detector is located at "http://foxfour-jetson:8765".
Description of Detector endpoint that accepts prompt and target char spans:
@app.post("/set-prompt")
async def set_prompt(prompt: str = Form(...), target_char_spans: str = Form(...)):
    app_state["prompt"] = prompt
    app_state["target_char_spans"] = json.loads(target_char_spans)
    return {"message": "Prompt and target_char_spans saved"}
Description of Detector endpoint that accepts mission type:
@app.post("/set-mission-type")
async def set_mission_type(mission_type: MissionType = Form(...)):
    app_state["mission_type"] = mission_type.value
    return {"message": "Mission type saved"}
class MissionType(str, Enum):
    LMD = "LMD"
    SAR = "SAR"
Prepare requirements.txt to create venv in which app will run.
Prepare short README.md.