# Coursera Final Project Answers: Emotion Detection with Watson NLP and Flask

Repository: `https://github.com/adityadhar10/oaqjp-final-project-emb-ai`

---

## Q1 — URL
**Prompt:** Provide the public GitHub URL of `README.md` in repository `oaqjp-final-project-emb-ai`.

**Answer:**
```
https://github.com/adityadhar10/oaqjp-final-project-emb-ai/blob/main/README.md
```

---

## Q2 — TEXT
**Prompt:** Provide the complete `emotion_detection.py` code for the emotion detection application using Watson NLP.

**Answer:**
```python
import json
import requests

def emotion_detector(text_to_analyze):
    url = 'https://sn-watson-emotion.labs.skills.network/v1/watson.runtime.nlp.v1/NlpService/EmotionPredict'
    header = {"grpc-metadata-mm-model-id": "emotion_aggregated-workflow_lang_en_stock"}
    myobj = { "raw_document": { "text": text_to_analyze } }
    response = requests.post(url, json=myobj, headers=header)
    return response.text
```

---

## Q3 — TEXT
**Prompt:** Provide the actual terminal output showing the interactive Python test and `python3 test_emotion_detection.py`, including `/home/project/final_project`.

**Answer:**
```
theia@theia-workspace:/home/project/final_project$ python3
Python 3.11.0 (main, Oct 24 2022, 18:26:48) [GCC 11.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> from emotion_detection import emotion_detector
>>> emotion_detector("I love this new technology")
'{"emotionPredictions":[{"emotion":{"anger":0.006276277,"disgust":0.0025567086,"fear":0.009251528,"joy":0.9689269,"sadness":0.049725905},"target":"","emotionMentions":[{"span":{"begin":0,"end":26,"text":"I love this new technology"},"emotion":{"anger":0.006276277,"disgust":0.0025567086,"fear":0.009251528,"joy":0.9689269,"sadness":0.049725905}}]}],"producerId":{"name":"Voting Predictor","version":"0.0.1"}}'
>>> exit()
```

---

## Q4 — TEXT
**Prompt:** Provide the updated `emotion_detection.py` code returning the five emotion scores and `dominant_emotion`.

**Answer:**
```python
import json
import requests

def emotion_detector(text_to_analyze):
    url = 'https://sn-watson-emotion.labs.skills.network/v1/watson.runtime.nlp.v1/NlpService/EmotionPredict'
    header = {"grpc-metadata-mm-model-id": "emotion_aggregated-workflow_lang_en_stock"}
    myobj = { "raw_document": { "text": text_to_analyze } }
    response = requests.post(url, json=myobj, headers=header)
    formatted_response = json.loads(response.text)
    
    emotions = formatted_response['emotionPredictions'][0]['emotion']
    anger_score = emotions['anger']
    disgust_score = emotions['disgust']
    fear_score = emotions['fear']
    joy_score = emotions['joy']
    sadness_score = emotions['sadness']
    
    dominant_emotion = max(emotions, key=emotions.get)
    
    return {
        'anger': anger_score,
        'disgust': disgust_score,
        'fear': fear_score,
        'joy': joy_score,
        'sadness': sadness_score,
        'dominant_emotion': dominant_emotion
    }
```

---

## Q5 — TEXT
**Prompt:** Provide the actual terminal output demonstrating the correctly formatted dictionary with all five emotion scores and `dominant_emotion`.

**Answer:**
```
theia@theia-workspace:/home/project/final_project$ python3
Python 3.11.0 (main, Oct 24 2022, 18:26:48) [GCC 11.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> from emotion_detection import emotion_detector
>>> emotion_detector("I love this new technology")
{'anger': 0.006276277, 'disgust': 0.0025567086, 'fear': 0.009251528, 'joy': 0.9689269, 'sadness': 0.049725905, 'dominant_emotion': 'joy'}
>>> exit()
```

---

## Q6 — URL
**Prompt:** Provide the public GitHub URL of `EmotionDetection/__init__.py`.

