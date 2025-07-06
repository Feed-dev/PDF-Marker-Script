# PDF to Markdown Converter with Marker and Ollama

A powerful Python script that converts PDF files to Markdown format using the Marker library with optional LLM enhancement via Marker's built-in Ollama integration.

## Features

- **High-accuracy PDF conversion** using Marker
- **GPU acceleration** with CUDA support
- **LLM enhancement** via Marker's native Ollama integration for improved formatting and OCR error correction
- **Single file and batch processing**
- **Comprehensive logging** and error handling
- **Configurable output formats**

## Installation

### Prerequisites

- Python 3.10+
- WSL (Windows Subsystem for Linux) recommended
- NVIDIA GPU with CUDA support (optional but recommended)

### Setup

1. **Create and activate virtual environment:**
   ```bash
   python3 -m venv marker_env
   source marker_env/bin/activate
   ```

2. **Install PyTorch with CUDA support:**
   ```bash
   pip3 install --pre torch torchvision torchaudio --index-url https://download.pytorch.org/whl/nightly/cu128
   ```

3. **Install Marker PDF converter:**
   ```bash
   pip install marker-pdf
   ```

4. **Install Ollama and granite3.2-vision:2b model:**
   ```bash
   curl -fsSL https://ollama.com/install.sh | sh
   ollama pull granite3.2-vision:2b
   ```

5. **Install additional dependencies:**
   ```bash
   pip install requests
   ```

## Usage

### Command Line Interface

#### Basic single PDF conversion:
```bash
python pdf_to_markdown.py input/document.pdf -o output/document.md
```

#### LLM-enhanced conversion:
```bash
python pdf_to_markdown.py input/document.pdf --use-llm -o output/document.md
```

#### Batch processing:
```bash
python pdf_to_markdown.py input/ --batch -o output/
```

#### Get setup information:
```bash
python pdf_to_markdown.py --info
```

### Command Line Options

- `input`: Input PDF file or directory
- `-o, --output`: Output file or directory
- `--use-llm`: Enable Marker's native Ollama LLM enhancement
- `--no-gpu`: Disable GPU acceleration
- `--ollama-url`: Ollama API URL (default: http://localhost:11434)
- `--ollama-model`: Ollama model name (default: granite3.2-vision:2b)
- `--batch`: Process directory in batch mode
- `--info`: Show conversion setup information
- `--verbose, -v`: Enable verbose logging


## Project Structure

```
PDF-Marker-Script/
├── marker_env/              # Virtual environment
├── input/                   # Input PDF files
├── output/                  # Output markdown files
├── pdf_to_markdown.py      # Main conversion script
├── example_usage.py        # Usage examples
├── requirements.txt        # Python dependencies
├── conversion.log          # Conversion logs
└── README.md              # This file
```

## Configuration

### Ollama Configuration

The script uses Marker's native Ollama integration with the granite3.2-vision:2b model for LLM enhancement. Make sure Ollama is running:

```bash
ollama serve
```

You can configure different Ollama models:
```bash
python pdf_to_markdown.py input.pdf --use-llm --ollama-model=llama2:7b
```

### GPU Configuration

The script automatically detects CUDA availability. To disable GPU acceleration:

```bash
python pdf_to_markdown.py input.pdf --no-gpu
```

## Logging

The script creates detailed logs in `conversion.log` with information about:
- Conversion progress
- Error messages
- Performance metrics
- Marker's LLM enhancement details

## Troubleshooting

### Common Issues

1. **PyTorch CUDA not available:**
   - Ensure NVIDIA drivers are installed
   - Check CUDA installation
   - Verify GPU compatibility

2. **Ollama service not available:**
   - Start Ollama service: `ollama serve`
   - Check if granite3.2-vision:2b model is pulled: `ollama list`
   - Verify Ollama URL configuration

3. **Marker installation issues:**
   - Ensure Python 3.10+ is used
   - Try installing in a fresh virtual environment
   - Check system dependencies

### Performance Tips

- Use GPU acceleration for faster processing
- Enable LLM enhancement with `--use-llm` for better accuracy on complex documents
- Use batch processing for multiple files
- Monitor GPU memory usage for large documents
- Markdown files with images and metadata are kept in subdirectories for better organization

## License

This project uses the Marker library which has specific commercial usage limitations. Please refer to the Marker project license for details.

## Contributing

Feel free to submit issues and enhancement requests!