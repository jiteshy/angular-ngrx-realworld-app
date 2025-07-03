# Conduit - Angular NgRx RealWorld Application

## 1. Project Overview

**Purpose**: A Medium.com clone demonstrating real-world Angular application patterns with article publishing, user authentication, and social interactions.

**Domain**: Social blogging platform serving developers and content creators who want to share technical articles and engage with the community.

**Target Users**: 
- **Content Creators**: Authors writing and publishing technical articles
- **Readers**: Developers browsing and consuming technical content  
- **Community Members**: Users interacting through comments, favorites, and following

**Core Value Proposition**: Showcases modern Angular architecture with NgRx Signals, standalone components, and zoneless change detection while providing a fully functional blogging platform for developers.

## 2. Functional Documentation

### 2.1 Feature Inventory

**Authentication & User Management**
- **User Role**: Anonymous/Authenticated Users
- **Primary Use Case**: User registration, login, profile management
- **Key User Flows**: 
  1. Register new account with username/email/password
  2. Login with email/password credentials
  3. Update profile information in settings
  4. Logout and clear session

**Article Management**
- **User Role**: Authenticated Authors
- **Primary Use Case**: Create, edit, delete articles
- **Key User Flows**:
  1. Navigate to editor (requires authentication)
  2. Create article with title, description, body, tags
  3. Edit existing articles (author only)
  4. Delete own articles

**Content Discovery**
- **User Role**: All Users
- **Primary Use Case**: Browse and read articles
- **Key User Flows**:
  1. View global article feed on homepage
  2. Filter articles by tags from sidebar
  3. Switch to personal feed (authenticated users)
  4. Read full article with markdown rendering

**Social Features**
- **User Role**: Authenticated Users
- **Primary Use Case**: Engage with content and authors
- **Key User Flows**:
  1. Favorite/unfavorite articles
  2. Follow/unfollow authors
  3. Comment on articles
  4. View user profiles with article history

**Profile Management**
- **User Role**: All Users
- **Primary Use Case**: View user profiles and content
- **Key User Flows**:
  1. Browse author profiles
  2. View user's published articles
  3. View user's favorited articles

### 2.2 User Journey Maps

**Authentication Flow**:
1. Visit application → Anonymous homepage with global feed
2. Click login/register → Form submission
3. Successful authentication → Redirect to homepage (authenticated view)
4. Access protected features (editor, settings, personal feed)

**Article Creation Journey**:
1. Login required → Navigate to editor via navbar
2. Fill article form (title, description, body, tags)
3. Submit article → Publish and redirect to article view
4. Share or further edit published content

**Content Discovery Flow**:
1. Homepage visit → Global article feed displayed
2. Browse sidebar tags or switch to personal feed
3. Select article → Full article view with author info
4. Engage via favorites, comments, or author following

### 2.3 Business Logic Rules

**Data Validation Rules**:
- Required field validation for email, username, password
- Article title, description, and body are required
- No client-side email format validation (server-side only)

**Business Constraints**:
- Only authenticated users can create/edit/delete articles
- Users can only modify their own articles  
- Comments can only be deleted by comment authors
- Article favoriting requires authentication

**Workflow Dependencies**:
- Authentication required for: article creation, editing, commenting, favoriting, following
- User session must be valid for protected operations
- Article must exist before comments can be added

**Integration Points**:
- External RealWorld API: `https://real-world-app-39656dff2ddc.herokuapp.com/api`
- Markdown rendering for article content (marked library)
- Cookie-based session management

## 3. Technical Documentation

### 3.1 Architecture Overview

**Technology Stack**:
- **Frontend Framework**: Angular 20.0.3 with standalone components
- **State Management**: NgRx Signals 20.0.0-beta.0
- **Build System**: Nx 21.2.0 monorepo workspace
- **Language**: TypeScript 5.8.3
- **Styling**: SCSS with component-scoped styles
- **HTTP Client**: Angular HttpClient with cookie-based authentication
- **Testing**: Jest 29.7.0 (unit tests) + Playwright 1.36.0 (E2E tests)
- **Package Manager**: npm (confirmed by package-lock.json presence)

