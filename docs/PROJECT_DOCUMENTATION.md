# Angular NgRx RealWorld Conduit - Project Documentation

## 1. Project Overview

**Purpose**: A feature-rich Medium clone blog platform where users can create, read, update, and delete articles, follow other users, and favorite articles.

**Domain**: Social blogging and content sharing platform

**Target Users**: 
- Content creators who want to publish articles
- Readers who want to discover and engage with content
- Community members who want to follow authors and curate favorites

**Core Value Proposition**: Provides a complete modern blogging experience with social features including article authoring, user profiles, following system, and content discovery.

## 2. Functional Documentation

### 2.1 Feature Inventory

**Article Management**
- User role: Authenticated Users
- Primary use case: Content creation and management
- Key user flows:
  1. Create new article with title, description, body, and tags
  2. Edit existing articles (author only)
  3. Delete articles (author only)
  4. View article details with comments
  5. Favorite/unfavorite articles

**User Authentication**
- User role: Anonymous and Authenticated Users
- Primary use case: Account management and access control
- Key user flows:
  1. Register new account with username, email, password
  2. Login with email and password
  3. Update profile settings
  4. Logout from application

**Social Features**
- User role: Authenticated Users
- Primary use case: Social interaction and content discovery
- Key user flows:
  1. Follow/unfollow other users
  2. View user profiles with their articles
  3. Browse global feed of all articles
  4. Browse personal feed of followed users' articles
  5. Add and delete comments on articles

**Content Discovery**
- User role: All Users
- Primary use case: Finding relevant content
- Key user flows:
  1. Browse articles by tags
  2. Filter articles by popularity
  3. Search through article listings
  4. View popular tags
  5. Paginate through article lists

### 2.2 User Journey Maps

**Authentication Flow**:
1. User visits homepage
2. Click "Sign in" or "Sign up"
3. Complete authentication form
4. Redirect to homepage with authenticated state
5. Access to protected features unlocked

**Article Creation Flow**:
1. Authenticated user clicks "New Article"
2. Fill in article form (title, description, body, tags)
3. Submit article
4. Redirect to article detail view
5. Article appears in user's profile and global feed

**Social Interaction Flow**:
1. User discovers article or author
2. Click to favorite article or follow author
3. Optimistic UI update provides immediate feedback
4. Backend synchronization confirms action
5. Updated state reflects in relevant feeds

### 2.3 Business Logic Rules

**Article Management Rules**:
- Only authenticated users can create articles
- Only article authors can edit or delete their articles
- Article slug must be unique and URL-friendly
- Articles require title, description, and body content
- Tags are optional but recommended for discoverability

**User Management Rules**:
- Email addresses must be unique across all users
- Username must be unique and URL-friendly
- Password and email fields require basic required validation only
- User profiles support bio, image, and basic information

**Social Features Rules**:
- Users cannot follow themselves
- Users cannot favorite their own articles
- Comments can only be deleted by their authors
- Following/unfollowing updates both user's following list and target's followers

**Content Discovery Rules**:
- Global feed shows all articles in reverse chronological order
- Personal feed shows articles from followed users only
- Popular tags are calculated based on article frequency
- Pagination limits articles to manageable chunks

## 3. Technical Documentation

### 3.1 Architecture Overview

**Technology Stack**:
- Angular 20.0.3 (Latest standalone components)
- NgRx Signals 20.0.0-beta.0 (State management)
- TypeScript 5.8.3
- RxJS 7.8.1 (Reactive programming)
- Nx 21.2.0 (Monorepo tooling)
- Marked 5.0.1 (Markdown processing)
- Jest 29.7.0 (Testing framework)
- Playwright 1.36.0 (E2E testing)

**Project Structure**: Nx monorepo with feature-based library organization
- Apps contain deployable applications
- Libs contain reusable feature libraries
- Shared libraries provide common utilities

**Design Patterns**:
- Component-based architecture with standalone components
- Feature-based modular design
- Signal-based state management
- Dependency injection using Angular's DI system
- Reactive programming with RxJS observables

**Data Flow**:
1. User interactions trigger component methods
2. Components dispatch actions to NgRx Signal stores
3. Stores manage state updates and side effects
4. Services handle HTTP communication with backend
5. Components subscribe to store signals for reactive updates

**Component Hierarchy**:
- App component serves as root shell
- Layout components provide navigation structure
- Feature components handle specific functionality
- Shared components provide reusable UI elements

**State Management**:
- NgRx Signals for reactive state management
- Store-based architecture with clear separation of concerns
- Optimistic updates for better user experience
- Error handling and loading states

