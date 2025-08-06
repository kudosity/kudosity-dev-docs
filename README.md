# Kudosity Developer Documentation

This repository contains the source files for the Kudosity Developer Documentation portal at [developers.kudosity.com](https://developers.kudosity.com). It uses a bi-directional sync with ReadMe.io, allowing documentation to be maintained as markdown files in version control while automatically publishing to the developer portal.

## 🔄 Bi-Directional Sync

This repository is synchronized with ReadMe.io for **documentation content only**, which means:

- **Local to Portal**: Changes made to markdown files in this repository are automatically published to the developer portal when committed to the main branch
- **Portal to Local**: Changes made directly in the ReadMe.io interface are synced back to this repository
- **Version Control**: All documentation changes are tracked in Git, providing full history and collaboration capabilities

**Note**: API specifications (OpenAPI/JSON files) are **not** part of this sync. They are deployed separately from the main /burst codebase through a dedicated deployment system.

## 📁 Repository Structure

```
kudosity-dev-docs/
├── docs/                           # Main documentation content
│   ├── Documentation/              # Getting started guides and core concepts
│   │   ├── getting-started/        # Quick start tutorials
│   │   ├── webhooks/               # Webhook documentation
│   │   └── ...
│   ├── RCS/                        # Rich Communication Services docs
│   ├── WhatsApp/                   # WhatsApp API documentation
│   └── _order.yaml                 # Navigation order configuration
├── reference/                      # API reference documentation
│   ├── API Overview/               # General API information
│   ├── Transmit SMS API/           # SMS API endpoints
│   ├── Transmit Message API/       # Message API endpoints
│   ├── Transmit SMS Fast API/      # Fast API endpoints
│   ├── WhatsApp API/               # WhatsApp API endpoints
│   ├── RCS API/                    # RCS API endpoints
│   ├── *.json                      # OpenAPI specification files
│   └── _order.yaml                 # API reference navigation order
├── custom_blocks/                  # Custom ReadMe blocks
├── custom_pages/                   # Custom page content
└── README.md                       # This file
```

## 📚 Content Types

### Documentation (`docs/`)
Contains guides, tutorials, and conceptual documentation organized by product area:

- **Documentation**: Core getting started guides and fundamental concepts
- **RCS**: Rich Communication Services documentation and onboarding
- **WhatsApp**: WhatsApp Business API guides and registration processes

### API Reference (`reference/`)
Contains detailed API documentation and OpenAPI specifications:

- **API Overview**: Authentication, rate limiting, status codes, and general API concepts
- **Transmit SMS API**: Traditional SMS sending and management endpoints
- **Transmit Message API**: Modern messaging API with MMS and webhook support
- **Transmit SMS Fast API**: High-volume bulk messaging endpoints
- **WhatsApp API**: WhatsApp Business messaging endpoints
- **RCS API**: Rich Communication Services endpoints

### OpenAPI Specifications
API specifications exist as JSON files in the `reference/` directory for reference purposes:

- `transmit-sms-api.json` - Traditional SMS API
- `transmit-message-api.json` - Modern messaging API
- `transmit-sms-fast-api.json` - Fast bulk messaging API
- `whatsapp-api-1.json` - WhatsApp Business API
- `rcs-api.json` - RCS API

**Important**: These files are **not the source of truth** for the API specifications on the developer portal. The actual API specs are deployed from the main /burst codebase through a separate deployment system. Changes to API specifications should be made in the source code, not in these files.

## 🚀 Getting Started

### Prerequisites
- Git access to this repository
- Basic understanding of Markdown syntax
- Familiarity with ReadMe.io (optional, for direct portal editing)

### Making Changes

1. **Clone the repository**:
   ```bash
   git clone https://github.com/burstsms/kudosity-dev-docs.git
   cd kudosity-dev-docs
   ```

2. **Create a new branch**:
   ```bash
   git checkout -b feature/your-documentation-update
   ```

3. **Edit documentation**:
   - Modify existing `.md` files or create new ones
   - Adjust navigation order in `_order.yaml` files if needed
   - **Note**: Do not edit `.json` files for API changes - these should be made in the main /burst codebase

4. **Commit and push changes**:
   ```bash
   git add .
   git commit -m "Update documentation: brief description of changes"
   git push origin feature/your-documentation-update
   ```

5. **Create a pull request** for review

6. **Automatic publishing**: Once merged to main, changes will automatically sync to the developer portal

## 📝 Writing Guidelines

### Markdown Files
- Use standard Markdown syntax
- Include frontmatter with title, excerpt, and metadata
- Follow the existing file naming conventions
- Use relative links for internal references

### Navigation Order
- Update `_order.yaml` files when adding new sections
- Maintain logical grouping and hierarchy
- Test navigation flow after changes

### API Documentation
- Focus on markdown documentation files that sync with the portal
- Include comprehensive examples and descriptions in markdown format
- For API specification changes, work in the main /burst codebase, not this repository

## 🔧 API Endpoints Covered

The documentation covers three main API endpoints:

1. **api.transmitsms.com** - Transactional API with high reliability
2. **api.transmitmessage.com** - High-volume API with MMS and webhook support
3. **sendsms.transmitsms.com** - Fast API optimized for bulk messaging

## 🤝 Contributing

1. Follow the existing documentation structure and style
2. Test your changes locally when possible
3. Ensure all links work correctly
4. Update navigation files when adding new content
5. Write clear, concise commit messages
6. Request reviews for significant changes

## 📞 Support

For questions about:
- **Documentation content**: Create an issue in this repository
- **API specification changes**: Work in the main /burst codebase or contact the development team
- **API functionality**: Contact support at [help.kudosity.com](https://help.kudosity.com/s/submit-ticket)
- **ReadMe.io sync issues**: Contact the development team

## 🔗 Related Links

- [Kudosity Developer Portal](https://developers.kudosity.com)
- [Kudosity Main Website](https://kudosity.com)
- [Support Portal](https://help.kudosity.com)
- [ReadMe.io Documentation](https://docs.readme.com)

---

*This repository is maintained by the Kudosity development team. For technical issues or questions, please create an issue or contact support.*
