FROM python:3.9-slim

RUN useradd -u 101 -m -s /bin/sh appuser

WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY app.py .
RUN mkdir -p /app/data && chown -R 101:101 /app

USER 101
EXPOSE 8080
CMD ["python", "/app/app.py"]
