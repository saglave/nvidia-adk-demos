# Multi-Agent NAT (NVIDIA Agent Toolkit) Project

This project contains two implementations of NAT (NVIDIA Agent Toolkit) agents that demonstrate weather and time functionality:

## Project Structure

- `src/nat_adk_individual_wrap/` - Individual wrapper implementation
- `src/nat_adk_single_wrap/` - Single wrapper implementation
- `.vscode/` - VS Code launch configurations

## Setup

1. **Environment Setup**
   ```bash
   python -m venv .venv
   source .venv/bin/activate  # On Windows: .venv\Scripts\activate
   ```

2. **Install Dependencies**
   ```bash
   pip install nvidia-nat  # Or your preferred NAT installation method
   ```

3. **Configuration**
   - Copy `.env.template` files to `.env` in each implementation directory:
     ```bash
     cp src/nat_adk_individual_wrap/.env.template src/nat_adk_individual_wrap/.env
     cp src/nat_adk_single_wrap/.env.template src/nat_adk_single_wrap/.env
     ```
   - Update the `.env` files with your OpenAI API credentials and endpoints

## Usage

### Individual Wrap Implementation
```bash
nat run --config_file src/nat_adk_individual_wrap/configs/config.yml --input "What is the time in New York?"
```

### Single Wrap Implementation
```bash
nat run --config_file src/nat_adk_single_wrap/configs/config.yml --input "What is the time in New York?"
```

## Features

Both implementations include:
- Weather update tool
- City time tool
- Sub-agent for time queries
- OpenAI LLM integration
- Phoenix tracing support

## Development

The project includes VS Code launch configurations for debugging. The configurations are set up to run both implementations with sample queries.

## License

Licensed under the Apache License, Version 2.0. See the configuration files for full license text.