**Routing Strategy**:
- Standard path-based routing (not hash-based)
- Lazy loading for feature modules
- Modern effect-based authentication guards
- Resolvers for data pre-loading

### 3.2 Development Environment

**Prerequisites**:
- Node.js (version compatible with Angular 20.0.3)
- npm (package manager as indicated by package-lock.json)
- Angular CLI (for global ng commands)

**Installation Steps**:
```bash
# Clone the repository
git clone https://github.com/jiteshy/angular-ngrx-realworld-app.git

# Navigate to project directory
cd angular-ngrx-realworld-app

# Install dependencies
npm install

# Start development server
npm start
```

**Configuration**:
- Environment configurations in `apps/conduit/src/environments/`
- Development environment: `environment.ts`
- Production environment: `environment.prod.ts`
- API URL configuration required for backend communication

**Common Commands**:
```bash
# Development server
npm start

# Build for production
npm run build

# Run tests
npm test

# Run end-to-end tests
npm run e2e

# Lint code
npm run lint

# Format code
npm run format

# Generate dependency graph
npm run dep-graph
```

### 3.3 Application Configuration

**Required Configuration**:
- **API Base URL**: Backend API endpoint for all HTTP requests
- **Environment Variables**: Production/development environment settings

**Configuration Files**:
- `apps/conduit/src/environments/environment.ts` - Development settings
- `apps/conduit/src/environments/environment.prod.ts` - Production settings

**Configuration Structure**:
```typescript
export const environment = {
  production: false,
  api_url: 'https://real-world-app-39656dff2ddc.herokuapp.com/api'
};
```

**Configuration Requirements**:
- **Development**: Local API server or public demo API
- **Production**: Configured backend API with CORS support
- **Testing**: Test API endpoints or mock services

### 3.4 Code Organization

**Directory Structure**:
```
angular-ngrx-realworld-app/
├── apps/
│   ├── conduit/                 # Main application
│   └── conduit-e2e/            # E2E tests
├── libs/
│   ├── articles/               # Article feature libraries
│   ├── auth/                   # Authentication libraries
│   ├── core/                   # Core utilities and services
│   ├── home/                   # Home page features
│   ├── profile/                # User profile features
│   ├── settings/               # User settings features
│   └── ui/                     # Shared UI components
```

**Library Structure**:
- **data-access**: State management and API services
- **feature-**: UI components and feature logic
- **ui/components**: Reusable UI components

**File Naming Conventions**:
- Components: `*.component.ts` (kebab-case)
- Services: `*.service.ts` (kebab-case)
- Stores: `*.store.ts` (kebab-case)
- Models: `*.model.ts` (kebab-case)
- Specs: `*.spec.ts` (kebab-case)

**Import/Export Patterns**:
- Barrel exports through `index.ts` files
- Feature libraries expose public APIs
- Internal implementation details remain private

### 3.5 API Documentation

**External APIs**:
This application consumes the RealWorld API specification:

**API Endpoints Inventory**:

**Authentication Endpoints**:
- `POST /api/users/login` - User login
- `POST /api/users` - User registration
- `GET /api/user` - Get current user
- `PUT /api/user` - Update user profile

**Articles Endpoints**:
- `GET /api/articles` - List articles (with filtering)
- `GET /api/articles/feed` - Get user's article feed
- `POST /api/articles` - Create new article
- `GET /api/articles/:slug` - Get article by slug
- `PUT /api/articles/:slug` - Update article
- `DELETE /api/articles/:slug` - Delete article
- `POST /api/articles/:slug/favorite` - Favorite article
- `DELETE /api/articles/:slug/favorite` - Unfavorite article

**Comments Endpoints**:
- `GET /api/articles/:slug/comments` - Get article comments
- `POST /api/articles/:slug/comments` - Add comment to article
- `DELETE /api/articles/:slug/comments/:id` - Delete comment

**Profile Endpoints**:
- `GET /api/profiles/:username` - Get user profile
- `POST /api/profiles/:username/follow` - Follow user
- `DELETE /api/profiles/:username/follow` - Unfollow user

**Tags Endpoints**:
- `GET /api/tags` - Get all tags

**API Service Architecture**:
- **API Service**: `libs/core/http-client/src/lib/api.service.ts`
- **HTTP Client Configuration**: Angular HttpClient with interceptors
- **Base URL Configuration**: Configurable API base URL through environment
- **Error Handling**: Centralized error handling through interceptors

**Authentication Flow**:
- JWT token-based authentication
- Token management handled by authentication service
- Automatic token inclusion in API requests via HTTP interceptors
- Session management through NgRx Signals state

