# FaceDB - A Face Recognition Database

<p align="center">
  <img src="https://readme-typing-svg.herokuapp.com?font=Fira+Code&size=30&pause=1000&color=00F5D4&center=true&vCenter=true&width=800&lines=FaceDB+-+Your+Smart+Face+Database!;Built+with+DeepFace+%2B+face_recognition!;Simple%2C+Fast%2C+and+Flexible!" alt="Typing SVG"/>
</p>

---

## 👨‍💻 Key Features

- 🔍 **Face Recognition** using [DeepFace](https://github.com/serengil/deepface) and [face_recognition](https://github.com/ageitgey/face_recognition).
- 💡 **Smart Querying** with flexible search operators.
- ✨ **Fast Vector Search** using [Pinecone](https://www.pinecone.io/) backend.
- 🎉 **Simple API** to add, update, search, delete, and manage face data.
- 📊 **Data Visualization** with Pandas & Matplotlib.

---

## 🔗 Links

- [PyPI Package 🔗](https://pypi.org/project/facedb/)
- [GitHub Repository 🔗](https://github.com/shhossain/facedb)

---

## 💪 Installation

```bash
pip install facedb
```

For recognition engines:

```bash
pip install face_recognition
```
or
```bash
pip install deepface
```

For Pinecone (Optional backend):

```bash
pip install pinecone
```

---

## 🚀 Simple Usage

```python
from facedb import FaceDB

db = FaceDB(path="facedata")

face_id = db.add("John Doe", img="john_doe.jpg")

result = db.recognize(img="new_face.jpg")

if result and result["id"] == face_id:
    print("Recognized as John Doe")
else:
    print("Unknown face")
```

---

## 🌐 Advanced Usage

```python
import os
from glob import glob
from pathlib import Path
from facedb import FaceDB

os.environ["PINECONE_API_KEY"] = "YOUR_API_KEY"

db = FaceDB(
    path="facedata",
    metric='euclidean',
    database_backend='pinecone',
    index_name='faces',
    embedding_dim=128,
    module='face_recognition',
)

files = glob("faces/*.jpg")
imgs, names = [], []
for file in files:
    imgs.append(file)
    names.append(Path(file).stem)

ids, failed_indexes = db.add_many(imgs=imgs, names=names)

result = db.recognize(img="unknown_face.jpg", include=["name"])
if result:
    print(f"Recognized as {result['name']}")
else:
    print("Unknown face")

# Visualize all faces
db.all(include='name').show_img()
```

---

## 🔢 Query Examples

```python
db.add("Nelson Mandela", img="mandela.jpg", profession="Politician", country="South Africa")
db.add("Barack Obama", img="obama.jpg", profession="Politician", country="USA")
db.add("Einstein", img="einstein.jpg", profession="Scientist", country="Germany")

# Simple query
results = db.query(name="Nelson Mandela")

# Complex query
results = db.query(
    where={
        "profession": {"$eq": "Politician"},
        "country": {"$in": ["USA", "South Africa"]}
    }
)

results.show_img()
```

---

## 📢 Supported Query Operators

| Operator | Meaning                |
|:---------|:------------------------|
| `$eq`    | Equal to                 |
| `$ne`    | Not equal to             |
| `$gt`    | Greater than             |
| `$lt`    | Less than                |
| `$in`    | In array                 |
| `$regex` | Regular expression match |

---

## 📊 Visualization

```python
results = db.all(include=['name', 'img'])
results.show_img()  # matplotlib needed
```

---

<p align="center">
  <b>© 2025 Charan Reddy - All rights reserved.</b>
</p>
