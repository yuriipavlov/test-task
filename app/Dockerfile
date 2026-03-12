FROM python:3.14.3-slim-bookworm

RUN useradd -u 101 -m -s /bin/sh appuser

WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

RUN mkdir -p /app/data && chown -R 101:101 /app
COPY --chown=101:101 app.py .

USER 101
EXPOSE 8080
CMD ["python", "/app/app.py"]
