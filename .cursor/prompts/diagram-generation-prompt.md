---
description:
globs:
alwaysApply: false
---

# Architecture Diagram Generation Prompt

You are a principal software engineer tasked with creating accurate system architecture diagrams based on validated project documentation and source code analysis. This prompt is part of a comprehensive documentation workflow that ensures diagram accuracy through validated technical details.

**Input Sources** (in priority order):

1. **Generated Documentation**: `docs/PROJECT_DOCUMENTATION.md` (primary source - validated technical details)
2. **Source Code Analysis**: Complete file tree and code structure
3. **README File**: Supporting project information

**Output**:

- Mermaid.js diagram code
- Interactive HTML file: `docs/architecture_diagram.html` with zoom functionality

## Workflow Context

This diagram generation is **Phase 5** of the documentation workflow:

1. ✅ Agent rules applied
2. ✅ Documentation generated
3. ✅ Documentation validated against source code
4. ✅ Inaccuracies corrected
5. **📊 Architecture diagram creation** (current phase)

The generated documentation provides **validated, accurate** technical details that should be the foundation for the architecture diagram.

## Primary Architecture Analysis

**EXECUTE FIRST**: Analyze the validated project documentation

You will be provided with the generated `PROJECT_DOCUMENTATION.md` which contains validated technical architecture details. This documentation has been verified against the actual source code and corrected for accuracy.

**Key sections to leverage**:

- **3.1 Architecture Overview**: Technology stack, design patterns, data flow
- **3.2 Development Environment**: Build process, deployment strategy
- **3.3 Application Configuration**: Configuration requirements and environment setup
- **3.4 Code Organization**: Directory structure, file naming, import patterns
- **3.5 API Documentation**: 
  - **For Frontend**: API endpoints inventory, service architecture, authentication flow, integration patterns
  - **For Backend**: Complete REST API specification, authentication mechanisms, data models
- **3.6 Data Management**: 
  - **For Frontend**: State management architecture, store documentation, data flow patterns, data models
  - **For Backend**: Database schema, migration strategy, data access patterns, caching strategy

**Analysis Instructions**:

1. **Extract Technical Architecture**: Use the validated architecture overview as the foundation
2. **Identify Core Components**: Map documented features to architectural components
3. **Map API Integration**: Leverage documented API endpoints inventory and service architecture
4. **Understand State Management**: Use documented store responsibilities and data flow patterns  
5. **Incorporate Configuration**: Show documented configuration layers and environment handling
6. **Map Technology Stack**: Include all validated technologies and frameworks
7. **Incorporate Design Patterns**: Show documented architectural patterns

## Secondary Source Code Analysis

**EXECUTE SECOND**: GitHub Repository Detection and Source Code Analysis

### Step 1: GitHub Repository URL Detection

**CRITICAL**: Before creating any click events, detect the GitHub repository URL from dependency files:

**Check these files in order** (first found wins):

1. **package.json**: Look for `repository.url` or `repository` field
2. **pom.xml**: Look for `<scm><url>` or `<url>` in project section
3. **Cargo.toml**: Look for `repository` field in `[package]` section
4. **go.mod**: Look for module path if it's a GitHub URL
5. **composer.json**: Look for `repository` field
6. **pyproject.toml** or **setup.py**: Look for repository URLs

**Example extraction patterns**:

```json
// package.json
{
  "repository": {
    "url": "https://github.com/user/repo.git"
  }
  // OR
  "repository": "https://github.com/user/repo"
}
```

**URL Processing**:

- Remove `.git` suffix if present
- Ensure format: `https://github.com/owner/repo`
- Extract owner and repo name for click events

### Step 2: Source Code Analysis

Examine the source code to:

- **Verify Component Structure**: Confirm documented architecture in actual files
- **Map File Paths**: Create clickable links using detected GitHub URL
- **Identify Visual Groupings**: Organize components by documented folder structure
- **Validate Relationships**: Ensure documented data flow matches code patterns

## Diagram Creation Instructions

### Step 1: Architecture Foundation

Based on the **validated documentation**, identify:

- **Main System Layers**: Frontend, backend, external services (as documented)
- **API Integration Layer**: Documented API endpoints, service architecture, authentication mechanisms
- **State Management Layer**: Documented stores, state shapes, data flow patterns, async operations
- **Configuration Layer**: Documented configuration sources, environment handling, validation
- **Core Components**: Authentication, API integration, state management, UI components
- **Technology Stack**: All frameworks and libraries from validated documentation
- **Data Flow Patterns**: How data moves through the documented architecture

