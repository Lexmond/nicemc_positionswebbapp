# Influenza Sequence Analysis Web Application

A web-based interface for running Snakemake influenza sequence analysis workflows. This application provides an easy-to-use platform for uploading sequence files, running bioinformatics analysis, and downloading results.

## Features

- **File Upload Interface**: Upload H1N1pdm, H3N2, B-Victoria sequence files and position reference data
- **Automated Workflow Execution**: Runs Snakemake workflows with Nextclade for phylogenetic analysis
- **Real-time Monitoring**: WebSocket-based status updates and log streaming
- **Results Download**: Packaged results in ZIP format with comprehensive analysis outputs
- **Containerized Environment**: Docker-based deployment with all dependencies included

## Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   React Client  │    │  Node.js API    │    │ Snakemake Env   │
│   (Port 3000)   │───▶│   (Port 5000)   │───▶│   (Conda)       │
│                 │    │                 │    │                 │
│ - File Upload   │    │ - File Handling │    │ - Nextclade     │
│ - Status UI     │    │ - Workflow Mgmt │    │ - Python Scripts│
│ - Results DL    │    │ - WebSocket     │    │ - Analysis Tools│
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

## Prerequisites

- Docker and Docker Compose
- At least 4GB RAM available for containers
- 10GB+ disk space for analysis data

## Quick Start

1. **Clone and navigate to the project:**
   ```bash
   cd /path/to/Positions_WebApp
   ```

2. **Copy your Snakemake workflow:**
   ```bash
   # Copy your existing Snakefile and scripts to:
   cp -r /path/to/your/snakemake_workflow/* ./snakemake_workflow/
   ```

3. **Build and start the application:**
   ```bash
   docker-compose up --build
   ```

4. **Access the application:**
   - Web interface: http://localhost:3000
   - API: http://localhost:5000

## Usage

### File Upload

1. Navigate to http://localhost:3000
2. Upload your files in the designated fields:
   - **H1N1pdm sequences**: FASTA file with H1N1pdm sequences
   - **H3N2 sequences**: FASTA file with H3N2 sequences  
   - **B-Victoria sequences**: FASTA file with B-Victoria sequences
   - **Positions reference**: Excel file with amino acid positions of interest

3. Click "Upload Files & Start Analysis"

### Monitoring Progress

- Real-time status updates appear automatically
- Workflow logs are streamed live during execution
- Progress indicators show current workflow stage

### Downloading Results

- When analysis completes, a download button appears
- Results are packaged as a ZIP file containing:
  - Nextclade phylogenetic classification (CSV)
  - Amino acid translations (FASTA)
  - Position analysis results (CSV)
  - Glycosylation site analysis (CSV)
  - Antiviral resistance positions (CSV)
  - Combined Excel reports

## File Requirements

| File Type | Expected Name | Format | Required |
|-----------|---------------|---------|----------|
| H1N1pdm sequences | `sequences_h1n1pdm_ha.fasta` | FASTA | Optional* |
| H3N2 sequences | `sequences_h3n2_ha.fasta` | FASTA | Optional* |
| B-Victoria sequences | `sequences_vic_ha.fasta` | FASTA | Optional* |
| Positions reference | `aa_positions_of_interest.xlsx` | Excel | **Required** |

*At least one sequence file must be provided

## Development

### Project Structure

```
Positions_WebApp/
├── client/                 # React frontend
│   ├── src/
│   │   ├── components/     # UI components
│   │   ├── services/       # API client
│   │   └── types.ts        # TypeScript definitions
│   ├── Dockerfile
│   └── package.json
├── server/                 # Node.js backend
│   ├── src/
│   │   ├── routes/         # API routes
│   │   └── server.ts       # Main server
│   ├── Dockerfile
│   └── package.json
├── snakemake/             # Snakemake environment
│   ├── environment.yml     # Conda dependencies
│   ├── Dockerfile
│   └── run_snakemake.sh   # Execution script
├── docker-compose.yml     # Multi-service setup
└── README.md
```

### Local Development

1. **Install dependencies:**
   ```bash
   npm install
   cd client && npm install
   cd ../server && npm install
   ```

2. **Start development servers:**
   ```bash
   npm run dev
   ```

3. **Build for production:**
   ```bash
   npm run build
   ```

### Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `PORT` | 5000 | Backend API port |
| `CLIENT_URL` | http://localhost:3000 | Frontend URL for CORS |
| `REACT_APP_API_URL` | /api | API base URL |
| `REACT_APP_WS_URL` | http://localhost:5000 | WebSocket URL |

## Deployment

### Docker Compose (Recommended)

```bash
docker-compose up -d
```

### Manual Container Build

```bash
# Build all images
docker-compose build

# Start services
docker-compose up
```

## Troubleshooting

### Common Issues

1. **Out of Memory**: Increase Docker memory allocation to 4GB+
2. **Port Conflicts**: Change ports in docker-compose.yml if 3000/5000 are taken
3. **File Upload Fails**: Check file formats and size limits (100MB max)
4. **Workflow Errors**: Check container logs with `docker-compose logs`

### Logs

```bash
# View all logs
docker-compose logs

# View specific service logs
docker-compose logs api
docker-compose logs client
docker-compose logs snakemake-env
```

### Container Management

```bash
# Stop all services
docker-compose down

# Rebuild specific service
docker-compose build api

# Clean up volumes (removes uploaded data)
docker-compose down -v
```

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/upload` | Upload files for analysis |
| POST | `/api/workflow/start/:uploadId` | Start workflow execution |
| GET | `/api/workflow/status/:uploadId` | Get workflow status |
| GET | `/api/workflow/download/:uploadId` | Download results |

## License

This project is part of the influenza sequence analysis pipeline for the National Influenza Centre (NIC).

## Support

For issues or questions, please contact the bioinformatics team.

