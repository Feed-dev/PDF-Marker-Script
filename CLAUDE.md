# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Common Commands

### Environment Setup
```bash
# Activate virtual environment (required for all operations)
source marker_env/bin/activate

# Install dependencies
pip install -r requirements.txt

# Install PyTorch with CUDA support
pip3 install --pre torch torchvision torchaudio --index-url https://download.pytorch.org/whl/nightly/cu128

# Install Marker PDF converter
pip install marker-pdf

# Setup Ollama and granite3.2-vision model
curl -fsSL https://ollama.com/install.sh | sh
ollama pull granite3.2-vision:2b
```

### Running the Converter
```bash
# Basic PDF conversion
python pdf_to_markdown.py input/document.pdf -o output/document.md

# LLM-enhanced conversion (requires Ollama running)
python pdf_to_markdown.py input/document.pdf --use-llm -o output/document.md

# Batch processing
python pdf_to_markdown.py input/ --batch -o output/

# Check system status
python pdf_to_markdown.py --info

# Run example demonstrations
python example_usage.py

# Run tests
python test_conversion.py
```

### Ollama Service Management
```bash
# Start Ollama service (required for LLM enhancement)
ollama serve

# Check available models
ollama list

# Test API connection
curl -s http://localhost:11434/api/tags
```

## Architecture

### Core Components

**PDFToMarkdownConverter Class**: Main conversion engine that orchestrates the entire process. Handles both single file and batch operations, manages GPU/CPU selection, and integrates with Ollama for LLM enhancement.

**OllamaClient Class**: Simple client for checking Ollama service availability. LLM enhancement is handled natively by Marker's built-in Ollama integration.

### Conversion Pipeline

1. **Input Validation**: Checks file existence and format
2. **Marker Processing**: Uses `marker_single` command to convert PDF to markdown
   - Creates subdirectory named after PDF file in output directory
   - Generates .md file, _meta.json, and extracted images (.jpeg files)
   - Optional LLM enhancement via Marker's native Ollama integration (--llm_service=marker.services.ollama.OllamaService)
3. **Output Generation**: Files remain in subdirectories with images and metadata for better RAG system integration

### Key Integration Points

- **Marker Library**: External PDF processing via subprocess calls to `marker_single`
- **Ollama Integration**: Native Marker integration with Ollama service for LLM enhancement
- **PyTorch**: GPU acceleration for the underlying ML models
- **CUDA**: Hardware acceleration when available

### Configuration

The converter automatically detects:
- CUDA availability for GPU acceleration
- Ollama service status for LLM enhancement
- Available system resources

### Error Handling

Comprehensive logging to `conversion.log` with fallback mechanisms:
- Ollama unavailability falls back to basic conversion without LLM enhancement
- GPU unavailability falls back to CPU processing
- Individual file failures in batch mode don't stop the entire process
- Large/complex PDFs may require extended timeouts (default: 3600 seconds)

### Important Implementation Details

**Marker Output Structure**: The `marker_single` command creates a subdirectory named after the input PDF file within the specified output directory. This subdirectory contains:
- The main .md file
- A _meta.json file with conversion metadata
- Extracted images as .jpeg files (named _page_X_Picture_Y.jpeg)

**File Path Handling**: The script looks for generated markdown files first in the subdirectory created by Marker, then falls back to searching the parent output directory to handle different Marker behavior patterns. Files are kept in their subdirectories to preserve the relationship between markdown, images, and metadata.

**LLM Integration**: Uses Marker's native Ollama support via command-line flags:
- `--llm_service=marker.services.ollama.OllamaService`
- `--ollama_base_url=http://localhost:11434`
- `--ollama_model=granite3.2-vision:2b`

### Dependencies

- **marker-pdf**: Core PDF to markdown conversion
- **torch/torchvision/torchaudio**: ML model acceleration
- **requests**: HTTP communication for Ollama availability checking
- **Ollama + granite3.2-vision:2b**: LLM enhancement model (integrated natively by Marker)

The project requires WSL environment and Python 3.10+ for optimal performance.