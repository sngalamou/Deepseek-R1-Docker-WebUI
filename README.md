# DeepSeek Web UI with Ollama

A containerized web interface for interacting with DeepSeek large language models using Ollama.


## Overview

This project provides a simple yet powerful web interface for using the DeepSeek language models through Ollama, all containerized with Docker for easy deployment and scaling.

### Features

- Clean, intuitive chat interface
- Real-time responses from DeepSeek models
- Support for code highlighting and mathematical notation
- Fully containerized for easy deployment
- Works with any DeepSeek model available through Ollama

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)
- Sufficient system resources (min. 8GB RAM recommended)

## Quick Start

1. Clone this repository:
   ```bash
   git clone https://github.com/yourusername/deepseek-web-app.git
   cd deepseek-web-app
   ```

2. Start the Ollama container and pull the model:
   ```bash
   docker-compose up -d ollama
   docker exec -it ollama ollama pull deepseek-r1:8b
   ```

3. Start the remaining services:
   ```bash
   docker-compose up -d
   ```

4. Access the web interface:
   ```
   http://localhost:8081
   ```

## Project Structure

```
deepseek-web-app/
├── docker-compose.yml
├── backend/
│   ├── Dockerfile
│   ├── package.json
│   └── server.js
├── frontend/
│   ├── Dockerfile
│   ├── index.html
│   └── nginx.conf
└── README.md
```

## Configuration

### Docker Compose

The `docker-compose.yml` file defines three services:

- **ollama**: Runs the Ollama server with the DeepSeek model
- **backend**: Node.js Express server that communicates with Ollama
- **frontend**: Nginx server hosting the web interface

### Backend Configuration

The backend server proxies requests to the Ollama service. You can modify `server.js` to customize:

- Default model (currently set to `deepseek-r1:8b`)
- Temperature and other generation parameters
- API endpoints and functionality

### Frontend Configuration

The web interface is a simple HTML/JS application served by Nginx. Key files:

- `index.html`: The main web interface
- `nginx.conf`: Nginx configuration, including proxy settings for the backend API

## Advanced Usage

### Using Different Models

To use a different DeepSeek model:

1. Pull the model in the Ollama container:
   ```bash
   docker exec -it ollama ollama pull deepseek-coder:7b
   ```

2. Update the model name in your chat requests (the UI allows specifying the model)

### Customizing Parameters

You can adjust model parameters like temperature and max tokens by modifying the options sent in the API requests.

### Persistent Storage

Model data is stored in a Docker volume (`ollama-data`), ensuring that downloaded models persist between restarts.

## Troubleshooting

### Common Issues

1. **"Port already in use" error**:
   - Change the port mapping in `docker-compose.yml` to use an available port

2. **Slow responses**:
   - DeepSeek models are resource-intensive. Consider using a smaller model or deploying on a machine with more resources/GPU support.

3. **Connection errors**:
   - Ensure all three containers are running: `docker-compose ps`
   - Check container logs: `docker-compose logs backend`

### Viewing Logs

To view logs for debugging:

```bash
docker-compose logs ollama    # Ollama server logs
docker-compose logs backend   # Backend API logs
docker-compose logs frontend  # Nginx logs
```

## Deployment Options

### Local Network

To make the application available on your local network:

1. Update the port bindings in `docker-compose.yml` to use `0.0.0.0` (already configured)
2. Access the application from other devices using your host machine's IP address

### Cloud Deployment

This setup can be deployed to any cloud provider that supports Docker:

- AWS EC2/ECS
- Google Cloud Compute Engine
- Azure VMs
- DigitalOcean Droplets

For production deployments, consider:
- Adding HTTPS with Let's Encrypt
- Implementing authentication
- Setting up monitoring and automatic restarts

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Acknowledgments

- [Ollama](https://github.com/ollama/ollama) for providing the model serving infrastructure
- [DeepSeek](https://github.com/deepseek-ai) for the language models
