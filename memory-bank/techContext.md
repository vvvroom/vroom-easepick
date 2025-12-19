# Technical Context: easepick

## Technology Stack

### Core Technologies
- **Language**: TypeScript 4.6+
- **Build System**: Rollup 2.x with custom configuration
- **Testing**: Jest 27.x with ts-jest
- **Linting**: ESLint 8.x with TypeScript rules
- **Styling**: SCSS with PostCSS and Autoprefixer
- **Package Management**: npm workspaces

### Runtime Environment
- **Target**: Modern browsers supporting Shadow DOM
- **ES Version**: ES2018+ features
- **DOM APIs**: Shadow DOM, Custom Elements, EventTarget
- **No External Dependencies**: Pure browser APIs only

## Development Setup

### Local Development
```bash
npm install          # Install all workspace dependencies
npm run dev         # Development build with watch mode
npm run prod        # Production build
npm run test        # Run test suite
```

### Package Structure
- **workspaces**: npm workspaces for monorepo management
- **build**: Rollup configuration for multiple output formats (ESM, UMD)
- **assets**: SCSS compilation with PostCSS processing

### Build Pipeline
1. **TypeScript Compilation**: Source → JavaScript + Type Definitions
2. **SCSS Processing**: Source → CSS with vendor prefixing
3. **Bundling**: ESM + UMD outputs for different consumption patterns
4. **Minification**: Terser for production builds

## Technical Constraints

### Browser Support
- **Modern Browsers Only**: Shadow DOM support required
- **No IE Support**: Legacy browser compatibility not prioritized
- **Progressive Enhancement**: Graceful degradation where possible

### Bundle Size Limits
- **Core Package**: < 50KB gzipped
- **Plugin Packages**: < 20KB gzipped each
- **Bundle Package**: < 150KB gzipped (all plugins included)

### API Stability
- **Semantic Versioning**: Strict adherence to semver
- **Breaking Changes**: Major version bumps only
- **Deprecation Warnings**: Advance notice for API changes

## Dependencies

### Development Dependencies
- **@rollup/plugin-node-resolve**: ESM module resolution
- **@rollup/plugin-typescript**: TypeScript compilation in Rollup
- **@types/jest**: Type definitions for Jest
- **eslint**: Code linting and style enforcement (updated to v8.x)
- **jest**: Testing framework (updated with jsdom environment)
- **jest-environment-jsdom**: DOM testing environment for Jest
- **rollup**: Module bundler (security updates applied)
- **sass**: SCSS compilation (deprecation warnings for @import syntax)
- **typescript**: Type checking and compilation (updated within 4.6.x range)

### Runtime Dependencies
- **None**: Zero runtime dependencies for maximum compatibility

### Internal Dependencies
- **@easepick/datetime**: Date manipulation utilities
- **@easepick/core**: Required by all plugins
- **@easepick/base-plugin**: Base plugin functionality

## Tool Usage Patterns

### TypeScript Configuration
- **Strict Mode**: Full type checking enabled
- **Declaration Files**: Generated for all packages
- **Module Resolution**: Node.js style with path mapping
- **ES2018 Target**: Modern JavaScript features

### Testing Strategy
- **Unit Tests**: Individual component testing
- **Integration Tests**: Plugin-core interaction
- **DOM Testing**: JSDOM environment for browser simulation
- **Coverage**: Minimum 80% coverage requirement

### Code Quality
- **ESLint**: Airbnb config with TypeScript extensions
- **Prettier**: Code formatting (inferred from editor config)
- **EditorConfig**: Consistent whitespace and encoding
- **Git Hooks**: Pre-commit linting and testing

### Release Process
- **Automated Publishing**: GitHub Actions CI/CD
- **Version Management**: npm version with conventional commits
- **Changelog Generation**: Automated from commit messages
- **NPM Publishing**: Automated package publishing

## Development Workflow

### Feature Development
1. Create feature branch from main
2. Implement changes with tests
3. Update documentation
4. Create pull request
5. Code review and merge
6. Automated release

### Plugin Development
1. Extend BasePlugin class
2. Implement required interfaces
3. Add TypeScript definitions
4. Write comprehensive tests
5. Update documentation
6. Publish as separate package

### Maintenance Tasks
- **Dependency Updates**: Monthly review and updates
- **Security Audits**: Regular npm audit checks
- **Performance Monitoring**: Bundle size tracking
- **Browser Compatibility**: Testing against supported browsers

## Performance Considerations

### Bundle Optimization
- **Tree Shaking**: ESM exports enable dead code elimination
- **Code Splitting**: Separate packages prevent bundle bloat
- **Minification**: Production builds optimized for size
- **Lazy Loading**: Plugins loaded on demand

### Runtime Performance
- **Virtual DOM**: Efficient DOM updates through Shadow DOM
- **Event Delegation**: Single event listener with target checking
- **Memory Management**: Proper cleanup on destroy
- **Animation Performance**: CSS transforms over layout changes

### Developer Experience
- **Hot Reload**: Development server with live updates
- **Type Safety**: Comprehensive TypeScript definitions
- **IntelliSense**: Full IDE support and autocomplete
- **Documentation**: Comprehensive API docs and examples