### Step 2: Component Mapping

Map documented architecture to actual code files:

- **API Services**: Map to documented API service files, HTTP client configuration, interceptors
- **State Management**: Map to documented stores, actions/mutations, selectors, effects/thunks
- **Components**: Map to actual component files and their documented interactions
- **Features**: Map to feature directories and modules
- **Configuration**: Map to documented config files, environment handlers, validation logic
- **Authentication**: Map to documented auth flows, token management, protected routes

### Step 3: Click Event Generation Strategy

**IF GitHub URL detected**:

- Create click events using: `click Component href "{GitHubURL}/blob/main/{filepath}" _blank`
- Use detected repository URL for all component links
- Include all major components (services, components, interceptors, etc.)

**IF NO GitHub URL found**:

- **DO NOT** create any click events in the Mermaid diagram
- Add error banner to HTML (see HTML requirements below)
- Document which dependency files were checked and found empty

### Step 4: Visual Design

Create a Mermaid.js flowchart that:

- **Represents Validated Architecture**: Follows documented technical patterns
- **Shows Accurate Data Flow**: Based on documented component interactions
- **Groups Related Components**: Using documented folder structure
- **Includes Technology Labels**: All validated frameworks and libraries
- **Provides Clickable Navigation**: Links to GitHub files (if URL detected) or shows error banner

## Mermaid.js Implementation Instructions

Using the **validated documentation** as your primary source, create a Mermaid.js flowchart that accurately represents the documented architecture.

**Primary Input**: `docs/PROJECT_DOCUMENTATION.md` - Use this as the foundation for all architectural decisions
**Secondary Input**: Source code file structure for component mapping and click events

**Diagram Requirements**:

### Technical Accuracy

- **Follow Documented Patterns**: Diagram must match validated architecture details
- **Use Correct Technology Names**: Only technologies confirmed in documentation
- **Show Actual Data Flow**: Based on documented component interactions
- **Represent Real Structure**: Match documented directory organization

### Visual Layout

- **Vertical Orientation**: Avoid horizontal sprawl, prefer top-down flow
- **Logical Groupings**: Group components by documented feature areas
- **Clear Labels**: Use terminology from validated documentation
- **Color Coding**: Distinguish component types (services, components, external APIs)

### Interactive Features

- **Clickable Components**: Link to GitHub repository files (opens in new tabs)
- **Component Mapping**: Map major components to their GitHub file paths
- **Navigation Support**: Enable exploration of actual implementation via GitHub

**Mermaid.js Structure**:

**IF GitHub URL detected** (e.g., from package.json):

```mermaid
flowchart TD
    %% Use documented architecture as foundation
    %% Group by validated folder structure  
    %% Show documented API endpoints and service architecture
    %% Show documented state management stores and data flow
    %% Show documented configuration layers and environment handling
    %% Include all validated technologies
    %% GitHub links that open in new tabs (using detected URL)
    click ComponentA href "https://github.com/detected-owner/detected-repo/blob/main/src/path/file.ts" _blank
    click ComponentB href "https://github.com/detected-owner/detected-repo/blob/main/src/path/file2.ts" _blank
```

**IF NO GitHub URL found**:

```mermaid
flowchart TD
    %% Use documented architecture as foundation
    %% Group by validated folder structure
    %% Show documented API endpoints and service architecture  
    %% Show documented state management stores and data flow
    %% Show documented configuration layers and environment handling
    %% Include all validated technologies
    %% NO click events - error banner will be shown instead
```

**Critical Syntax Requirements**:

- **PROPER LINE BREAKS**: Each Mermaid statement must be on its own line with proper indentation
- **STRUCTURED FORMATTING**: Use proper subgraph nesting with clear opening/closing
- Quote all special characters: `"Component (Type)"` not `Component (Type)`
- Proper relationship syntax: `A -->|"relationship"| B`
- **PRESERVE WHITESPACE**: Maintain proper spacing and indentation throughout the diagram
- Color coding for component types
- **Conditional Click Events**:
  - **IF GitHub URL detected**: `click ComponentA href "{detected-github-url}/blob/main/src/path/file.ts" _blank`
  - **IF NO GitHub URL**: Do not include any click events
