# Dockerfile

A Dockerfile is a text document that contains all the commands/instructions needed to assemble a Docker image. It serves as a blueprint for building container images automatically by following the instructions you define.

## Key Characteristics:
- Plain text file named Dockerfile (no extension)
- Contains sequential commands that Docker executes to create an image
- Follows a specific syntax with instruction + arguments format

| Instruction    | Purpose                                                                 |
|---------------|-------------------------------------------------------------------------|
| `FROM`        | Base image to start from (e.g., `FROM ubuntu:24.04`)                   |
| `RUN`         | Executes commands during build (e.g., `RUN apt-get update`)             |
| `COPY`        | Copies files from host to container                                     |
| `ADD`         | Enhanced copy that can handle URLs and auto-extract archives            |
| `WORKDIR`     | Sets working directory for subsequent instructions                      |
| `ENV`         | Sets environment variables                                              |
| `EXPOSE`      | Documents which ports the container will listen on                      |
| `CMD`         | Default command to run when container starts                           |
| `ENTRYPOINT`  | Configures the container to run as an executable                        |
| `USER`        | Sets which user runs the commands                                       |
| `VOLUME`      | Creates mount points for external volumes                               |
| `ARG`         | Defines build-time variables                                            |
| `LABEL`       | Adds metadata to the image                                              |
| `HEALTHCHECK` | Defines how to test container health                                    |
| `ONBUILD`     | Adds trigger instructions for later builds                              |
---

## Technical Explanation:
    "We package the application and its dependencies inside a container, creating a portable, self-contained unit that runs consistently across environments."

1 Sample Python Application Structure
```
myapp/
├── app/
│   ├── __init__.py
│   ├── main.py
│   └── requirements.txt
├── Dockerfile
└── .dockerignore
```

2 python files `app/main.py`
```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello from Containerized Python App!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

3 app/requirement.txt
```
flask==3.0.0
gunicorn==21.2.0
```

4 Create Dockerfile
```
echo << EOF > Dockerfile
# Base image
FROM python:3.12-slim

# Set environment variables
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

# Set work directory
WORKDIR /app

# Install dependencies
COPY app/requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY app .

# Expose port
EXPOSE 5000

# Runtime command (using Gunicorn as WSGI server)
CMD ["gunicorn", "--bind", "0.0.0.0:5000", "main:app"]
EOF
```

5 .dockerignore
```
__pycache__
*.pyc
*.pyo
*.pyd
venv
.env
```

### How to Use a Dockerfile:

Build an image:

```bash
# Build the image
docker build -t python-app .

# Run the container
docker run -d -p 5000:5000 --name my-python-app python-app
```


