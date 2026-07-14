# AI Native Engineering Workspace — UI Design Prompt for Gemini AI Studio

You are a world-class UX/UI designer specializing in modern SaaS developer tools. Your task is to design and build a complete, production-quality web application UI for an **AI Native Engineering Workspace** — an internal R&D management platform for a small team (6 people).

## Product Context

This is a project management tool similar to **Linear**, **GitHub Issues**, and **Notion** — designed to replace messy spreadsheets. The team currently manages bugs, features, tasks, and improvements using online Excel-like documents. Everything is just a row in a table. This tool should give each issue a proper lifecycle.

## Design Philosophy

The absolute highest priority reference is **Linear.app** — study its design language carefully:

- **Minimalist**: Extreme simplicity. Every element must earn its place.
- **Clean**: Generous whitespace, breathing room between elements.
- **Professional**: Blue-white-gray palette, card-based layout, rounded corners (8-12px).
- **Lightweight**: No heavy animations, no complex gradients, no unnecessary interactions.
- **Modern SaaS**: Think Stripe, Linear, Vercel, GitHub — not old enterprise software.

**DO NOT:**
- Flashy animations or complex CSS transitions
- Colorful gradients or neon effects
- Cluttered layouts or high information density
- Unnecessary decorative elements
- Over-designed components

**DO:**
- Subtle hover state changes (background color shift only)
- Clean border separators (1px, light gray)
- Crisp typography using system fonts (Inter/SF Pro)
- Clear visual hierarchy through spacing and font weight
- Card components with soft shadows (or just border outlines)

## Color Palette

**Light Mode:**
- Background: `#FAFAFA` or `#F8F9FA`
- Card Surface: `#FFFFFF`
- Primary Text: `#1A1A2E` (near black)
- Secondary Text: `#6B7280` (medium gray)
- Borders: `#E5E7EB` (light gray)
- Primary Accent: `#5E6AD2` (Linear-style purplish blue)
- Success: `#0F783E` (dark green)
- Warning: `#B54708` (dark orange)
- Error/Danger: `#E5484D` (red)
- Feature Purple: `#7C3AED`
- Info Blue: `#2563EB`

**Dark Mode (also build this, with toggle):**
- Background: `#0D0D0D`
- Card Surface: `#1A1A1A`
- Primary Text: `#ECEDEE`
- Secondary Text: `#878787`
- Borders: `#2E2E2E`
- Primary Accent: `#6C7CFC`

## Font & Typography
- Font family: System font stack (`-apple-system, BlinkMacSystemFont, 'Segoe UI', 'Inter', Roboto, sans-serif`)
- Base size: 14px
- Headings: 22px (page title), 16px (section title)
- Small labels/badges: 11-12px

## Pages to Build

### 1. Dashboard Page (`/`)
The main landing page after login. Should feel like an overview, not a report.

**Layout:**
- **Left Sidebar** (220px, fixed):
  - Logo + "AI Workspace" text at top (with a small colored icon/dot)
  - Nav items: Dashboard (active), Issues, Board — with simple emoji or SVG icons
  - Projects section: "Certification System" and "AI Agent" as list items with colored dots
  - User avatar + name at bottom
  - **Important**: Sidebar should look premium — subtle borders, clean typography, good spacing

- **Main Content Area:**
  - Header: Page title "Dashboard" on left, "New Issue" button (primary) + filter button on right
  - **Stats Bar** (4 cards in a row):
    - Total Issues: 23
    - Bugs: 5
    - Features: 8
    - Completed This Week: 4
    - Each card: light card background, large number with color accent, small gray label below
  - **Content Grid** (2 columns):
    - Left column (wider): "My Tasks" card — list of 3 issues showing: type badge (Bug/Feature), title, priority badge, assignee avatar
    - Right column: "Status Distribution" card — list of statuses with colored dots and counts
    - Row 2: "Pending" card + "Recently Completed" card

### 2. Issue List Page (`/issues`)
All issues in a clean table-like list.