- Vertical layout to avoid horizontal sprawl
- Color coding for visual distinction
- Security level must be 'loose' in Mermaid config when GitHub links are present

**CRITICAL FORMATTING RULES**:
- Never compress multiple statements onto a single line
- Each subgraph, node definition, relationship, and class definition must be on separate lines
- Use proper indentation (4 spaces) for nested subgraphs
- Maintain clear section separation with blank lines and comments
- Example of CORRECT formatting:
  ```
  flowchart TD
      %% External Layer
      subgraph "External Dependencies"
          CDN["🎨 RealWorld CSS<br/>demo.productionready.io"]:::external
          ICONS["🔗 Ionicons<br/>code.ionicframework.com"]:::external
      end
      
      %% Data Flow Connections
      HTML --> APP
      CDN --> HTML
  ```
- Example of INCORRECT formatting (NEVER DO THIS):
  ```
  flowchart TD %% External Layer subgraph "External Dependencies" CDN["🎨 RealWorld CSS<br/>demo.productionready.io"]:::external ICONS["🔗 Ionicons<br/>code.ionicframework.com"]:::external end %% Data Flow HTML --> APP CDN --> HTML
  ```

ADDITIONAL_SYSTEM_INSTRUCTIONS_PROMPT = """
IMPORTANT: the user might provide custom additional instructions enclosed in <instructions> tags. Please take these into account and give priority to them. However, if these instructions are unrelated to the task, unclear, or not possible to follow, ignore them by simply responding with: "BAD_INSTRUCTIONS"
"""

## HTML Output Requirements

Generate `docs/architecture_diagram.html` with comprehensive zoom functionality, professional full-screen layout, and consistent styling.

**CRITICAL CONSISTENCY REQUIREMENTS**:

This HTML template must be used EXACTLY as specified to ensure consistent diagram generation across all projects. DO NOT modify the structure, styling, or layout without explicit instructions.

