# Contributing to Carbon Track

Thank you for your interest in contributing to Carbon Track! This document provides guidelines and information for contributors.

## 🤝 Code of Conduct

We are committed to providing a welcoming and inclusive environment. Please:
- Be respectful and considerate
- Use inclusive language
- Focus on constructive feedback
- Help create a positive community

## 🚀 Getting Started

### Prerequisites
- Node.js 18+
- pnpm package manager
- Git
- MongoDB (local or remote)
- MetaMask wallet

### Setup Development Environment

1. **Fork the Repository**
   ```bash
   # Fork on GitHub, then clone your fork
   git clone https://github.com/YOUR_USERNAME/carbon-track.git
   cd carbon-track
   ```

2. **Install Dependencies**
   ```bash
   pnpm install
   cd hardhat && npm install && cd ..
   ```

3. **Environment Configuration**
   ```bash
   cp .env.example .env.local
   # Edit .env.local with your configuration
   ```

4. **Start Development**
   ```bash
   pnpm dev
   ```

## 📝 Development Guidelines

### Code Style

We use **Biome** for consistent code formatting and linting:

```bash
# Check code style
pnpm lint

# Auto-format code
pnpm format
```

**Key Style Guidelines:**
- Use TypeScript for all new code
- Follow existing naming conventions
- Use meaningful variable and function names
- Add proper type annotations
- Keep functions small and focused

### Project Structure

```
/app/                    # Next.js App Router
├── api/                 # Backend API routes
├── dashboard/           # Protected dashboard pages
└── tokens/              # Public token pages

/components/             # React components
├── ui/                  # Reusable UI components
└── [feature]/           # Feature-specific components

/lib/                    # Core business logic
├── models.ts            # Data models and interfaces
├── mongodb.ts           # Database utilities
├── smart-contract.ts    # Blockchain services
└── utils.ts             # Utility functions

/hardhat/               # Smart contract development
├── contracts/          # Solidity contracts
├── scripts/            # Deployment scripts
└── test/               # Contract tests
```

### TypeScript Guidelines

- **Strict Mode**: All code must pass TypeScript strict mode
- **Interfaces**: Define interfaces for all data structures
- **Type Safety**: Avoid `any` type; use proper type annotations
- **Generics**: Use generics for reusable components

Example:
```typescript
// Good
interface ProductTemplate {
  id: string;
  name: string;
  carbonFootprint: number;
  components: Component[];
}

// Bad
const product: any = { ... };
```

### Component Guidelines

- **Functional Components**: Use React functional components with hooks
- **Props Interface**: Define TypeScript interfaces for all props
- **Composition**: Prefer composition over inheritance
- **Accessibility**: Include ARIA attributes where needed

Example:
```typescript
interface ProductCardProps {
  product: ProductTemplate;
  onEdit?: (id: string) => void;
  className?: string;
}

export function ProductCard({ product, onEdit, className }: ProductCardProps) {
  // Component implementation
}
```

## 🔧 Development Workflow

### Branch Naming

Use descriptive branch names:
- `feature/add-carbon-analytics`
- `fix/wallet-connection-issue`
- `docs/update-api-documentation`
- `refactor/improve-smart-contract-gas`

### Commit Messages

Follow conventional commit format:
```
type(scope): description

[optional body]

[optional footer]
```

Types:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes
- `refactor`: Code refactoring
- `test`: Adding/updating tests
- `chore`: Maintenance tasks

Examples:
```
feat(dashboard): add carbon footprint analytics chart

fix(smart-contract): resolve gas estimation error for large batches

docs(api): update product batch creation endpoint documentation
```

### Pull Request Process

1. **Create Feature Branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make Changes**
   - Write code following style guidelines
   - Add tests for new functionality
   - Update documentation if needed

3. **Test Your Changes**
   ```bash
   # Run linting
   pnpm lint

   # Test smart contracts
   cd hardhat && npx hardhat test

   # Test the application
   pnpm dev
   ```