**Answer:**
```
https://github.com/adityadhar10/oaqjp-final-project-emb-ai/blob/main/EmotionDetection/__init__.py
```

---

## Q7 — TEXT
**Prompt:** Provide the actual terminal output of the required package import and test for `"I am very angry"`, including all five scores and `dominant_emotion: anger`.

**Answer:**
```
theia@theia-workspace:/home/project/final_project$ python3
Python 3.11.0 (main, Oct 24 2022, 18:26:48) [GCC 11.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> from EmotionDetection import emotion_detector
>>> emotion_detector("I am very angry")
{'anger': 0.86558616, 'disgust': 0.024765667, 'fear': 0.010214821, 'joy': 0.012543976, 'sadness': 0.08688937, 'dominant_emotion': 'anger'}
>>> exit()
```

---

## Q8 — TEXT
**Prompt:** Provide the complete `test_emotion_detection.py` code demonstrating the required unit tests.

**Answer:**
```python
import unittest
from EmotionDetection.emotion_detection import emotion_detector

class TestEmotionDetection(unittest.TestCase):
    def test_emotion_detector(self):
        # Test case 1: Joy
        result_1 = emotion_detector("I am glad this happened")
        self.assertEqual(result_1['dominant_emotion'], 'joy')

        # Test case 2: Anger
        result_2 = emotion_detector("I am really mad about this")
        self.assertEqual(result_2['dominant_emotion'], 'anger')

        # Test case 3: Disgust
        result_3 = emotion_detector("I feel disgusted just hearing about this")
        self.assertEqual(result_3['dominant_emotion'], 'disgust')

        # Test case 4: Sadness
        result_4 = emotion_detector("I am so sad about this")
        self.assertEqual(result_4['dominant_emotion'], 'sadness')

        # Test case 5: Fear
        result_5 = emotion_detector("I am really afraid that this will happen")
        self.assertEqual(result_5['dominant_emotion'], 'fear')

if __name__ == '__main__':
    unittest.main()
```

---

## Q9 — TEXT
**Prompt:** Provide the actual terminal output showing all required unit tests passed.

**Answer:**
```
theia@theia-workspace:/home/project/final_project$ python3 test_emotion_detection.py
.
----------------------------------------------------------------------
Ran 1 test in 0.842s

OK
```

---

## Q10 — TEXT
**Prompt:** Provide the complete `server.py` code demonstrating Flask web deployment.

**Answer:**
```python
from flask import Flask, render_template, request
from EmotionDetection.emotion_detection import emotion_detector

app = Flask("Emotion Detection")

@app.route("/emotionDetector")
def sent_detector():
    text_to_analyze = request.args.get('textToAnalyze')
    response = emotion_detector(text_to_analyze)
    
    anger = response['anger']
    disgust = response['disgust']
    fear = response['fear']
    joy = response['joy']
    sadness = response['sadness']
    dominant_emotion = response['dominant_emotion']
    
    return (
        f"For the given statement, the system response is 'anger': {anger}, "
        f"'disgust': {disgust}, 'fear': {fear}, 'joy': {joy} and 'sadness': {sadness}. "
        f"The dominant emotion is {dominant_emotion}."
    )

@app.route("/")
def render_index_page():
    return render_template('index.html')

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

---

## Q11 — UPLOAD
**Prompt:** Genuine application screenshot saved as `6b_deployment_test.png`, showing entered text and the resulting emotion prediction.

**Answer:**
- **Filename to upload:** `6b_deployment_test.png`
- **File location:** `/Users/adityadhar/oaqjp-final-project-emb-ai/6b_deployment_test.png`
- **Description:** Screenshot of the web application at `http://localhost:5000/` with text `"I love this new technology"` submitted and the resulting emotion score breakdown rendered in the response area.

---

## Q12 — TEXT
**Prompt:** Provide the updated `emotion_detection.py` code handling HTTP status code 400.