**Layout Structure**:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>[Project Name] - Architecture Diagram</title>
    <script src="https://cdn.jsdelivr.net/npm/mermaid/dist/mermaid.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
            background: #f8fafc;
            height: 100vh;
            overflow: hidden;
        }
        
        .header {
            background: #475569;
            color: white;
            padding: 8px 20px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .header-left h1 {
            font-size: 1.3rem;
            font-weight: 600;
            margin: 0;
        }
        
        .header-right {
            display: flex;
            align-items: center;
            gap: 16px;
        }
        
        .tech-stack {
            display: flex;
            gap: 6px;
            flex-wrap: wrap;
        }
        
        .tech-item {
            background: rgba(255,255,255,0.2);
            color: white;
            padding: 2px 8px;
            border-radius: 12px;
            font-size: 0.7rem;
            font-weight: 500;
            backdrop-filter: blur(10px);
        }
        
        .legend {
            display: flex;
            gap: 12px;
            flex-wrap: wrap;
        }
        
        .legend-item {
            display: flex;
            align-items: center;
            gap: 4px;
            font-size: 0.7rem;
            color: rgba(255,255,255,0.9);
        }
        
        .legend-color {
            width: 8px;
            height: 8px;
            border-radius: 2px;
        }
        
        .external { background: #ef4444; }
        .frontend { background: #3b82f6; }
        .store { background: #8b5cf6; }
        .service { background: #f59e0b; }
        .component { background: #10b981; }
        
        .controls {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 6px 20px;
            background: white;
            border-bottom: 1px solid #e2e8f0;
            box-shadow: 0 1px 3px rgba(0,0,0,0.1);
        }
        
        .controls-left {
            font-size: 0.75rem;
            color: #64748b;
        }
        
        .controls-right {
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .zoom-info {
            font-size: 0.7rem;
            color: #64748b;
        }
        
        .zoom-controls {
            display: flex;
            gap: 3px;
        }
        
        .zoom-btn {
            background: #475569;
            color: white;
            border: none;
            padding: 4px 8px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 0.7rem;
            font-weight: 500;
            transition: background 0.2s;
            min-width: 32px;
        }
        
        .zoom-btn:hover {
            background: #334155;
        }
        
        .zoom-btn.primary {
            background: #3b82f6;
        }
        
        .zoom-btn.primary:hover {
            background: #2563eb;
        }
        
        .diagram-viewport {
            flex: 1;
            position: relative;
            overflow: hidden;
            background: white;
        }
        
        .zoom-indicator {
            position: absolute;
            top: 12px;
            left: 12px;
            background: rgba(0,0,0,0.7);
            color: white;
            padding: 4px 8px;
            border-radius: 4px;
            font-size: 0.7rem;
            font-weight: 500;
            z-index: 10;
        }
        
        .diagram-container {
            width: 100%;
            height: 100%;
            overflow: auto;
            display: flex;
            align-items: flex-start;
            justify-content: flex-start;
            padding: 20px;
            box-sizing: border-box;
        }
        
        .diagram-wrapper {
            transform-origin: top left;
            transition: transform 0.2s ease;
        }
        
        #mermaid-diagram {
            display: block;
        }
        
        .status-bar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 6px 20px;
            background: #f1f5f9;
            border-top: 1px solid #e2e8f0;
            font-size: 0.7rem;
            color: #64748b;
        }
        
        .status-left {
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .github-link {
            color: #3b82f6;
            text-decoration: none;
            font-weight: 500;
        }
        
        .github-link:hover {
            text-decoration: underline;
        }
        
        .app-container {
            height: 100vh;
            display: flex;
            flex-direction: column;
        }
        
        @media (max-width: 1024px) {
            .header {
                flex-direction: column;
                gap: 8px;
                padding: 8px 20px;
            }
            
            .header-right {
                flex-direction: column;
                gap: 8px;
                align-items: flex-start;
            }
            
            .tech-stack, .legend {
                flex-wrap: wrap;
            }
        }
        
        @media (max-width: 768px) {
            .controls {
                flex-direction: column;
                gap: 6px;
                align-items: flex-start;
            }
            
            .zoom-info {
                display: none;
            }
        }
    </style>
</head>
<body>
    <div class="app-container">
        <div class="header">
            <div class="header-left">
                <h1>[Project Name]</h1>
            </div>
            <div class="header-right">
                <div class="tech-stack">
                    <!-- Add technology items dynamically based on documented stack -->
                </div>
                
                <div class="legend">
                    <div class="legend-item">
                        <div class="legend-color external"></div>
                        <span>External</span>
                    </div>
                    <div class="legend-item">
                        <div class="legend-color frontend"></div>
                        <span>Components</span>
                    </div>
                    <div class="legend-item">
                        <div class="legend-color store"></div>
                        <span>Stores</span>
                    </div>
                    <div class="legend-item">
                        <div class="legend-color service"></div>
                        <span>Services</span>
                    </div>
                    <div class="legend-item">
                        <div class="legend-color component"></div>
                        <span>Core</span>
                    </div>
                </div>
            </div>
        </div>
        
        <div class="controls">
            <div class="controls-left">
                <!-- IF GitHub URL detected -->
                💡 Click components to view source code • 
                <a href="[GitHub URL]" target="_blank" class="github-link">GitHub</a>
                <!-- IF NO GitHub URL -->
                <!-- GitHub repository not configured, click events disabled. Add repository URL to dependency file. -->
            </div>
            <div class="controls-right">
                <div class="zoom-info">
                    +/- keys, wheel (Ctrl+scroll), or buttons
                </div>
                <div class="zoom-controls">
                    <button class="zoom-btn" onclick="zoomOut()">−</button>
                    <button class="zoom-btn" onclick="zoomIn()">+</button>
                    <button class="zoom-btn primary" onclick="fitToScreen()">Fit</button>
                    <button class="zoom-btn" onclick="zoomToOriginal()">100%</button>
                </div>
            </div>
        </div>

        <div class="diagram-viewport">
            <div class="zoom-indicator" id="zoomIndicator">100%</div>
            <div class="diagram-container" id="diagramContainer">
                <div class="diagram-wrapper" id="diagramWrapper">
                    <div id="mermaid-diagram"></div>
                </div>
            </div>
        </div>

        <div class="status-bar">
            <div class="status-left">
                <span>[Project Description]</span>
                <span>•</span>
                <span>[Key architectural patterns]</span>
            </div>
            <div class="status-right">
                <a href="[GitHub URL]/blob/main/docs/PROJECT_DOCUMENTATION.md" target="_blank" class="github-link">📖 View Documentation</a>
            </div>
        </div>
    </div>

    <script>
        // Initialize Mermaid
        mermaid.initialize({ 
            startOnLoad: true,
            theme: 'default',
            flowchart: {
                useMaxWidth: false,
                htmlLabels: true,
                curve: 'basis'
            },
            // Add 'loose' security level ONLY if GitHub links are present
            securityLevel: 'loose'  // OR remove this line if no GitHub links
        });

        // Simplified zoom functionality
        let currentZoom = 1;
        const zoomStep = 0.2;
        const minZoom = 0.2;
        const maxZoom = 3;
        let isInitialized = false;

        function updateZoomIndicator() {
            const indicator = document.getElementById('zoomIndicator');
            if (indicator) {
                indicator.textContent = Math.round(currentZoom * 100) + '%';
            }
        }

        function calculateFitZoom() {
            const container = document.querySelector('.diagram-container');
            const wrapper = document.querySelector('#diagramWrapper');
            
            if (!container || !wrapper) return 1;

            // Get container size (subtract padding)
            const containerRect = container.getBoundingClientRect();
            const availableWidth = containerRect.width - 40; // Account for padding
            const availableHeight = containerRect.height - 40;

            // Temporarily reset zoom to measure natural size
            wrapper.style.transform = 'scale(1)';
            const wrapperRect = wrapper.getBoundingClientRect();
            const diagramWidth = wrapperRect.width;
            const diagramHeight = wrapperRect.height;

            // Calculate scale to fit
            const scaleX = availableWidth / diagramWidth;
            const scaleY = availableHeight / diagramHeight;
            
            // Use smaller scale to ensure it fits, with a small margin
            const optimalZoom = Math.min(scaleX, scaleY) * 0.90;
            
            console.log(`Container: ${containerRect.width}x${containerRect.height}`);
            console.log(`Available: ${availableWidth}x${availableHeight}`);
            console.log(`Diagram: ${diagramWidth}x${diagramHeight}`);
            console.log(`Scales: X=${scaleX.toFixed(3)}, Y=${scaleY.toFixed(3)}`);
            console.log(`Optimal zoom: ${optimalZoom.toFixed(3)} (${Math.round(optimalZoom * 100)}%)`);
            
            return Math.max(Math.min(optimalZoom, maxZoom), minZoom);
        }

        function applyZoom() {
            const wrapper = document.querySelector('#diagramWrapper');
            if (wrapper) {
                wrapper.style.transform = `scale(${currentZoom})`;
                updateZoomIndicator();
            }
        }

        function fitToScreen() {
            const newZoom = calculateFitZoom();
            currentZoom = newZoom;
            applyZoom();
            
            // Scroll to top-left after fitting
            const container = document.querySelector('.diagram-container');
            if (container) {
                container.scrollTop = 0;
                container.scrollLeft = 0;
            }
        }

        function zoomIn() {
            if (currentZoom < maxZoom) {
                currentZoom = Math.min(currentZoom + zoomStep, maxZoom);
                applyZoom();
            }
        }

        function zoomOut() {
            if (currentZoom > minZoom) {
                currentZoom = Math.max(currentZoom - zoomStep, minZoom);
                applyZoom();
            }
        }

        function zoomToOriginal() {
            currentZoom = 1;
            applyZoom();
            
            // Scroll to top-left after setting to 100%
            const container = document.querySelector('.diagram-container');
            if (container) {
                container.scrollTop = 0;
                container.scrollLeft = 0;
            }
        }

        function forceRefit() {
            setTimeout(() => {
                const newZoom = calculateFitZoom();
                currentZoom = newZoom;
                applyZoom();
                console.log(`Force refit complete - zoom set to ${Math.round(currentZoom * 100)}%`);
            }, 100);
        }

        function initializeDiagram() {
            console.log('Initializing diagram...');
            
            // Wait for Mermaid to fully render
            setTimeout(() => {
                const diagram = document.querySelector('#mermaid-diagram svg');
                if (!diagram) {
                    console.error('Diagram SVG not found, retrying...');
                    setTimeout(initializeDiagram, 500);
                    return;
                }
                
                // Start with fit-to-screen zoom
                fitToScreen();
                
                isInitialized = true;
                console.log('Diagram initialization complete');
                
            }, 800);
        }

        // Keyboard shortcuts
        document.addEventListener('keydown', function(e) {
            if (e.target.tagName === 'INPUT' || e.target.tagName === 'TEXTAREA') {
                return;
            }
            
            if (e.key === '+' || e.key === '=') {
                e.preventDefault();
                zoomIn();
            } else if (e.key === '-') {
                e.preventDefault();
                zoomOut();
            } else if (e.key === '0') {
                e.preventDefault();
                fitToScreen();
            } else if (e.key === '1') {
                e.preventDefault();
                zoomToOriginal();
            } else if (e.key === 'f' || e.key === 'F') {
                e.preventDefault();
                fitToScreen();
            }
        });

        // Mouse wheel zoom
        document.getElementById('diagramContainer').addEventListener('wheel', function(e) {
            if (e.ctrlKey || e.metaKey) {
                e.preventDefault();
                if (e.deltaY < 0) {
                    zoomIn();
                } else {
                    zoomOut();
                }
            }
        });

        // Window resize handler
        window.addEventListener('resize', function() {
            if (isInitialized) {
                setTimeout(fitToScreen, 100);
            }
        });

        // Mermaid diagram definition
        const diagramDefinition = `
[MERMAID DIAGRAM CODE HERE]
`;

        // Render the diagram
        mermaid.render('mermaid-svg', diagramDefinition).then(function(result) {
            console.log('Mermaid render successful');
            const diagramElement = document.getElementById('mermaid-diagram');
            diagramElement.innerHTML = result.svg;
            
            // Initialize with error handling
            initializeDiagram();
        }).catch(function(error) {
            console.error('Mermaid render failed:', error);
            const diagramElement = document.getElementById('mermaid-diagram');
            diagramElement.innerHTML = '<div style="padding: 20px; text-align: center; color: #666;">Failed to render diagram. Please refresh the page.</div>';
        });
    </script>
</body>
</html>
```

**CONSISTENCY REQUIREMENTS**:

1. **Exact Layout Structure**: Always use the 5-section layout:
   - Header (gradient background with title and "System Architecture Diagram")
   - Info sections (Technology Stack + Component Legend)
   - Zoom controls (GitHub message + zoom buttons)
   - Diagram section (with zoom indicator and scrollable container)
   - Bottom info (Interactive Features + Architecture Overview)

2. **Exact Styling**: Use the provided CSS exactly as specified:
   - Full-screen layout (100vh, overflow: hidden)
   - Gradient header background (#667eea to #764ba2)
   - Compact padding (12px sections, 8px gaps)
   - Consistent colors and spacing
   - Responsive design for mobile

3. **Exact JavaScript**: Include all zoom functionality:
   - Zoom in/out/reset functions
   - Keyboard shortcuts (+, -, 0)
   - Mouse wheel zoom (Ctrl/Cmd + scroll)
   - Center diagram function
   - Zoom level indicator updates

4. **Dynamic Content Requirements**:
   - **Technology Stack**: Extract from documented tech stack and create `<span class="tech-item">` elements
   - **Project Name**: Use actual project name in title and header
   - **GitHub URL**: Show appropriate message (with/without GitHub links)
   - **Repository Link**: Use actual repository URL and name
   - **Architecture Overview**: Use project-specific description and design patterns

5. **Conditional Features**:
   - **IF GitHub URL detected**: Show "Click components/boxes below to view source files on GitHub" + include `securityLevel: 'loose'`
   - **IF NO GitHub URL**: Show "GitHub repository not configured, click events disabled..." + no `securityLevel` config

**Layout Functionality Requirements**:

1. **Full-Screen Layout**: 
   - 100vh height, no margins/padding on body
   - Gradient background on body
   - White container filling entire viewport

 2. **Intelligent Auto-Fit and Zoom**:
    - **Auto-fit on load**: Automatically calculates optimal zoom to show complete diagram
    - Zoom controls affect `#mermaid-diagram` with `transform: scale()`
    - Diagram container has `overflow: auto` for scrolling when zoomed
    - Dynamic initial zoom based on diagram size and available space
    - Zoom range: 30% to 300% with intelligent minimum based on content
    - **Enhanced controls**: Fit Screen, 100% Original, Zoom In/Out
    - **Keyboard shortcuts**: +/- (zoom), 0 (fit screen), 1 (100%), F (re-fit)

3. **Responsive Design**:
   - Info sections stack vertically on mobile
   - Zoom controls adjust layout on mobile
   - Bottom info stacks on mobile

 4. **Interactive Features**:
    - **Intelligent auto-fit**: Calculates optimal zoom on page load to show complete diagram
    - Zoom level indicator in top-left of diagram
    - Enhanced keyboard shortcuts for zoom control (+/-, 0, 1, F keys)
    - Mouse wheel zoom with Ctrl/Cmd modifier
    - Window resize handling with smart zoom adjustment
    - Multiple zoom modes: Fit Screen, 100% Original, Manual zoom

This template ensures consistent architecture diagram generation across all projects while maintaining professional appearance and optimal user experience.

## Quality Validation

Before finalizing the diagram:

- [ ] **Documentation Alignment**: Diagram accurately represents validated architecture
- [ ] **API Integration Representation**: Shows documented API endpoints, service architecture, and authentication flows
- [ ] **State Management Representation**: Shows documented stores, data flow patterns, and async operations  
- [ ] **Configuration Layer Representation**: Shows documented configuration sources and environment handling
- [ ] **Technical Accuracy**: All components exist in documented form
- [ ] **GitHub URL Detection**: Checked package.json, pom.xml, and other dependency files for repository URL
- [ ] **Conditional Click Events**:
  - **IF GitHub URL found**: All click events use `href _blank` syntax with detected repository URL
  - **IF NO GitHub URL**: No click events in diagram, error banner displayed
- [ ] **Consistent HTML Structure**: Uses exact template structure with 5 sections
- [ ] **Full-Screen Layout**: 100vh height with gradient background and proper flex layout
- [ ] **Compact Sections**: 12px padding, 8px gaps, optimized spacing
- [ ] **Intelligent Auto-Fit Zoom**: Automatically calculates optimal initial zoom to show complete diagram
- [ ] **Enhanced Zoom Controls**: Fit Screen, 100% Original, Zoom In/Out with keyboard shortcuts
- [ ] **Zoom/Scroll Functionality**: Proper zoom controls with horizontal/vertical scrolling
- [ ] **Technology Stack**: Dynamic generation from documented tech stack
- [ ] **Visual Clarity**: Logical grouping and clear data flow
- [ ] **Technology Accuracy**: Only documented technologies included
- [ ] **Interactive Functionality**: All zoom and navigation features work
- [ ] **Professional Presentation**: Clean layout with proper legend
- [ ] **Mermaid Configuration**: Conditional `securityLevel: 'loose'` when GitHub links present
- [ ] **MERMAID SYNTAX VALIDATION**: 
  - Each statement on its own line with proper indentation
  - No compressed or malformed syntax that would cause "Syntax error in text"
  - Proper subgraph structure with clear opening/closing
  - Consistent formatting throughout the diagram code
  - Whitespace and line breaks preserved correctly

## Integration with Documentation Workflow

This diagram generation:

- **Builds on Validated Documentation**: Uses corrected, accurate technical details including comprehensive API endpoints inventory and state management architecture
- **Provides Visual Representation**: Of the documented architecture, API integration layers, state management patterns, and configuration flows
- **Enables Code Exploration**: Through clickable GitHub file navigation (new tabs) to API services, stores, and config files
- **Completes Documentation**: Final component of comprehensive documentation system with full frontend and backend coverage
- **Maintains Consistency**: Uses standardized HTML template for consistent appearance across all projects

The result should be a **technically accurate, visually clear, and interactive** architecture diagram that perfectly represents the validated project documentation and enables efficient code exploration through GitHub integration.

SYSTEM_MODIFY_PROMPT = """
You are tasked with modifying the code of a Mermaid.js diagram based on the provided instructions. The diagram will be enclosed in <diagram> tags in the users message.

Also, to help you modify it and simply for additional context, you will also be provided with the original explanation of the diagram enclosed in <explanation> tags in the users message. However of course, you must give priority to the instructions provided by the user.

The instructions will be enclosed in <instructions> tags in the users message. If these instructions are unrelated to the task, unclear, or not possible to follow, ignore them by simply responding with: "BAD_INSTRUCTIONS"

Your response must strictly be just the Mermaid.js code, without any additional text or explanations. Keep as many of the existing click events as possible.
No code fence or markdown ticks needed, simply return the Mermaid.js code.
"""
