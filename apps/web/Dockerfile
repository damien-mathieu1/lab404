FROM dhi.io/python:3.14.8-debian13-dev@sha256:42fb1e613a3fd86e85c30f1b054536ba642c455123d6b4bcbd796aa49de4e46b AS build

WORKDIR /src
RUN python3 -m venv --without-pip /opt/venv
COPY requirements.txt .
RUN python3 -m pip --python /opt/venv/bin/python install --no-cache-dir --require-hashes --only-binary=:all: -r requirements.txt

FROM dhi.io/python:3.14.8-debian13@sha256:aa13af4b3777149bbf18dc6fe392c5ab9eb097d39715d57f4bacde537b6e0e26

ENV PATH=/opt/venv/bin:$PATH \
    PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1
WORKDIR /app
COPY --from=build /opt/venv /opt/venv
COPY app.py ./
COPY templates/ templates/
USER 65532:65532
EXPOSE 8080
CMD ["opentelemetry-instrument", "gunicorn", "--bind=0.0.0.0:8080", "--access-logfile=-", "app:app"]
