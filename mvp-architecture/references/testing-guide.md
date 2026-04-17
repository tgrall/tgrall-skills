# Testing guide

Comprehensive testing strategy for the MVP, covering unit, integration, and e2e tests.

## Testing philosophy

- Test behavior, not implementation details
- Prioritize tests that catch real bugs over tests that just increase coverage numbers
- E2e tests for critical user flows; unit tests for logic and component contracts
- Every test should have a clear reason to exist

## Tool setup

### Vitest configuration

```ts
// vitest.config.ts
import { defineConfig } from "vitest/config";
import react from "@vitejs/plugin-react";
import path from "path";

export default defineConfig({
  plugins: [react()],
  test: {
    environment: "jsdom",
    globals: true,
    setupFiles: ["./src/__tests__/setup.ts"],
    include: ["src/__tests__/**/*.test.{ts,tsx}"],
    coverage: {
      provider: "v8",
      reporter: ["text", "html", "lcov"],
      include: ["src/**/*.{ts,tsx}"],
      exclude: [
        "src/__tests__/**",
        "src/components/ui/**",
        "**/*.d.ts",
      ],
    },
  },
  resolve: {
    alias: {
      "@": path.resolve(__dirname, "./src"),
    },
  },
});
```

### Test setup file

```ts
// src/__tests__/setup.ts
import "@testing-library/jest-dom/vitest";
```

### Playwright configuration

```ts
// playwright.config.ts
import { defineConfig, devices } from "@playwright/test";

export default defineConfig({
  testDir: "./e2e",
  fullyParallel: true,
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? 2 : 0,
  workers: process.env.CI ? 1 : undefined,
  reporter: [["html"], ["list"]],
  use: {
    baseURL: "http://localhost:3000",
    trace: "on-first-retry",
    screenshot: "only-on-failure",
  },
  projects: [
    {
      name: "chromium",
      use: { ...devices["Desktop Chrome"] },
    },
    {
      name: "firefox",
      use: { ...devices["Desktop Firefox"] },
    },
    {
      name: "mobile-chrome",
      use: { ...devices["Pixel 5"] },
    },
  ],
  webServer: {
    command: "pnpm dev",
    url: "http://localhost:3000",
    reuseExistingServer: !process.env.CI,
  },
});
```

## Test file structure

```
src/__tests__/
├── unit/
│   ├── components/
│   │   ├── [ComponentName].test.tsx
│   │   └── ...
│   ├── hooks/
│   │   └── [hookName].test.ts
│   └── lib/
│       └── [utilName].test.ts
├── integration/
│   ├── [feature-flow].test.tsx
│   └── api/
│       └── [route].test.ts
├── fixtures/
│   └── [entity].fixtures.ts
└── setup.ts

e2e/
├── [user-flow].spec.ts
├── fixtures/
│   └── [test-data].ts
└── pages/
    └── [PageName].page.ts
```

## Naming conventions

- Unit tests: `[ComponentName].test.tsx` or `[utilName].test.ts`
- Integration tests: `[feature-flow].test.tsx`
- E2E tests: `[user-flow].spec.ts`
- Fixtures: `[entity].fixtures.ts`
- Page objects: `[PageName].page.ts`

## Unit test patterns

### Component test example

```tsx
// src/__tests__/unit/components/NoteCard.test.tsx
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import { NoteCard } from "@/components/shared/NoteCard";
import { mockNote } from "../../fixtures/note.fixtures";

describe("NoteCard", () => {
  it("renders the note title and preview", () => {
    render(<NoteCard note={mockNote} />);

    expect(screen.getByText(mockNote.title)).toBeInTheDocument();
    expect(screen.getByText(mockNote.preview)).toBeInTheDocument();
  });

  it("calls onPin when the pin button is clicked", async () => {
    const user = userEvent.setup();
    const onPin = vi.fn();

    render(<NoteCard note={mockNote} onPin={onPin} />);

    await user.click(screen.getByRole("button", { name: /pin/i }));
    expect(onPin).toHaveBeenCalledWith(mockNote.id);
  });
});
```

### Hook test example

```ts
// src/__tests__/unit/hooks/useDebounce.test.ts
import { renderHook, act } from "@testing-library/react";
import { useDebounce } from "@/hooks/useDebounce";

describe("useDebounce", () => {
  beforeEach(() => { vi.useFakeTimers(); });
  afterEach(() => { vi.restoreAllTimers(); });

  it("returns the initial value immediately", () => {
    const { result } = renderHook(() => useDebounce("hello", 300));
    expect(result.current).toBe("hello");
  });

  it("updates the value after the delay", () => {
    const { result, rerender } = renderHook(
      ({ value }) => useDebounce(value, 300),
      { initialProps: { value: "hello" } }
    );

    rerender({ value: "world" });
    expect(result.current).toBe("hello");

    act(() => { vi.advanceTimersByTime(300); });
    expect(result.current).toBe("world");
  });
});
```