**Project Structure**: Nx workspace with feature-based library architecture
```
apps/conduit/                 # Main application
libs/
  ├── auth/                   # Authentication features
  │   ├── data-access/        # Auth store and services
  │   └── feature-auth/       # Login/register components
  ├── articles/               # Article management
  │   ├── data-access/        # Article stores and services
  │   ├── feature-article/    # Article view component
  │   ├── feature-article-edit/ # Article editor
  │   └── feature-articles-list/ # Article listing
  ├── profile/                # User profile features
  ├── home/                   # Homepage with feeds
  ├── settings/               # User settings
  ├── core/                   # Shared utilities
  │   ├── api-types/          # TypeScript interfaces
  │   ├── http-client/        # HTTP service layer
  │   ├── error-handler/      # Error handling
  │   └── forms/              # Form utilities
  └── ui/components/          # Reusable UI components
```

**Design Patterns**:
- **Nx Monorepo**: Feature-based library organization with dependency management
- **Standalone Components**: Modern Angular component architecture (no NgModules)
- **Signal-Based State**: NgRx Signals for reactive state management
- **Lazy Loading**: Route-based code splitting with `loadComponent` and `loadChildren`
- **Provider Pattern**: Dependency injection with `inject()` function

**Data Flow**:
1. **Component** → triggers signal store methods via user interactions
2. **Signal Store** → calls service methods through rxMethod for async operations  
3. **Service** → makes HTTP requests through ApiService with typed interfaces
4. **HTTP Client** → communicates with external API using cookie-based authentication
5. **State Update** → components react to signal changes automatically

**Frontend Specific**:
- **Component Hierarchy**: App → Layout (Navbar/Footer) → Feature Components
- **State Management**: NgRx Signals with reactive methods (rxMethod, patchState)
- **Routing Strategy**: Standard routing (not hash-based) with lazy loading
- **Change Detection**: Zoneless change detection with OnPush strategy

### 3.2 Development Environment

**Prerequisites**:
- Node.js (compatible with Angular 20.0.3)
- npm (package manager - verified by package-lock.json presence)
- Git for version control

**Installation Steps**:
```bash
# Clone the repository
git clone https://github.com/jiteshy/angular-ngrx-realworld-app.git
cd angular-ngrx-realworld-app

# Install dependencies
npm install

# Start development server
npm start

# Application runs on http://localhost:4200
```

**Configuration**:
**Environment Variables**:
- `environment.ts`: Development configuration with API URL
- `environment.prod.ts`: Production configuration
- API URL: `https://real-world-app-39656dff2ddc.herokuapp.com/api` (hardcoded in both environments)

**Common Commands**:
```bash
# Development
npm start                    # Start dev server
npm run build               # Build for production
npm test                    # Run unit tests
npm run e2e                 # Run E2E tests

# Code Quality
npm run lint                # ESLint code analysis
npm run format              # Prettier code formatting

# Nx-specific commands
npm run affected:test       # Test affected projects
npm run dep-graph          # View dependency graph
```

### 3.3 Code Organization

**Directory Structure**:
```
apps/conduit/src/
├── app/
│   ├── app.component.ts          # Root component
│   ├── app.config.ts             # Application configuration
│   └── layout/                   # Layout components (navbar, footer)
├── environments/                 # Environment configurations
└── main.ts                      # Application bootstrap

libs/
├── auth/
│   ├── data-access/             # Auth store and services
│   └── feature-auth/            # Login/register components
├── articles/
│   ├── data-access/             # Article stores and services
│   ├── feature-article/         # Article view component
│   ├── feature-article-edit/    # Article editor
│   └── feature-articles-list/   # Article listing
├── core/
│   ├── api-types/               # TypeScript interfaces
│   ├── http-client/             # HTTP service layer
│   ├── error-handler/           # Error handling
│   └── forms/                   # Form utilities
└── ui/components/               # Reusable UI components
```

**File Naming Conventions**:
- `*.component.ts` - Angular components
- `*.service.ts` - Injectable services  
- `*.store.ts` - NgRx Signal stores
- `*.model.ts` - TypeScript interfaces/types
- `*.spec.ts` - Unit tests
- All files use kebab-case naming

**Import/Export Patterns**:
- Barrel exports via `index.ts` files
- Relative imports within libraries
- Library imports use TypeScript path mapping (`@realworld/feature`)
- Angular core imports grouped first

