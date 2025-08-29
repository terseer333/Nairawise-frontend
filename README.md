# Nairawise-frontend
<script type="module" src="/src/main.tsx"></script> <script type="text/javascript" src="https://replit.com/public/js/replit-dev-banner.js"></script>

CSS Code (client/src/index.css) @tailwind base; @tailwind components; @tailwind utilities;

:root { --background: hsl(0 0% 100%); --foreground: hsl(210 25% 7.8431%); --card: hsl(180 6.6667% 97.0588%); --card-foreground: hsl(210 25% 7.8431%); --popover: hsl(0 0% 100%); --popover-foreground: hsl(210 25% 7.8431%); --primary: hsl(158.7097 64.4186% 43.7255%); --primary-foreground: hsl(0 0% 100%); --secondary: hsl(217.2 91.2% 59.8%); --secondary-foreground: hsl(0 0% 100%); --muted: hsl(240 1.9608% 90%); --muted-foreground: hsl(215 16.2791% 46.2745%); --accent: hsl(42.6154 96.2264% 50.3922%); --accent-foreground: hsl(0 0% 100%); --destructive: hsl(0 84.2105% 60.1961%); --destructive-foreground: hsl(0 0% 100%); --border: hsl(201.4286 30.4348% 90.9804%); --input: hsl(200 23.0769% 97.4510%); --ring: hsl(158.7097 64.4186% 43.7255%); --success: hsl(158.7097 64.4186% 43.7255%); --warning: hsl(42.6154 96.2264% 50.3922%); --danger: hsl(0 84.2105% 60.1961%); }

.dark { --background: hsl(0 0% 0%); --foreground: hsl(200 6.6667% 91.1765%); --card: hsl(228 9.8039% 10%); --card-foreground: hsl(0 0% 85.0980%); /* ... more dark theme variables */ }

@layer base {

{ @apply border-border; }
body { @apply font-sans antialiased bg-background text-foreground; font-family: 'Inter', sans-serif; } }

@layer utilities { .text-success { color: var(--success); }

.text-warning { color: var(--warning); }

.text-danger { color: var(--danger); }

.bg-success { background-color: var(--success); }

.bg-warning { background-color: var(--warning); }

.bg-danger { background-color: var(--danger); } }


MAIN APPLICATION ENTRY POINT(client/src/main.tsx)import{ createRoot } from "react-doment"; import App from "./App"; import "./index.css";

createRoot(document.getElementById("root")!).render();

MAIN APP COMPONENTS (client/app.tsx) import { Switch, Route } from "wouter"; import { queryClient } from "./lib/queryClient"; import { QueryClientProvider } from "@tanstack/react-query"; import { Toaster } from "@/components/ui/toaster"; import { TooltipProvider } from "@/components/ui/tooltip"; import AppLayout from "@/components/layout/app-layout"; import Dashboard from "@/pages/dashboard"; import Transactions from "@/pages/transactions"; import Budgets from "@/pages/budgets"; import Insights from "@/pages/insights"; import Settings from "@/pages/settings"; import NotFound from "@/pages/not-found";

function Router() { return ( ); }

function App() { return ( ); }

export default App;

API Client (client/src/lib/queryClient.ts) import { QueryClient, QueryFunction } from "@tanstack/react-query";

async function throwIfResNotOk(res: Response) { if (!res.ok) { const text = (await res.text()) || res.statusText; throw new Error(${res.status}: ${text}); } }

export async function apiRequest( method: string, url: string, data?: unknown | undefined, ): Promise { const res = await fetch(url, { method, headers: data ? { "Content-Type": "application/json" } : {}, body: data ? JSON.stringify(data) : undefined, credentials: "include", });

await throwIfResNotOk(res); return res; }

type UnauthorizedBehavior = "returnNull" | "throw"; export const getQueryFn: (options: { on401: UnauthorizedBehavior; }) => QueryFunction = ({ on401: unauthorizedBehavior }) => async ({ queryKey }) => { const res = await fetch(queryKey.join("/") as string, { credentials: "include", });

if (unauthorizedBehavior === "returnNull" && res.status === 401) {
  return null;
}

await throwIfResNotOk(res);
return await res.json();
};

export const queryClient = new QueryClient({ defaultOptions: { queries: { queryFn: getQueryFn({ on401: "throw" }), refetchInterval: false, refetchOnWindowFocus: false, staleTime: Infinity, retry: false, }, mutations: { retry: false, }, }, });
// script.js - Frontend logic for NairaWise

window.addEventListener('DOMContentLoaded', () => {
  console.log("NairaWise frontend loaded.");
  // Example: update a placeholder element with id="status"
  const statusEl = document.getElementById('status');
  if (statusEl) {
    statusEl.textContent = 'Frontend is loaded. Ready to use!';
  }

  // Example: fetch data from backend API (uncomment and adjust URL as needed)
  // fetch('http://localhost:3000/api/example')
  //   .then(response => response.json())
  //   .then(data => console.log(data))
  //   .catch(err => console.error('API error:', err));
});

# NairaWise Frontend

This is the frontend for the **NairaWise** budget tracker app. It is a simple static HTML/JavaScript application. 

## Usage

- Clone or download the repository.
- Ensure `index.html` and `script.js` are in the same folder.
- Open `index.html` in your web browser to launch the app.
- (Optional) For local development, you can run a static server, e.g., with `npx serve` or `python -m http.server 8000`.
- The browser console will log “NairaWise frontend loaded.” once the page initializes.

## Notes

- Modify `script.js` to add interactivity or connect to the backend API.
- Adjust any API URLs in `script.js` to match your backend server location.