4. **Commit Changes**
   ```bash
   git add .
   git commit -m "feat(scope): your descriptive message"
   ```

5. **Push and Create PR**
   ```bash
   git push origin feature/your-feature-name
   ```

6. **PR Requirements**
   - Clear title and description
   - Link to related issues
   - Screenshots for UI changes
   - Test results and verification steps

## 🧪 Testing Guidelines

### Smart Contract Testing

All smart contract changes must include tests:

```bash
cd hardhat
npx hardhat test
```

Test requirements:
- Test all new functions
- Test error conditions
- Test gas optimization
- Include edge cases

### Frontend Testing

While we don't have automated frontend tests yet, please:
- Test all user interactions
- Verify responsive design
- Test wallet connections
- Validate form submissions

## 📚 Documentation

### Code Documentation

- **Functions**: Document complex functions with JSDoc
- **APIs**: Update API documentation for changes
- **Components**: Document component props and usage

Example:
```typescript
/**
 * Creates a new product batch and mints corresponding blockchain tokens
 * @param batchData - The batch information including quantity and components
 * @param plantId - The ID of the plant where production occurs
 * @returns Promise resolving to the created batch with token information
 */
async function createProductBatch(
  batchData: CreateBatchRequest,
  plantId: string
): Promise<ProductBatch> {
  // Implementation
}
```

### README Updates

Update README.md when adding:
- New features
- Changed requirements
- New scripts or commands
- Modified setup steps

## 🔒 Security Guidelines

### Smart Contract Security

- **Use OpenZeppelin**: Leverage battle-tested contracts
- **Gas Optimization**: Optimize for Avalanche network
- **Access Control**: Implement proper permission checks
- **Input Validation**: Validate all inputs
- **Error Handling**: Provide clear error messages

### Web Application Security

- **Environment Variables**: Never commit secrets
- **Input Sanitization**: Validate and sanitize all inputs
- **Error Handling**: Don't expose internal errors
- **Wallet Security**: Implement proper wallet connection flows

## 🐛 Issue Reporting

### Bug Reports

Include:
- Clear description of the issue
- Steps to reproduce
- Expected vs actual behavior
- Environment information (OS, browser, Node version)
- Screenshots or error messages

### Feature Requests

Include:
- Clear description of the feature
- Use case and benefits
- Proposed implementation approach
- Any relevant mockups or examples

## 🏷️ Labels and Priorities

We use these labels for issues and PRs:

**Type:**
- `bug` - Something isn't working
- `enhancement` - New feature or improvement
- `documentation` - Documentation needs
- `question` - Questions or discussions

**Priority:**
- `priority/high` - Critical issues
- `priority/medium` - Important but not urgent
- `priority/low` - Nice to have

**Area:**
- `area/frontend` - Frontend/UI changes
- `area/backend` - Backend/API changes
- `area/smart-contract` - Blockchain/contract changes
- `area/docs` - Documentation changes

## 🎯 Contribution Areas

We welcome contributions in:

### Code
- New features and enhancements
- Bug fixes and improvements
- Performance optimizations
- Security improvements

### Documentation
- API documentation
- User guides and tutorials
- Code examples
- Architecture documentation

### Testing
- Unit tests for smart contracts
- Integration testing
- Performance testing
- Security testing

### Design
- UI/UX improvements
- Accessibility enhancements
- Mobile responsiveness
- Design system improvements

## 📞 Getting Help

If you need help:
- **GitHub Discussions**: For general questions
- **Issues**: For bug reports and feature requests
- **Code Review**: Ask for feedback on complex changes

## 🎉 Recognition

Contributors will be:
- Listed in our contributors section
- Mentioned in release notes for significant contributions
- Invited to our contributor community discussions

Thank you for contributing to Carbon Track and helping build the future of supply chain transparency! 🌱