**API Integration Patterns**:
- RxJS Observables for async operations
- Reactive error handling with retry logic
- Loading states managed through NgRx Signals
- Optimistic updates for better UX

### 3.6 Data Management

**State Management Architecture**:
- **NgRx Signals**: Modern signal-based state management
- **Store Structure**: Feature-based store organization
- **Global State**: Shared across application features
- **Local State**: Component-specific state management

**Store Documentation**:

**Auth Store** (`libs/auth/data-access/src/auth.store.ts`):
- **Responsibilities**: User authentication and authorization
- **State Shape**: Current user, authentication status, loading states
- **Actions**: Login, logout, register, update profile
- **Selectors**: Current user, authentication status, loading states

**Articles Store** (`libs/articles/data-access/src/articles-list.store.ts`):
- **Responsibilities**: Article listing and filtering
- **State Shape**: Articles array, pagination, filters, loading states
- **Actions**: Load articles, filter by tag, paginate, favorite/unfavorite
- **Selectors**: Articles list, pagination info, loading states

**Article Store** (`libs/articles/data-access/src/article.store.ts`):
- **Responsibilities**: Individual article management
- **State Shape**: Current article, comments, loading states
- **Actions**: Load article, update article, add/delete comments
- **Selectors**: Current article, comments, loading states

**Profile Store** (`libs/profile/data-access/src/profile.store.ts`):
- **Responsibilities**: User profile management
- **State Shape**: Profile data, user articles, loading states
- **Actions**: Load profile, follow/unfollow, load user articles
- **Selectors**: Profile data, articles, following status

**Home Store** (`libs/home/feature-home/src/home.store.ts`):
- **Responsibilities**: Home page tag management
- **State Shape**: Popular tags array
- **Actions**: Load tags (getTags method)
- **Selectors**: Popular tags

**Data Flow Patterns**:
- **Component to Store**: Direct store method calls
- **Store to API**: Service injection and HTTP calls
- **Async Operations**: Observable-based with loading states
- **State Updates**: Immutable updates through signals
- **Error Handling**: Centralized error state management

**Data Models**:
```typescript
// Core entity interfaces
interface User {
  email: string;
  username: string;
  bio?: string;
  image?: string;
  token?: string;
}

interface Article {
  slug: string;
  title: string;
  description: string;
  body: string;
  tagList: string[];
  createdAt: string;
  updatedAt: string;
  favorited: boolean;
  favoritesCount: number;
  author: Profile;
}

interface Profile {
  username: string;
  bio?: string;
  image?: string;
  following: boolean;
}

interface Comment {
  id: number;
  body: string;
  createdAt: string;
  author: Profile;
}
```

**Data Synchronization**:
- **Real-time Updates**: Optimistic UI updates
- **Conflict Resolution**: Server state takes precedence
- **Offline Support**: Not implemented
- **Background Sync**: Not implemented

## 4. Development Guidelines

### 4.1 Code Standards

**Coding Style**:
- Single quotes for string literals
- 2-space indentation
- Kebab-case for file names
- PascalCase for class names
- camelCase for variables and methods

**Linting and Formatting**:
- ESLint with Angular-specific rules
- Prettier for code formatting
- Husky pre-commit hooks for formatting
- NgRx-specific linting rules

**Testing Requirements**:
- Jest for unit testing
- Playwright for E2E testing
- Test files follow `*.spec.ts` convention
- Coverage expectations per library

**Git Workflow**:
- Feature branch workflow
- Commit message conventions
- Pull request reviews required
- Main branch protection

### 4.2 Testing Strategy

**Unit Tests**:
- Component testing with Angular Testing Utilities
- Service testing with mock dependencies
- Store testing with NgRx Signals testing utilities
- Pipe testing for markdown and utility functions

**Integration Tests**:
- Component interaction testing
- API service integration testing
- Store-service integration testing
- Form validation testing

**E2E Tests**:
- Authentication flow testing
- Article management workflows
- Social features testing
- Navigation and routing testing

**Testing Files Found**:
- Component specs: `*.component.spec.ts`
- Service specs: `*.service.spec.ts`
- Store specs: `*.store.spec.ts`
- E2E specs: `apps/conduit-e2e/src/**/*.spec.ts`

### 4.3 Performance Considerations

**Bundle Optimization**:
- Lazy loading for feature modules
- Tree shaking for unused code
- Production build optimization
- Code splitting strategies

**Runtime Performance**:
- OnPush change detection strategy
- TrackBy functions for ngFor loops
- Async pipe for observables
- Deferrable views for optimization

**Memory Management**:
- Proper subscription cleanup
- Component lifecycle management
- State cleanup on navigation
- Image optimization strategies

