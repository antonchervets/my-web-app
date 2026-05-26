FROM python:3.14.3-slim

WORKDIR /app

RUN pip install --no-cache-dir flask

COPY helloworld.py .

EXPOSE 8080

CMD ["python", "helloworld.py"]