### Utility test example

```ts
// src/__tests__/unit/lib/formatDate.test.ts
import { formatRelativeDate } from "@/lib/utils";

describe("formatRelativeDate", () => {
  it("returns 'just now' for dates within the last minute", () => {
    const now = new Date();
    expect(formatRelativeDate(now)).toBe("just now");
  });

  it("returns '2 hours ago' for dates 2 hours in the past", () => {
    const twoHoursAgo = new Date(Date.now() - 2 * 60 * 60 * 1000);
    expect(formatRelativeDate(twoHoursAgo)).toBe("2 hours ago");
  });
});
```

## Integration test patterns

### API route test example

```ts
// src/__tests__/integration/api/notes.test.ts
import { GET, POST } from "@/app/api/notes/route";
import { NextRequest } from "next/server";

describe("GET /api/notes", () => {
  it("returns all notes", async () => {
    const request = new NextRequest("http://localhost:3000/api/notes");
    const response = await GET(request);
    const data = await response.json();

    expect(response.status).toBe(200);
    expect(Array.isArray(data)).toBe(true);
  });
});

describe("POST /api/notes", () => {
  it("creates a new note and returns it", async () => {
    const request = new NextRequest("http://localhost:3000/api/notes", {
      method: "POST",
      body: JSON.stringify({ title: "Test Note", content: "" }),
    });
    const response = await POST(request);
    const data = await response.json();

    expect(response.status).toBe(201);
    expect(data.title).toBe("Test Note");
  });
});
```

## E2E test patterns

### Page Object Model

```ts
// e2e/pages/Dashboard.page.ts
import { type Page, type Locator } from "@playwright/test";

export class DashboardPage {
  readonly page: Page;
  readonly sidebar: Locator;
  readonly notesList: Locator;
  readonly newNoteButton: Locator;
  readonly searchInput: Locator;

  constructor(page: Page) {
    this.page = page;
    this.sidebar = page.getByRole("navigation");
    this.notesList = page.getByTestId("notes-list");
    this.newNoteButton = page.getByRole("button", { name: /new note/i });
    this.searchInput = page.getByPlaceholder(/search/i);
  }

  async goto() {
    await this.page.goto("/en/dashboard");
  }

  async createNote(title: string) {
    await this.newNoteButton.click();
    await this.page.getByPlaceholder(/untitled/i).fill(title);
  }

  async searchFor(query: string) {
    await this.searchInput.fill(query);
  }
}
```

### E2E test example

```ts
// e2e/note-management.spec.ts
import { test, expect } from "@playwright/test";
import { DashboardPage } from "./pages/Dashboard.page";

test.describe("Note management", () => {
  let dashboard: DashboardPage;

  test.beforeEach(async ({ page }) => {
    dashboard = new DashboardPage(page);
    await dashboard.goto();
  });

  test("user can create a new note", async ({ page }) => {
    await dashboard.createNote("My Test Note");

    await expect(
      page.getByRole("heading", { name: "My Test Note" })
    ).toBeVisible();
  });

  test("user can search for notes", async ({ page }) => {
    await dashboard.searchFor("test");

    await expect(dashboard.notesList).toContainText("test");
  });
});
```

### E2E fixture pattern

```ts
// e2e/fixtures/test-data.ts
export const testNote = {
  title: "E2E Test Note",
  content: "This note was created by an automated test.",
};

export const testNotebook = {
  name: "E2E Test Notebook",
};
```

## Coverage targets

| Test type | Target | Rationale |
|---|---|---|
| **Unit** | 80%+ line coverage on `src/lib/` and `src/hooks/` | Logic and utilities should be well-tested |
| **Unit (components)** | Key interactive components | Focus on behavior: clicks, form submission, conditional rendering |
| **Integration** | All API routes and server actions | Data flow correctness |
| **E2E** | Every must-have user flow from the PRD | Confidence that the product works end-to-end |

## CI integration

```yaml
# Example GitHub Actions snippet
- name: Unit & Integration Tests
  run: pnpm test -- --coverage

- name: E2E Tests
  run: pnpm exec playwright install --with-deps && pnpm test:e2e
```

## Package.json scripts

```json
{
  "scripts": {
    "test": "vitest run",
    "test:watch": "vitest",
    "test:coverage": "vitest run --coverage",
    "test:e2e": "playwright test",
    "test:e2e:ui": "playwright test --ui"
  }
}
```
