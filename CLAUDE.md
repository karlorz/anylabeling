# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

AnyLabeling is a PyQt5-based image annotation tool with AI-powered auto-labeling capabilities. It combines traditional manual labeling with modern AI models like YOLO and Segment Anything (SAM/SAM2) for efficient data labeling workflows.

## Development Commands

### Environment Setup
```bash
# Create conda environment
conda create -n anylabeling python=3.12
conda activate anylabeling

# For macOS only - install PyQt5 via conda
conda install -c conda-forge pyqt==5.15.9

# Install dependencies
pip install -r requirements-dev.txt
# or for macOS: pip install -r requirements-macos-dev.txt
```

### Running the Application
```bash
# Generate Qt resources (required before running)
pyrcc5 -o anylabeling/resources/resources.py anylabeling/resources/resources.qrc

# Run from source
python anylabeling/app.py

# Or run installed package
anylabeling
```

### Building
```bash
# Build executable
bash scripts/build_executable.sh

# Output will be in dist/ directory
```

### Package Management
```bash
# Build and publish to PyPI
bash scripts/build_and_publish_pypi.sh

# Build macOS folder mode
bash scripts/build_macos_folder.sh
```

## Architecture

### Core Structure
- **anylabeling/app.py**: Main application entry point
- **anylabeling/views/mainwindow.py**: Primary application window
- **anylabeling/views/labeling/**: Core labeling functionality
  - **label_wrapper.py**: Main labeling widget wrapper
  - **widgets/**: UI components (canvas, dialogs, toolbars)
  - **utils/**: Utility functions for I/O, export formats, image processing

### Key Components
- **Auto-labeling Services**: `/services/auto_labeling/` contains AI model integrations
  - SAM/SAM2 models for segmentation
  - YOLO models for object detection
  - Model manager for loading/caching models
- **Export Formats**: Multiple output formats (YOLO, VOC, CreateML, etc.)
- **UI Themes**: Dark/light theme support in `styles/theme.py`
- **Internationalization**: Translation files in `resources/translations/`

### Configuration
- **anylabeling_config.yaml**: Main application configuration
- **models.yaml**: Auto-labeling model configurations
- User config stored in `~/.anylabelingrc`

## Platform-Specific Notes

### macOS
- PyQt5 must be installed via conda, not pip
- Use macOS-specific requirement files
- Special folder mode build available for distribution

### GPU Support
- GPU variants available for non-macOS platforms
- Uses `onnxruntime-gpu` for accelerated inference
- Device preference set in `app_info.py`

## Testing
The codebase includes basic validation utilities in `anylabeling/views/labeling/testing.py` for label file sanity checks, but no comprehensive test suite is currently implemented.

## Code Style Guidelines
- Use PyQt5 for GUI components
- Split code into logical modules
- Prioritize clean, understandable code
- Optimize for performance and memory usage
- Follow existing patterns for new UI components