**Layout:**
- Header with project selector dropdown, search bar, and filter buttons (Status, Type, Assignee, Priority)
- View toggle: List / Board (tabs)
- **List View**: Clean table rows showing:
  - Type icon (Bug=red dot, Feature=purple diamond, Task=blue square, Improvement=green triangle)
  - Issue title (clickable)
  - Priority badge (Urgent=red, High=orange, Medium=gray, Low=gray)
  - Assignee avatar (small circle with initial, or "?" if unassigned)
  - Status badge (pill-shaped colored label)
  - Created date
- Hover: subtle row background change
- Bottom: pagination

### 3. Issue Detail Page (`/issues/:id`)
The most important page — where team members spend most time.

**Layout (GitHub Issue style):**
- **Top**: Issue title (#12), Edit and Close buttons
- **Two-column layout** (70/30 split):
  - **Left (70%)**:
    - Author avatar + name + creation date
    - Rich Markdown description area (well-rendered, clean typography)
    - Embedded screenshots displayed nicely
    - Attached files shown as file cards
    - Comment thread below with separator lines
    - Each comment: avatar, name, date, content
    - Comment input box at bottom with Markdown hint
  - **Right (30%)** — Metadata Sidebar:
    - Status dropdown (colored pill)
    - Assignee dropdown with avatar
    - Priority dropdown
    - Type (Bug/Feature/Task/Improvement)
    - Labels (color-coded tags, add/remove)
    - Project name
    - Module/Category
    - Created date, Updated date
    - Reporter (who created it)

### 4. Create Issue Modal
A centered modal/overlay that appears when clicking "New Issue":
- Project selector dropdown at top
- Title input (large, borderless until focused — Linear style)
- Description textarea with Markdown toolbar hints (Bold, Italic, Code, List, Quote)
- Drag & drop zone for screenshots and attachments
- Bottom row: Type dropdown, Priority dropdown, Labels input, Assignee dropdown (optional)
- Cancel + Create buttons
- Support Ctrl+Enter to submit

## Component Style Guide

### Buttons
- Primary: Solid background with primary color, white text, 6-8px border-radius
- Secondary/Ghost: Transparent with 1px border, gray text
- Icon-only: 32x32px square, subtle border
- Danger: Red background for destructive actions
- All buttons: smooth 150ms transitions, slightly darken on hover

### Badges / Pills
- Issue type badges: colored background (10% opacity) + colored text, small rounded rectangle
- Priority badges: similar style, urgent=red, high=orange, medium=gray
- Status badges: pill-shaped (fully rounded), distinct colors per status
- Rounded: 4-6px for badges, 20px for status pills

### Cards
- White background with 1px light border
- 10-12px border-radius
- Optional: very subtle box-shadow (0 1px 3px rgba(0,0,0,0.04))
- Internal padding: 16-20px

### Status Colors (for dots/badges)
- New: Gray (#9CA3AF)
- Suggested: Amber (#F59E0B)
- Accepted: Blue (#3B82F6)
- In Progress: Primary (#5E6AD2)
- Ready for Test: Purple (#8B5CF6)
- Testing: Amber (#F59E0B)
- Done: Green (#0F783E)

### Avatar Circles
- 24-28px circles with user initials
- Background: light primary color, text: primary color
- Unassigned: gray circle with "?"

## Interaction Details

- All interactive elements should have hover states
- Dropdowns should feel smooth and modern (slide down with subtle animation)
- Page transitions should be instant (no SPA route animations)
- Modal: fade in backdrop, slide up modal
- Keyboard shortcuts welcome (e.g., Cmd+K for command palette)
- Dark mode toggle in header area

## Technical Notes

- Use pure HTML + CSS + minimal JavaScript (no frameworks needed)
- Make it fully responsive (but desktop-first)
- Support both light and dark modes with a toggle
- Use CSS custom properties for easy theming
- All text should be in Chinese where user-facing (but English for code/technical labels is fine)

## Deliverables

Build ALL pages as a single-page application (SPA) or multi-page HTML. Focus on making it look **stunning** — this is a design prototype, so visual quality is the top priority. Every pixel should feel intentional. The UI should feel like you're using Linear or a premium SaaS product.

---

**Key Reference Products (study their design):**
1. Linear.app — #1 reference for layout, spacing, simplicity
2. GitHub Issues — reference for issue detail page layout
3. Notion — reference for clean typography and document feel
4. shadcn/ui — reference for component patterns