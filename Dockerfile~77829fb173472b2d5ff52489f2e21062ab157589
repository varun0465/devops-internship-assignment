# syntax=docker/dockerfile:1
FROM python:3.11-slim

# set working directory
WORKDIR /app

# copy requirements and install
COPY app/requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# copy app source
COPY app/ ./

# expose port
EXPOSE 5000

# run
CMD ["python", "app.py"]