**State Management**:
- **NgRx Signals**: Signal-based reactive state with `signalStore`
- **Signal Stores**: Feature-specific stores (AuthStore, ArticlesListStore, etc.)
- **rxMethod**: For handling async operations and side effects
- **patchState**: For immutable state updates
- **Computed Signals**: Derived state from store signals

### 3.4 API Documentation

**For Frontend Applications**:

**External APIs**: 
- **RealWorld API**: Backend REST service for all data operations
- **Base URL**: `https://real-world-app-39656dff2ddc.herokuapp.com/api`
- **Authentication**: Cookie-based authentication with credentials

**API Integration**:
- **HTTP Service**: `ApiService` wraps Angular HttpClient
- **Request Format**: JSON with content-type headers
- **Error Handling**: Global `errorHandlingInterceptor` for HTTP errors
- **Authentication**: Automatic cookie transmission with `withCredentials: true`

**Authentication Flow**:
- Login/Register establishes cookie-based session
- All HTTP requests include `withCredentials: true` 
- Session persistence via browser cookies
- Logout clears session state and redirects

**Error Handling**:
- HTTP errors processed by global interceptor
- Form validation errors displayed via `FormErrorsStore`
- User-friendly error notifications with `ListErrorsComponent`
- Network failures handled gracefully

### 3.5 Data Management

**For Frontend Applications**:

**Client-Side State**: 
```typescript
// Auth State
interface AuthState {
  user: User;
  loggedIn: boolean;
}

// Articles List State  
interface ArticlesListState {
  articles: Article[];
  articlesCount: number;
  listConfig: ArticlesListConfig;
}
```

**Data Models**:
```typescript
interface User {
  email: string;
  username: string;
  bio: string;
  image: string;
}

interface Article {
  slug: string;
  title: string;
  description: string;
  body: string;
  tagList: string[];
  favorited: boolean;
  favoritesCount: number;
  author: Profile;
}
```

**Local Storage**: No client-side persistence used (cookie-based authentication only)

**Data Synchronization**:
- Signal stores automatically sync with backend via reactive methods
- Optimistic updates for user interactions (favorites, follows)
- Manual refresh required for new data (no real-time updates)

## 4. Development Guidelines

### 4.1 Code Standards

**Coding Style**:
- TypeScript strict mode enabled
- Single quotes for string literals
- 2-space indentation
- ESLint + Prettier for code formatting
- Component selector prefix: `cdt-`

**Component Standards**:
- Standalone components (no NgModules)
- OnPush change detection strategy
- Signal-based reactive patterns
- Dependency injection via `inject()` function
- Meaningful component selectors with `cdt-` prefix

**TypeScript Guidelines**:
- Strict typing (avoid `any` type)
- Interface definitions for all data structures
- Optional chaining and nullish coalescing operators
- Template literal strings for multi-line content

**Git Workflow**:
- Main branch for production-ready code
- Feature branches for development
- Husky pre-commit hooks for code formatting
- Conventional commit messages encouraged

### 4.2 Testing Strategy

**Unit Tests**: Jest framework with test files found in the codebase
- **Component Testing**: Shallow rendering with TestBed
- **Service Testing**: Isolated testing with mocked dependencies  
- **Store Testing**: Signal store behavior and state transitions
- **Utility Testing**: Pure functions and helper methods

**Integration Tests**: Angular Testing Library patterns
- **Component Integration**: Component interaction with services
- **HTTP Integration**: Service integration with HttpClient
- **Store Integration**: Component and store interaction patterns

**E2E Tests**: Playwright framework
- **Authentication Flows**: Login, register, logout journeys
- **Article Management**: Create, edit, delete article workflows
- **User Interactions**: Comments, favorites, following
- **Navigation**: Routing and page transitions

**Testing Commands**:
```bash
npm test                    # Run all unit tests
npm run e2e                # Run E2E tests
# Note: watch mode and coverage commands not configured in this project
```

### 4.3 Performance Considerations

**Optimization Strategies**:
- **Lazy Loading**: Feature modules loaded on-demand via route configuration
- **OnPush Change Detection**: Explicit change detection for better performance
- **Zoneless Architecture**: Angular's experimental zoneless change detection
- **Signal-Based Reactivity**: Fine-grained updates with NgRx Signals
- **Deferred Views**: Non-critical components loaded lazily (@defer directive)

