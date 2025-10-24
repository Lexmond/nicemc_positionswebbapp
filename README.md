# 🧬 NICEMC Influenza Positions Web Application

A web-based platform for automated influenza hemagglutinin (HA) sequence analysis using Nextclade phylogenetic classification and amino acid position tracking.

## 🌟 Features

- **Automated Phylogenetic Analysis**: Nextclade-based classification for H1N1pdm, H3N2, and B-Victoria
- **Position Analysis**: Track amino acid positions of interest for vaccine strain monitoring
- **Glycosylation Site Detection**: Automated identification of N-linked glycosylation sites
- **Antiviral Resistance Markers**: Detection of known resistance-associated positions
- **Web Interface**: User-friendly upload and download interface
- **Real-time Monitoring**: Live workflow status updates
- **Comprehensive Reports**: Excel and CSV output formats

## 📋 Prerequisites

- **Docker Desktop** (20.10+)
- **Docker Compose** (v2.0+)
- At least **4GB RAM** available for containers
- **10GB+ disk space** for analysis data and Nextclade datasets

## 🚀 Quick Start

### 1. Clone or Download

```bash
# Create a directory for the application
mkdir influenza-positions-app
cd influenza-positions-app

# Download the docker-compose file
curl -o docker-compose.yml https://raw.githubusercontent.com/yourusername/repo/main/docker-compose.public.yml
```

### 2. Start the Application

```bash
# Pull images and start all services
docker-compose up -d

# Check status
docker-compose ps
```

### 3. Access the Application

- **Web Interface**: http://localhost:3000
- **API**: http://localhost:5000

### 4. Upload Files

1. Navigate to http://localhost:3000
2. Upload your FASTA files:
   - **H1N1pdm sequences** (optional)
   - **H3N2 sequences** (optional)
   - **B-Victoria sequences** (optional)
   - **Positions reference** Excel file (optional - default provided)
3. Click **"Upload Files & Start Analysis"**
4. Monitor progress in real-time
5. Download results when complete

## 📁 Input File Requirements

### Sequence Files (FASTA)

- **Format**: Standard FASTA format
- **Header**: Must include strain name
- **Gene**: Hemagglutinin (HA) nucleotide sequences
- **Example**:
  ```
  >A/Netherlands/12345/2024
  ATGAAGACTATCATTGCTTTGAGCTAC...
  ```

### Positions Reference (Excel)

- **Optional**: Default file included if not provided
- **Format**: `.xlsx` with specific column structure
- **Download template**: Available in web interface

## 📊 Output Files

Results are packaged as a ZIP file containing:

- **Nextclade results**: CSV with phylogenetic classification and mutations
- **Amino acid translations**: FASTA format
- **Position analysis**: CSV with position-specific data
- **Glycosylation sites**: CSV with N-linked glycosylation predictions
- **Antiviral resistance**: CSV with resistance marker analysis
- **Combined reports**: Excel workbook with all results

## 🛠️ Configuration

### Environment Variables

You can customize the application by editing the `docker-compose.yml`:

```yaml
environment:
  - CORES=4              # CPU cores for Snakemake
  - INPUT_DIR=/app/input
  - OUTPUT_DIR=/app/output
```

### Port Configuration

Change ports if 3000 or 5000 are already in use:

```yaml
ports:
  - "8080:3000"  # Frontend
  - "8090:5000"  # Backend API
```

## 🔧 Management Commands

```bash
# View logs
docker-compose logs -f

# View specific service logs
docker-compose logs -f snakemake-env

# Stop all services
docker-compose down

# Stop and remove volumes (clean slate)
docker-compose down -v

# Restart services
docker-compose restart

# Update to latest images
docker-compose pull
docker-compose up -d
```

## 🧪 Testing

### Sample Data

Download sample influenza sequences from:
- [GISAID](https://www.gisaid.org/) (registration required)
- [NCBI Influenza Virus Resource](https://www.ncbi.nlm.nih.gov/genomes/FLU/)

### Quick Test

1. Upload a small FASTA file (5-10 sequences)
2. Use default positions file
3. Verify workflow completes
4. Download and inspect results

## 🔍 Troubleshooting

### Common Issues

**1. Port Already in Use**
```bash
# Change ports in docker-compose.yml
ports:
  - "8080:3000"  # Use different port
```

**2. Out of Memory**
```bash
# Increase Docker Desktop memory allocation
# Docker Desktop → Preferences → Resources → Memory (4GB minimum)
```

**3. Workflow Not Starting**
```bash
# Check snakemake container logs
docker-compose logs snakemake-env

# Ensure workflow_watcher is running
docker exec positions-snakemake ps aux | grep workflow_watcher
```

**4. Upload Fails**
```bash
# Check file size (max 100MB per file)
# Verify FASTA format is correct
# Check API logs
docker-compose logs api
```

### Debug Mode

Enable detailed logging:

```bash
# View all container logs in real-time
docker-compose logs -f

# Access container shell
docker exec -it positions-snakemake /bin/bash
```

## 📖 Architecture

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

## 🔬 Technology Stack

- **Frontend**: React 18 + TypeScript + Material-UI
- **Backend**: Node.js + Express + Socket.IO
- **Workflow**: Snakemake + Conda
- **Analysis**: Nextclade CLI + Python + BioPython
- **Containerization**: Docker + Docker Compose

## 📚 Included Tools

- **Nextclade**: Phylogenetic classification and mutation analysis
- **BioPython**: Sequence manipulation and translation
- **Pandas**: Data processing and analysis
- **openpyxl**: Excel file handling

## 🗂️ Nextclade Datasets

Pre-installed datasets for:
- **H1N1pdm**: HA, NA, PB2, PB1, PA, NP, MP, NS
- **H3N2**: HA, NA, PB2, PB1, PA, NP, MP, NS
- **B/Victoria**: HA, NA, PB2, PB1, PA, NP, MP, NS
- **B/Yamagata**: HA
- **H5 (Avian)**: Multiple HA clades

Datasets are automatically updated in new releases.

## 🔐 Data Privacy

- All data processing occurs **locally** in Docker containers
- No data is sent to external servers
- Uploaded files are stored only in Docker volumes
- Results are deleted when volumes are removed

## 🆘 Support

For issues, questions, or contributions:
- Open an issue on GitHub (if public repository available)
- Contact: [your-email@domain.com]

## 📄 License

[Specify your license - e.g., MIT, GPL-3.0, etc.]

## 🙏 Acknowledgments

- **Nextclade**: Nextstrain team for phylogenetic analysis tools
- **GISAID**: For influenza sequence data standards
- **WHO**: For influenza surveillance guidelines

## 📈 Version History

### Latest Release
- Multi-platform support (AMD64 + ARM64)
- Updated Nextclade datasets
- Enhanced error handling
- Improved UI/UX

---

**Developed by**: National Influenza Centre (NIC)  
**Maintained by**: [Your Name/Organization]  
**Docker Hub**: https://hub.docker.com/r/lexmond/nicemc_positionswebbapp