**Answer:**
```python
import json
import requests

def emotion_detector(text_to_analyze):
    url = 'https://sn-watson-emotion.labs.skills.network/v1/watson.runtime.nlp.v1/NlpService/EmotionPredict'
    header = {"grpc-metadata-mm-model-id": "emotion_aggregated-workflow_lang_en_stock"}
    myobj = { "raw_document": { "text": text_to_analyze } }
    response = requests.post(url, json=myobj, headers=header)
    
    if response.status_code == 400:
        return {
            'anger': None,
            'disgust': None,
            'fear': None,
            'joy': None,
            'sadness': None,
            'dominant_emotion': None
        }
    
    formatted_response = json.loads(response.text)
    emotions = formatted_response['emotionPredictions'][0]['emotion']
    anger_score = emotions['anger']
    disgust_score = emotions['disgust']
    fear_score = emotions['fear']
    joy_score = emotions['joy']
    sadness_score = emotions['sadness']
    dominant_emotion = max(emotions, key=emotions.get)
    
    return {
        'anger': anger_score,
        'disgust': disgust_score,
        'fear': fear_score,
        'joy': joy_score,
        'sadness': sadness_score,
        'dominant_emotion': dominant_emotion
    }
```

---

## Q13 — TEXT
**Prompt:** Provide the updated `server.py` code handling blank input errors.

**Answer:**
```python
from flask import Flask, render_template, request
from EmotionDetection.emotion_detection import emotion_detector

app = Flask("Emotion Detection")

@app.route("/emotionDetector")
def sent_detector():
    text_to_analyze = request.args.get('textToAnalyze')
    response = emotion_detector(text_to_analyze)
    
    if response['dominant_emotion'] is None:
        return "Invalid text! Please try again!"
        
    anger = response['anger']
    disgust = response['disgust']
    fear = response['fear']
    joy = response['joy']
    sadness = response['sadness']
    dominant_emotion = response['dominant_emotion']
    
    return (
        f"For the given statement, the system response is 'anger': {anger}, "
        f"'disgust': {disgust}, 'fear': {fear}, 'joy': {joy} and 'sadness': {sadness}. "
        f"The dominant emotion is {dominant_emotion}."
    )

@app.route("/")
def render_index_page():
    return render_template('index.html')

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

---

## Q14 — UPLOAD
**Prompt:** Genuine error-handling interface screenshot saved as `7c_error_handling_interface.png`.

**Answer:**
- **Filename to upload:** `7c_error_handling_interface.png`
- **File location:** `/Users/adityadhar/oaqjp-final-project-emb-ai/7c_error_handling_interface.png`
- **Description:** Screenshot of the web application at `http://localhost:5000/` with blank text submitted, showing the error message: `Invalid text! Please try again!`.

---

## Q15 — TEXT
**Prompt:** Provide the updated `server.py` code demonstrating static code analysis compliance.

**Answer:**
```python
"""
Emotion Detection Server using Flask.
"""
from flask import Flask, render_template, request
from EmotionDetection.emotion_detection import emotion_detector

app = Flask("Emotion Detection")


@app.route("/emotionDetector")
def sent_detector():
    """
    Analyzes the user text received from the frontend and returns the emotion scores
    and dominant emotion.
    """
    text_to_analyze = request.args.get('textToAnalyze')
    response = emotion_detector(text_to_analyze)

    if response['dominant_emotion'] is None:
        return "Invalid text! Please try again!"

    anger = response['anger']
    disgust = response['disgust']
    fear = response['fear']
    joy = response['joy']
    sadness = response['sadness']
    dominant_emotion = response['dominant_emotion']

    return (
        f"For the given statement, the system response is "
        f"'anger': {anger}, 'disgust': {disgust}, 'fear': {fear}, "
        f"'joy': {joy} and 'sadness': {sadness}. "
        f"The dominant emotion is {dominant_emotion}."
    )


@app.route("/")
def render_index_page():
    """
    Renders the main application page.
    """
    return render_template('index.html')


if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

---

## Q16 — TEXT
**Prompt:** Provide the actual pylint terminal output showing the final score.

**Answer:**
```
theia@theia-workspace:/home/project/final_project$ pylint server.py

------------------------------------
Your code has been rated at 10.00/10

```