**Bundle Optimization**:
- Tree-shaking for unused code elimination
- Code splitting via lazy routes
- Angular build optimizations with esbuild

## 5. Deployment & Operations

### 5.1 Build Process

**For Frontend Applications**:

**Build Commands**:
```bash
npm run build                    # Development build
npm run build -- --configuration=production  # Production build
```

**Build Configuration**:
- **Build Tool**: Angular CLI with esbuild for faster builds
- **Output Directory**: `dist/apps/conduit`
- **Environment Handling**: File replacement for production builds
- **Asset Optimization**: Bundling, minification, and hash-based caching

**Environment-Specific Configurations**:
- Development: Source maps enabled, optimization disabled
- Production: File replacement, optimization enabled, no source maps

### 5.2 Deployment Process

**For Frontend Applications**:

**Static Hosting**:
- Application deployable to any static hosting provider
- Build outputs to `dist/apps/conduit` directory
- SPA routing requires server configuration for fallback to index.html
- No environment variable injection at runtime (build-time only)

**Server Requirements**:
- Static file serving capability
- HTTPS recommended for secure cookie transmission
- Proper SPA routing configuration

### 5.3 Monitoring & Debugging

**For Frontend Applications**:

**Browser Developer Tools**: Standard debugging with Angular DevTools extension support
- **Component Inspector**: Angular-specific debugging
- **State Debugging**: NgRx Signals state inspection
- **Network Monitoring**: HTTP request/response tracking

**Error Tracking**: Console logging for errors and HTTP interceptor for API issues
- **Client-Side Errors**: Browser console logging
- **API Errors**: Centralized error handling via interceptor
- **No External Monitoring**: No Sentry or similar tools configured

## 6. Quick Start Guides

### 6.1 For New Developers

**5-Step Onboarding Process**:
1. **Setup**: Install Node.js and run `npm install` 
2. **Development**: Run `npm start` and visit `http://localhost:4200`
3. **Architecture**: Explore `libs/` structure and signal stores
4. **Patterns**: Study `AuthStore` and component communication patterns
5. **Testing**: Run `npm test` to verify environment setup

**First Feature Implementation**:
1. Create library: `nx generate @nx/angular:library feature-example`
2. Add signal store for state management
3. Create component with reactive signals
4. Add lazy route in `app.config.ts`
5. Test functionality with Jest

**Common Pitfalls**:
- Use `patchState()` for state updates, not direct mutation
- Import dependencies in component `imports` array (standalone components)
- Use TypeScript path mapping (`@realworld/feature`) for library imports
- Remember OnPush change detection requirements

### 6.2 For QA Engineers

**Test Execution**:
```bash
npm test                    # Unit tests with Jest
npm run e2e                # E2E tests with Playwright
```

**Key Testing Areas**:
- **Authentication**: Registration, login, logout flows
- **Article Management**: CRUD operations and validation
- **User Interactions**: Comments, favorites, following
- **Navigation**: Route changes and lazy loading

**Bug Reporting**: 
- Include browser version and console errors
- Provide step-by-step reproduction steps
- Screenshot/video for UI issues
- Network logs for API-related problems

## 7. Troubleshooting

### Common Issues

**Installation Problems**:
```bash
# Clear npm cache
npm cache clean --force
rm -rf node_modules package-lock.json
npm install
```

**Development Server Issues**:
```bash
# Port conflicts
npm start -- --port 4201

# Clear Angular cache
npx ng cache clean
```

**Build Failures**:
- Check Angular/TypeScript version compatibility
- Verify all dependencies match required versions
- Increase Node.js memory for large builds

**Runtime Errors**:
- Verify API endpoint accessibility
- Clear browser cookies for authentication issues
- Check network connectivity for API calls

**Performance Issues**:
- Use `npm run dep-graph` to analyze dependencies
- Verify OnPush change detection implementation
- Check for unsubscribed observables causing memory leaks

**Testing Issues**:
- Ensure development server is running for E2E tests
- Clear browser state between test runs
- Verify Jest configuration in each library

---

*Documentation generated by analyzing actual codebase implementation. All commands and configurations verified against source code.* 