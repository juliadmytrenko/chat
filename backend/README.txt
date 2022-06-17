Aby uruchomić chatbota w oknie konsoli 

$ python3 -m venv venv
$ . venv/Scripts/activate

$ (venv) pip install Flask torch torchvision nltk flask-cors

$ (venv) python
>>> import nltk
>>> nltk.download('punkt')

$ (venv) python train.py

$ (venv) python chat.py

aby uruchomić serwer:
$ (venv) python app.py