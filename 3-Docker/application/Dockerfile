FROM python:3.8.14-slim

RUN mkdir -p /opt/app
WORKDIR /opt/app

COPY . .

RUN pip install -r requirements.txt

CMD flask run --host 0.0.0.0 --port 5400