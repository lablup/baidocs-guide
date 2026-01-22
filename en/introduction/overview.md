# What is BaiDocs

BaiDocs is a comprehensive documentation platform built on modern web technologies, designed to facilitate the creation and maintenance of technical documentation at scale.

## Architecture Overview

BaiDocs employs a multi-application architecture consisting of:

### Editor Application
A web-based content management interface that provides:
- Real-time markdown editing with live preview
- File system integration with Git version control
- Navigation structure management
- Multi-language content coordination
- Asset management for images and media files

### Viewer Application
A optimized presentation layer featuring:
- Server-side rendering for optimal performance
- Responsive design for multiple device types
- Advanced search capabilities with full-text indexing
- Theme customization and branding options
- PDF export functionality

### Core Framework
The underlying infrastructure includes:
- MDX processing engine for enhanced markdown capabilities
- Path mapping system for virtual directory structures
- Git integration for version control workflows
- Build optimization and static site generation
- Plugin architecture for extensibility

## Technology Stack

BaiDocs is built using:

- **Frontend**: Next.js with React components
- **Styling**: Tailwind CSS with Ant Design components
- **Content Processing**: MDX with remark/rehype plugins
- **Version Control**: Git integration with simple-git library
- **Build System**: Turbo monorepo with optimized caching
- **Deployment**: Static site generation with multiple hosting options

## Use Cases

BaiDocs is designed for:

### Technical Documentation
- Software documentation and API references
- User manuals and operational guides
- Architecture documentation and design specifications

### Knowledge Management
- Internal wikis and knowledge bases
- Process documentation and standard operating procedures
- Training materials and educational content

### Multi-team Collaboration
- Cross-functional team documentation
- Distributed team knowledge sharing
- Version-controlled collaborative editing

## Comparison with Other Solutions

Unlike traditional documentation platforms, BaiDocs provides:

- **Git-native workflow**: Direct integration with existing development processes
- **Monorepo support**: Virtual directory mapping for complex project structures
- **Multi-language coordination**: Synchronized content management across languages
- **Developer-friendly**: Markdown-first approach with programmatic extensibility

BaiDocs differentiates itself from alternatives through its focus on developer experience and enterprise-grade scalability.