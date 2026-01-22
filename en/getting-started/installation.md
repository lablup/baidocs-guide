# Installation

This section provides step-by-step instructions for installing and configuring BaiDocs in your development environment.

## System Requirements

### Hardware Requirements
- **CPU**: 2+ cores recommended for optimal performance
- **Memory**: Minimum 4GB RAM, 8GB recommended for large documentation projects
- **Storage**: 1GB available disk space for installation and dependencies

### Software Dependencies
- **Node.js**: Version 18.17.0 or higher
- **npm**: Version 9.0.0 or higher (included with Node.js)
- **Git**: Version 2.25.0 or higher for version control integration

### Operating System Support
- **macOS**: 11.0 (Big Sur) or later
- **Windows**: Windows 10 version 1903 or later
- **Linux**: Ubuntu 20.04 LTS or equivalent distributions

## Installation Methods

### Method 1: Clone from Repository

1. **Clone the BaiDocs repository**:
   ```bash
   git clone https://github.com/lablup/baidocs.git
   cd baidocs
   ```

2. **Install dependencies**:
   ```bash
   npm install
   ```

3. **Initialize the workspace**:
   ```bash
   npm run setup
   ```

### Method 2: Package Installation (Future Release)

*Note: Package-based installation will be available in future releases.*

```bash
# Future implementation
npm install -g @baidocs/cli
baidocs init my-documentation
```

## Configuration Setup

### Environment Configuration

1. **Create environment file**:
   ```bash
   cp .env.example .env.local
   ```

2. **Configure environment variables**:
   ```env
   # Content directory path
   CONTENT_PATH=./content

   # Enable development mode
   NODE_ENV=development

   # Port configuration
   EDITOR_PORT=3000
   VIEWER_PORT=3001
   ```

### Workspace Initialization

1. **Create content directory**:
   ```bash
   mkdir -p content
   ```

2. **Verify installation**:
   ```bash
   npm run build
   ```

## Development Server Setup

### Starting the Editor Application

```bash
# Start the editor interface
npm run dev:editor
```

The editor interface will be available at `http://localhost:3000`.

### Starting the Viewer Application

```bash
# Start the viewer interface
npm run dev:viewer
```

The viewer interface will be available at `http://localhost:3001`.

### Starting Both Applications

```bash
# Start both applications simultaneously
npm run dev
```

## Verification Steps

### Installation Verification

1. **Check application status**:
   - Navigate to `http://localhost:3000` for the editor
   - Navigate to `http://localhost:3001` for the viewer
   - Verify both applications load without errors

2. **Test content creation**:
   - Create a new book through the editor interface
   - Add sample content to verify functionality
   - Confirm content appears in the viewer

### Troubleshooting Common Issues

#### Port Conflicts
If ports 3000 or 3001 are in use:
```bash
# Use custom ports
EDITOR_PORT=4000 VIEWER_PORT=4001 npm run dev
```

#### Permission Errors
On Unix-like systems:
```bash
# Fix permission issues
sudo chown -R $(whoami) ~/.npm
```

#### Node.js Version Issues
Update Node.js to the required version:
```bash
# Using nvm (recommended)
nvm install 18
nvm use 18
```

## Next Steps

After successful installation:

1. **Create your first book** following the next section
2. **Configure version control** for content management
3. **Customize themes and settings** according to your requirements
4. **Set up deployment pipelines** for production environments

The installation process is now complete. Proceed to the next section to create your first documentation project.