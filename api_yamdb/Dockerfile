FROM python:3.7-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --upgrade pip && pip3 install -r ./requirements.txt --no-cache-dir

COPY ./ /app

ENV DJANGO_SECRET_KEY 123

RUN chmod +x /app/entrypoint.sh

ENTRYPOINT ["/app/entrypoint.sh"]

CMD ["gunicorn", "api_yamdb.wsgi:application", "--bind", "0:8000"]
