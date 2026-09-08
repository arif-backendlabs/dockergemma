# Running Gemma 3 with Docker Model Runner and Spring Boot (GenAI)

This project demonstrates how to run the Gemma 3 model locally using Docker Desktop’s AI Model Runner
and integrate it with a Spring Boot application using Spring AI GenAI (OpenAI-compatible) support.

This setup does NOT use Ollama.

---

## Step 1: Install Docker Desktop

Docker Desktop is required to run AI models locally using Docker’s built-in Model Runner.





Download Docker Desktop from:
https://www.docker.com/products/docker-desktop/

After installation:
- Start Docker Desktop
- Ensure Docker is running successfully

---

## Step 2: Enable Docker AI Model Runner

Docker Desktop provides native support for running AI models locally.

Steps:
1. Open Docker Desktop
2. Go to Settings
3. Navigate to AI settings
4. Enable Docker Model Runner
5. Configure a port for the model runtime

In this setup, the configured port is:
12434

Make sure this port is available on your system.

---

## Step 3: Choose the Gemma 3 Model

Docker Hub provides officially supported AI models.

For this setup, we use:
ai/gemma3

Model details:
- ~3.88 billion parameters
- Model size ~2.3 GB
- Suitable for local development and moderate workloads
- Larger models can be used for production or large-scale workloads depending on hardware capacity

---

## Step 4: Run Gemma 3 Using Docker

You can run the model directly from the command line.

Open Command Prompt / Terminal and run:

docker model run ai/gemma3


Docker will:
- Download the model if not already present
- Start the model runtime
- Expose an OpenAI-compatible API endpoint
- Bind it to the configured port (12434)

The model runtime will be accessible at:
http://localhost:12434/engines

Note:
The model does not continuously run inference.
It loads into memory only when requests are sent.

---

## Step 5: Spring Boot Application Configuration (GenAI)

The Spring Boot application uses Spring AI GenAI (OpenAI-compatible) integration.
This setup treats Docker Model Runner as an OpenAI-compatible backend.

A dummy API key is required only to trigger bean creation.
No real OpenAI calls are made.

### application.properties

spring.ai.openai.chat.options.model=ai/gemma3
spring.ai.openai.api-key=dummy
spring.ai.openai.chat.base-url=http://localhost:12434/engines


Key points:
- The OpenAI client is used only as a protocol adapter
- The base URL redirects all requests to the local Docker model runtime
- The API key is not validated by Docker Model Runner
- All inference happens locally

---

## How It Works Internally

- Docker Model Runner hosts the Gemma 3 model
- It exposes an OpenAI-compatible HTTP API
- Spring AI creates ChatModel and ChatClient beans using GenAI configuration
- Requests are sent to the local Docker endpoint
- Gemma loads into memory on demand and generates responses
- No internet or OpenAI cloud dependency exists

---

## Where the LLM Runs

- The LLM runs locally inside Docker
- CPU or GPU usage depends on system availability
- The model is loaded into memory only when used
- Docker Model Runner listens on port 12434
- The model itself is not “always running”

---

## Summary

- Docker Desktop provides a local OpenAI-compatible LLM runtime
- Gemma 3 runs fully locally via Docker Model Runner
- Spring Boot uses GenAI annotations and OpenAI-compatible configuration
- A dummy API key is required only for bean initialization
- No data leaves the local machine
- No OpenAI costs involved

This setup is ideal for local development, testing, and controlled environments.
