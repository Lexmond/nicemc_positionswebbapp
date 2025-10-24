# 🚀 Quick Start Guide - NICEMC Influenza Positions Web Application

Get up and running with the influenza sequence analysis platform in 5 minutes!

## ⚡ Prerequisites Check

Before starting, verify you have:

```bash
# Check Docker version (20.10+ required)
docker --version

# Check Docker Compose version (v2.0+ required)
docker-compose --version

# Check available disk space (10GB+ recommended)
df -h
```

## 📥 Installation

### Option 1: Using docker-compose.yml directly

```bash
# Create a directory for the application
mkdir ~/influenza-positions
cd ~/influenza-positions

# Download the docker-compose file
curl -o docker-compose.yml https://raw.githubusercontent.com/Lexmond/nicemc_positionswebbapp/main/docker-compose.public.yml

# Start the application
docker-compose up -d
```

### Option 2: Manual docker-compose.yml creation

Create a file named `docker-compose.yml` with this content:

```yaml
version: '3.8'

services:
  snakemake-env:
    image: lexmond/nicemc_positionswebbapp:snakemake-env
    container_name: positions-snakemake
    volumes:
      - workflow_input:/app/input
      - workflow_output:/app/output
      - workflow_triggers:/app/triggers
    working_dir: /app
    command: /app/workflow_watcher.sh
    networks:
      - positions-network
    environment:
      - CORES=4
    restart: unless-stopped

  api:
    image: lexmond/nicemc_positionswebbapp:api
    container_name: positions-api
    ports:
      - "5050:5000"
    volumes:
      - uploads_data:/app/uploads
      - workflow_input:/app/workflow_input
      - workflow_output:/app/output
      - workflow_triggers:/app/triggers
    depends_on:
      - snakemake-env
    networks:
      - positions-network
    restart: unless-stopped

  client:
    image: lexmond/nicemc_positionswebbapp:client
    container_name: positions-client
    ports:
      - "3000:3000"
    depends_on:
      - api
    networks:
      - positions-network
    restart: unless-stopped

volumes:
  uploads_data:
  workflow_input:
  workflow_output:
  workflow_triggers:

networks:
  positions-network:
    driver: bridge
```

Then start:

```bash
docker-compose up -d
```

## ✅ Verify Installation

Check that all containers are running:

```bash
docker-compose ps
```

Expected output (all should show "running"):
```
NAME                    IMAGE                                        STATUS
positions-snakemake     lexmond/nicemc_positionswebbapp:snakemake    Up 30 seconds
positions-api           lexmond/nicemc_positionswebbapp:api          Up 30 seconds
positions-client        lexmond/nicemc_positionswebbapp:client       Up 30 seconds
```

## 🌐 Access the Application

Open your web browser:

- **Web Interface**: http://localhost:3000
- **API Documentation**: http://localhost:5050

## 📤 Your First Analysis

### Step 1: Get Sample Data

Download sample influenza sequences:
- From [NCBI Influenza Virus Resource](https://www.ncbi.nlm.nih.gov/genomes/FLU/)
- Or use sequences from [GISAID](https://www.gisaid.org/) (requires registration)

### Step 2: Upload Files

1. Navigate to http://localhost:3000
2. Click on the H3N2, H1N1pdm, or B-Victoria upload section
3. Select your FASTA file (must be valid FASTA format)
4. (Optional) Upload custom positions Excel file, or use default
5. Click **"Upload Files & Start Analysis"**

### Step 3: Monitor Progress

- Real-time status updates appear automatically
- Workflow logs stream live during execution
- Wait for "Analysis Complete" message

### Step 4: Download Results

1. Click the **"Download Results"** button
2. Extract the ZIP file
3. Open Excel files or view CSV results

## 📊 Understanding Results

The results ZIP contains:

```
results_[timestamp].zip
├── nextclade_h3n2.csv          # Phylogenetic classification
├── aa_translation_h3n2.fasta   # Amino acid sequences
├── positions_h3n2.csv          # Position-specific analysis
├── glycosylation_h3n2.csv      # N-glycosylation sites
├── resistance_h3n2.csv         # Antiviral resistance markers
└── combined_report.xlsx        # All-in-one Excel workbook
```

## 🛠️ Common Commands

```bash
# View logs
docker-compose logs -f

# View snakemake workflow logs specifically
docker-compose logs -f snakemake-env

# Restart all services
docker-compose restart

# Stop the application
docker-compose down

# Stop and clean up everything (fresh start)
docker-compose down -v

# Update to latest images
docker-compose pull
docker-compose up -d
```

## 🔧 Troubleshooting

### Port Already in Use

If ports 3000 or 5050 are occupied, edit `docker-compose.yml`:

```yaml
ports:
  - "8080:3000"  # Change frontend port
  - "8090:5000"  # Change API port
```

### Container Won't Start

```bash
# Check logs for errors
docker-compose logs

# Check specific container
docker logs positions-snakemake
```

### Workflow Not Processing

```bash
# Verify workflow_watcher is running
docker exec positions-snakemake ps aux | grep workflow_watcher

# Check trigger directory
docker exec positions-snakemake ls -la /app/triggers
```

### Out of Memory

Increase Docker Desktop memory:
1. Docker Desktop → Settings/Preferences
2. Resources → Memory
3. Set to at least 4GB
4. Apply & Restart

## 📖 Next Steps

- Read the full [README.public.md](README.public.md)
- Explore different influenza subtypes
- Customize positions of interest
- Batch process multiple files
- Integrate into your workflow pipeline

## 🆘 Getting Help

- Check Docker logs: `docker-compose logs`
- Verify file formats (FASTA must be valid)
- Ensure adequate system resources
- Review the troubleshooting section

## 🎯 Success Indicators

✅ All three containers running  
✅ Web interface loads at http://localhost:3000  
✅ Can upload files without errors  
✅ Workflow completes and produces results  
✅ Can download and open result files  

---

**Ready to analyze influenza sequences!** 🧬

For advanced usage and API integration, see the full documentation in README.public.md.
