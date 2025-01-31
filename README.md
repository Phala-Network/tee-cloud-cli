# TEE Cloud CLI

A command-line tool for managing TEE deployments on Phala Network, from local development to cloud deployment.

## Prerequisites

- Docker installed and running
- [Bun](https://bun.sh) installed
- Docker Hub account for publishing images
- [Phala Cloud](https://cloud.phala.network/login) API key

## Installation

```bash
# Install dependencies
bun install

# Build and link the CLI globally
bun run build && bun link
```

## Local Development Workflow

### 1. Start the TEE Simulator

Test your application in a local TEE environment:

```bash
teecloud simulator
```

The simulator will be available at http://localhost:8090.

### 2. Build Your Docker Image

```bash
teecloud build \
  -i, --image <name> \         # Required: Docker image name
  -u, --username <name> \      # Required: Docker Hub username
  -f, --dockerfile <path> \    # Required: Path to Dockerfile
  -t, --tag <tag>             # Required: Tag for the Docker image
```

### 3. Test Locally

Generate a compose file:
```bash
teecloud build-compose \
  -i, --image <name> \         # Required: Docker image name
  -u, --username <name> \      # Required: Docker Hub username
  -t, --tag <tag> \           # Required: Tag for the Docker image
  -e, --env-file <path> \      # Required: Path to environment file
  -v, --version <version>      # Optional: Version of compose template (default: 'basic')
```

Run locally:
```bash
teecloud run-local \
  -c, --compose <path> \       # Required: Path to docker-compose file
  -e, --env-file <path>        # Required: Path to environment file
```

The CLI stores generated compose files in:
```
.tee-cloud/
  └── compose-files/     # Generated docker-compose files
      └── tee-compose.yaml
```

## Cloud Deployment

### 1. Configure API Key

```bash
teecloud set-apikey your-phala-cloud-api-key
```

### 2. Publish Your Image

```bash
teecloud publish \
  -i, --image <name> \         # Required: Docker image name
  -u, --username <name> \      # Required: Docker Hub username
  -t, --tag <tag>             # Required: Tag of the Docker image
```

### 3. Deploy to Cloud

```bash
teecloud deploy \
  -t, --type <type> \         # Optional: TEE vendor type (default: "phala")
  -m, --mode <mode> \         # Optional: Deployment mode (default: "docker-compose")
  -n, --name <name> \         # Required: Name of docker image
  -c, --compose <path> \      # Required: Docker compose file path
  -e, --env <KEY=VALUE...> \  # Optional: Environment variables
  --env-file <path> \         # Optional: Environment file path
  --debug                     # Optional: Enable debug mode
```

List available tags:
```bash
teecloud list-tags \
  -i, --image <name> \        # Required: Docker image name
  -u, --username <name>       # Required: Docker Hub username
```

List TEE pods:
```bash
teecloud teepods \
  [--status <active|inactive>] \           # Optional: filter by status
  [--region <region-name>]                 # Optional: filter by region
```

View deployed images:
```bash
teecloud images \
  --teepod-id <id>           # Required: ID of the teepod
```

Upgrade deployment:
```bash
teecloud upgrade \
  -t, --type <type> \        # Optional: TEE vendor type (default: "phala")
  -m, --mode <mode> \        # Optional: Deployment mode (default: "docker-compose")
  --app-id <id> \            # Required: Application ID
  -c, --compose <path> \     # Required: Docker compose file path
  -e, --env <KEY=VALUE...> \ # Optional: Environment variables
  --env-file <path>          # Optional: Environment file path
```

## Managing Deployments

List your TEE pods:
```bash
teecloud teepods
```

List running CVMs:
```bash
teecloud list-cvms
```

View deployed images:
```bash
teecloud images
```

Upgrade an existing deployment:
```bash
teecloud upgrade \
  --tag v1.0.1 \
  --username your-dockerhub-username
```

## Docker Compose Templates

The CLI provides several templates for different use cases:

- `basic`: Simple template for general applications
  ```bash
  teecloud run-local --template basic
  ```

- `eliza-v2`: Modern template with Bun runtime
  ```bash
  teecloud run-local --template eliza-v2
  ```

- `eliza-v1`: Legacy template with character file support
  ```bash
  teecloud run-local --template eliza-v1
  ```

## Troubleshooting

Common issues and solutions:

1. **Docker Build Fails**
   - Verify Docker daemon is running
   - Check Dockerfile path
   - Ensure proper permissions

2. **Simulator Issues**
   - Check if port 8090 is available
   - Verify Docker permissions

3. **Cloud Deployment Fails**
   - Validate API key
   - Confirm image exists on Docker Hub
   - Check environment variables

For detailed help:
```bash
teecloud --help
teecloud <command> --help
```

## Development

To contribute or run in development mode:
```bash
bun run src/index.ts
```

The project uses:
- Bun for runtime and package management
- TypeScript for type safety
- Commander.js for CLI interface
- Zod for runtime validation

## License

tbd