## 5. Deployment & Operations

### 5.1 Build Process

**Build Commands**:
```bash
# Development build
npm run build

# Production build
npm run build -- --configuration=production

# Analyze bundle size
npm run build -- --configuration=production --stats-json
```

**Build Outputs**:
- `dist/apps/conduit/` - Production-ready static assets
- Optimized JavaScript bundles
- CSS files with optimization
- Asset optimization and compression

**Environment Configuration**:
- Build-time environment variable injection
- Configuration based on target environment
- API URL configuration per environment

### 5.2 Deployment Process

**Static Hosting**:
- Suitable for Netlify, Vercel, AWS S3, or similar
- Single-page application hosting requirements
- Proper routing configuration needed

**Deployment Steps**:
1. Build application for production
2. Configure environment variables
3. Deploy static assets to hosting platform
4. Configure routing for SPA
5. Set up SSL certificates if required

**Environment Variables**:
- API_URL: Backend API base URL
- PRODUCTION: Production environment flag

### 5.3 Monitoring & Debugging

**Browser Development Tools**:
- Angular DevTools for component inspection
- Redux DevTools for state management
- Network tab for API monitoring
- Performance profiling tools

**Error Tracking**:
- Console error monitoring
- Network error handling
- User experience error tracking
- Performance monitoring

**Debugging Strategies**:
- Component state inspection
- Store state debugging
- Network request debugging
- Performance bottleneck identification

## 6. Quick Start Guides

### 6.1 For New Developers

**5-Step Onboarding Process**:
1. **Setup Environment**: Install Node.js and clone repository
2. **Install Dependencies**: Run `npm install`
3. **Start Development**: Run `npm start`
4. **Explore Structure**: Review libs organization and app structure
5. **Make First Change**: Update a component or add a feature

**First Feature to Implement**:
1. Add a new article tag filter
2. Create component in appropriate library
3. Add store actions for tag filtering
4. Connect component to store
5. Test functionality and create unit tests

**Common Pitfalls**:
- Forgetting to unsubscribe from observables
- Not using proper change detection strategies
- Mixing reactive and imperative patterns
- Not following library boundaries

### 6.2 For QA Engineers

**Test Execution**:
```bash
# Run all unit tests
npm test

# Run E2E tests
npm run e2e

# Run specific test suite
npm test -- --testNamePattern="Article"
```

**Key Testing Areas**:
- User authentication flows
- Article CRUD operations
- Social features (follow, favorite)
- Navigation and routing
- Form validation and error handling

**Test Data Management**:
- Use demo API for testing
- Create test users and articles
- Clean up test data after tests
- Mock external dependencies

## 7. Troubleshooting

### Common Issues

**Build Errors**:
- **Issue**: Module not found errors
- **Solution**: Check import paths and library exports
- **Prevention**: Use barrel exports and proper library structure

**Runtime Errors**:
- **Issue**: Cannot read property of undefined
- **Solution**: Add null checks and optional chaining
- **Prevention**: Use TypeScript strict mode

**API Connection Issues**:
- **Issue**: CORS errors or API unavailable
- **Solution**: Configure API URL and check backend status
- **Prevention**: Use environment configuration

**Performance Issues**:
- **Issue**: Slow initial load or navigation
- **Solution**: Implement lazy loading and optimization
- **Prevention**: Use Angular performance best practices

### Environment Issues

**Node.js Version Compatibility**:
- Ensure Node.js version supports Angular 20.0.3
- Use Node Version Manager (nvm) for version management
- Check package.json engines field for requirements

**Dependency Issues**:
- Clear node_modules and reinstall dependencies
- Check for peer dependency warnings
- Update dependencies following Angular update guide

### Runtime Errors

**Authentication Errors**:
- Check JWT token validity and storage
- Verify API endpoint configuration
- Confirm user session management

**Navigation Issues**:
- Check route configuration and guards
- Verify component lazy loading
- Confirm routing module imports

**State Management Issues**:
- Check store initialization and injection
- Verify action dispatching and handling
- Confirm selector subscriptions

### Performance Issues

**Bundle Size**:
- Analyze bundle with webpack-bundle-analyzer
- Implement code splitting and lazy loading
- Remove unused dependencies and code

**Memory Leaks**:
- Check component subscription cleanup
- Verify store state management
- Monitor memory usage during development

This documentation provides a comprehensive guide for developers and QA engineers to understand, develop, and maintain the Angular NgRx RealWorld Conduit application. The application demonstrates modern Angular development practices with NgRx Signals for state management and follows the RealWorld specification for a complete blogging platform implementation. 