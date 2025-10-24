FROM python:3.10-slim

WORKDIR /app

# Install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy test code and configs
COPY . .

# Create reports directory
RUN mkdir -p reports

# Set Python path for module discovery
ENV PYTHONPATH=/app

# Default command (can be overridden by docker-compose)
CMD ["pytest"]
