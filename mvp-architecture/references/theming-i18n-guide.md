# Theming and i18n guide

Implementation patterns for multi-color theming, dark mode, and internationalization.

## Multi-color theming

### Strategy

Use CSS custom properties (variables) defined in `globals.css`. Each theme is a set of variable overrides applied via a `data-theme` attribute on the root element. Dark mode is handled via the `dark` class (Tailwind convention).

This approach:

- works with ShadCN out of the box
- requires no runtime JavaScript for color resolution
- supports any number of color themes
- composes cleanly with dark mode (each theme has light + dark variants)

### Color variable structure

Define colors in HSL format so Tailwind can apply opacity modifiers.

```css
/* src/app/globals.css */

@layer base {
  :root {
    --background: 0 0% 100%;
    --foreground: 240 10% 3.9%;
    --primary: 240 5.9% 10%;
    --primary-foreground: 0 0% 98%;
    --secondary: 240 4.8% 95.9%;
    --secondary-foreground: 240 5.9% 10%;
    --accent: 240 4.8% 95.9%;
    --accent-foreground: 240 5.9% 10%;
    --muted: 240 4.8% 95.9%;
    --muted-foreground: 240 3.8% 46.1%;
    --destructive: 0 84.2% 60.2%;
    --border: 240 5.9% 90%;
    --ring: 240 5.9% 10%;
    --radius: 0.5rem;
  }

  .dark {
    --background: 240 10% 3.9%;
    --foreground: 0 0% 98%;
    --primary: 0 0% 98%;
    --primary-foreground: 240 5.9% 10%;
    --secondary: 240 3.7% 15.9%;
    --secondary-foreground: 0 0% 98%;
    --accent: 240 3.7% 15.9%;
    --accent-foreground: 0 0% 98%;
    --muted: 240 3.7% 15.9%;
    --muted-foreground: 240 5% 64.9%;
    --destructive: 0 62.8% 30.6%;
    --border: 240 3.7% 15.9%;
    --ring: 240 4.9% 83.9%;
  }

  /* Blue theme */
  [data-theme="blue"] {
    --primary: 221 83% 53%;
    --primary-foreground: 210 40% 98%;
    --accent: 210 40% 96%;
    --ring: 221 83% 53%;
  }

  [data-theme="blue"].dark {
    --primary: 217 91% 60%;
    --primary-foreground: 222 47% 11%;
    --accent: 217 33% 17%;
    --ring: 217 91% 60%;
  }

  /* Green theme */
  [data-theme="green"] {
    --primary: 142 76% 36%;
    --primary-foreground: 356 29% 98%;
    --accent: 138 76% 97%;
    --ring: 142 76% 36%;
  }

  [data-theme="green"].dark {
    --primary: 142 71% 45%;
    --primary-foreground: 144 80% 10%;
    --accent: 144 31% 17%;
    --ring: 142 71% 45%;
  }
}
```

### Theme provider

```tsx
// src/components/shared/ThemeProvider.tsx
"use client";

import { createContext, useContext, useEffect, useState } from "react";

type Theme = "default" | "blue" | "green" | "purple";
type Mode = "light" | "dark" | "system";

interface ThemeContextType {
  theme: Theme;
  mode: Mode;
  setTheme: (theme: Theme) => void;
  setMode: (mode: Mode) => void;
}

const ThemeContext = createContext<ThemeContextType | undefined>(undefined);

export function ThemeProvider({ children }: { children: React.ReactNode }) {
  const [theme, setTheme] = useState<Theme>("default");
  const [mode, setMode] = useState<Mode>("system");

  useEffect(() => {
    const root = document.documentElement;

    // Apply theme
    root.setAttribute("data-theme", theme === "default" ? "" : theme);

    // Apply mode
    if (mode === "system") {
      const prefersDark = window.matchMedia("(prefers-color-scheme: dark)").matches;
      root.classList.toggle("dark", prefersDark);
    } else {
      root.classList.toggle("dark", mode === "dark");
    }

    // Persist
    localStorage.setItem("theme", theme);
    localStorage.setItem("mode", mode);
  }, [theme, mode]);

  // Hydrate from localStorage on mount
  useEffect(() => {
    const savedTheme = localStorage.getItem("theme") as Theme | null;
    const savedMode = localStorage.getItem("mode") as Mode | null;
    if (savedTheme) setTheme(savedTheme);
    if (savedMode) setMode(savedMode);
  }, []);

  return (
    <ThemeContext.Provider value={{ theme, mode, setTheme, setMode }}>
      {children}
    </ThemeContext.Provider>
  );
}

export function useTheme() {
  const context = useContext(ThemeContext);
  if (!context) throw new Error("useTheme must be used within ThemeProvider");
  return context;
}
```

### Theme switcher component

