FROM python:3.6-alpine3.10

WORKDIR /app

COPY requirements.txt /app
RUN pip install -r requirements.txt

COPY app.py /app

EXPOSE 5000
CMD apt-get install nano
CMD python app.py

