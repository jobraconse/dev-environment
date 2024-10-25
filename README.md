## Dockerfile
```
FROM python:3.12-slim

WORKDIR /code

COPY ./requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

COPY ./src ./src

CMD [ "uvicorn", "src.main:app", "--host", "0.0.0.0", "--port", "80", "--reload" ]
```
## docker-compose.yml
```
services:
  app:
    build: .
    container_name: fastapi-container
    command: uvicorn src.main:app --host 0.0.0.0 --port 80 --reload
    ports:
      - 80:80
    volumes:
      - .:/code
    restart:
      - "unless-stopped"
    depends_on:
      - redis

  redis:
    image: redis:alpine
```

## main.py
```
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def read_root():
    return {"Hello": "World! in dev container"}
```
## Directory structure
```
[serge@almaPC fastapi]$ tree .
.
├── docker-compose.yml
├── Dockerfile
├── README.md
├── requirements.txt
└── src
    ├── main.py
    └── __pycache__
        └── main.cpython-312.pyc

2 directories, 6 files
[serge@almaPC fastapi]$ 
```