```tsx
// src/components/shared/ThemeSwitcher.tsx
"use client";

import { useTheme } from "./ThemeProvider";
import { Button } from "@/components/ui/button";
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu";
import { Sun, Moon, Monitor, Palette } from "lucide-react";

const themes = [
  { value: "default", label: "Default" },
  { value: "blue", label: "Blue" },
  { value: "green", label: "Green" },
  { value: "purple", label: "Purple" },
] as const;

const modes = [
  { value: "light", label: "Light", icon: Sun },
  { value: "dark", label: "Dark", icon: Moon },
  { value: "system", label: "System", icon: Monitor },
] as const;

export function ThemeSwitcher() {
  const { theme, mode, setTheme, setMode } = useTheme();

  return (
    <div className="flex items-center gap-2">
      {/* Color theme */}
      <DropdownMenu>
        <DropdownMenuTrigger asChild>
          <Button variant="ghost" size="icon">
            <Palette className="h-4 w-4" />
          </Button>
        </DropdownMenuTrigger>
        <DropdownMenuContent>
          {themes.map((t) => (
            <DropdownMenuItem
              key={t.value}
              onClick={() => setTheme(t.value)}
            >
              {t.label} {theme === t.value && "✓"}
            </DropdownMenuItem>
          ))}
        </DropdownMenuContent>
      </DropdownMenu>

      {/* Light/dark mode */}
      <DropdownMenu>
        <DropdownMenuTrigger asChild>
          <Button variant="ghost" size="icon">
            {mode === "dark" ? <Moon className="h-4 w-4" /> : <Sun className="h-4 w-4" />}
          </Button>
        </DropdownMenuTrigger>
        <DropdownMenuContent>
          {modes.map((m) => (
            <DropdownMenuItem
              key={m.value}
              onClick={() => setMode(m.value)}
            >
              <m.icon className="mr-2 h-4 w-4" />
              {m.label} {mode === m.value && "✓"}
            </DropdownMenuItem>
          ))}
        </DropdownMenuContent>
      </DropdownMenu>
    </div>
  );
}
```

## Internationalization with next-intl

### Setup

```bash
pnpm add next-intl
```

### Configuration

```ts
// src/config/i18n.ts
export const locales = ["en", "fr", "de"] as const;
export type Locale = (typeof locales)[number];
export const defaultLocale: Locale = "en";
```

### Next.js middleware for locale routing

```ts
// src/middleware.ts
import createMiddleware from "next-intl/middleware";
import { locales, defaultLocale } from "@/config/i18n";

export default createMiddleware({
  locales,
  defaultLocale,
  localePrefix: "always",
});

export const config = {
  matcher: ["/((?!api|_next|.*\\..*).*)"],
};
```

### Message file structure

```
public/locales/
├── en/
│   ├── common.json
│   ├── dashboard.json
│   └── errors.json
├── fr/
│   ├── common.json
│   ├── dashboard.json
│   └── errors.json
└── de/
    ├── common.json
    ├── dashboard.json
    └── errors.json
```

### Message file example

```json
// public/locales/en/common.json
{
  "app": {
    "title": "AppName",
    "description": "A brief description"
  },
  "nav": {
    "home": "Home",
    "settings": "Settings",
    "search": "Search..."
  },
  "actions": {
    "create": "Create",
    "delete": "Delete",
    "save": "Save",
    "cancel": "Cancel",
    "confirm": "Confirm"
  },
  "theme": {
    "light": "Light",
    "dark": "Dark",
    "system": "System"
  }
}
```

### Translation key convention

Use dot-separated namespaced keys:

- `common.nav.home` — shared navigation label
- `dashboard.notesList.empty` — feature-specific empty state
- `errors.notFound` — error message

### Using translations in components

```tsx
// Server component
import { useTranslations } from "next-intl";

export default function Dashboard() {
  const t = useTranslations("dashboard");
  return <h1>{t("title")}</h1>;
}

// Client component
"use client";
import { useTranslations } from "next-intl";

export function SearchBar() {
  const t = useTranslations("common.nav");
  return <input placeholder={t("search")} />;
}
```

### Language switcher

```tsx
"use client";

import { useLocale } from "next-intl";
import { useRouter, usePathname } from "next/navigation";
import { locales } from "@/config/i18n";
import {
  Select,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from "@/components/ui/select";

const localeNames: Record<string, string> = {
  en: "English",
  fr: "Français",
  de: "Deutsch",
};

export function LanguageSwitcher() {
  const locale = useLocale();
  const router = useRouter();
  const pathname = usePathname();

  function handleChange(newLocale: string) {
    const newPath = pathname.replace(`/${locale}`, `/${newLocale}`);
    router.push(newPath);
  }

  return (
    <Select value={locale} onValueChange={handleChange}>
      <SelectTrigger className="w-[140px]">
        <SelectValue />
      </SelectTrigger>
      <SelectContent>
        {locales.map((loc) => (
          <SelectItem key={loc} value={loc}>
            {localeNames[loc] ?? loc}
          </SelectItem>
        ))}
      </SelectContent>
    </Select>
  );
}
```

### Layout integration

```tsx
// src/app/[locale]/layout.tsx
import { NextIntlClientProvider } from "next-intl";
import { getMessages } from "next-intl/server";
import { ThemeProvider } from "@/components/shared/ThemeProvider";

export default async function LocaleLayout({
  children,
  params: { locale },
}: {
  children: React.ReactNode;
  params: { locale: string };
}) {
  const messages = await getMessages();

  return (
    <html lang={locale} suppressHydrationWarning>
      <body>
        <NextIntlClientProvider messages={messages}>
          <ThemeProvider>
            {children}
          </ThemeProvider>
        </NextIntlClientProvider>
      </body>
    </html>
  );
}
```
