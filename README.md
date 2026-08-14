# Flight Schedule — Skyscanner Backpack React App

A simple flight date selection page built with [Backpack](https://backpack.github.io/), Skyscanner's design system. Built as part of the Skyscanner Front-End Software Engineering job simulation.

The app renders a heading, a date picker, and a continue action — the first screen of a flight search flow.

## Features

- **Backpack Calendar** — single date selection with month navigation, wired to React state
- **Backpack components throughout** — `BpkText`, `BpkButton`, and `BpkCalendar` for design-system-consistent styling
- **Week starts Monday** — configured via `weekStartsOn`, matching European convention
- **Accessible date labels** — full date descriptions supplied via `formatDateFull` for screen readers

## Tech Stack

| | |
|---|---|
| Framework | React 17 |
| Design system | `@skyscanner/backpack-web` 15.x |
| Build tooling | `@skyscanner/backpack-react-scripts` 11.2.4 |
| Date handling | `date-fns` |
| Styling | SCSS with CSS Modules |

## Getting Started

### Prerequisites

Node.js 18 or later, and npm.

### Installation

```bash
git clone https://github.com/hunteeerrr00/skyscanner-backpack.git
cd skyscanner-backpack/my-app
npm install
```

### Running

```bash
npm start
```

The app runs at [http://localhost:3000](http://localhost:3000). In GitHub Codespaces, open the forwarded port 3000 from the **Ports** tab rather than navigating to localhost directly.

### Testing

```bash
npm test
```

Runs the Jest test suite in watch mode. Press `q` to exit.

### Production build

```bash
npm run build
```

Outputs an optimised bundle to `build/`.

## Project Structure

```
my-app/
├── src/
│   ├── App.js          # Main component — heading, calendar, button
│   ├── App.scss        # Layout and spacing
│   ├── App.test.js     # Render test
│   └── index.js        # Entry point
├── public/
└── package.json
```

## Implementation Notes

### Calendar state

The selected date is held in component state and passed back to the calendar through `selectionConfiguration`, making the component fully controlled:

```jsx
const [selectedDate, setSelectedDate] = useState(null);

<BpkCalendar
  id="departure-calendar"
  onDateSelect={setSelectedDate}
  selectionConfiguration={{
    type: CALENDAR_SELECTION_TYPE.single,
    date: selectedDate,
  }}
  formatMonth={formatMonth}
  formatDateFull={formatDateFull}
  daysOfWeek={daysOfWeek}
  weekStartsOn={1}
/>
```

### Babel compilation of Backpack components

Backpack ships its components as uncompiled source containing Flow type annotations. `backpack-react-scripts` handles this via a prefix allowlist, but the default list needs to be set explicitly for `@skyscanner/`-scoped packages to be picked up. This is configured in `package.json`:

```json
"backpack-react-scripts": {
  "babelIncludePrefixes": ["@skyscanner/", "bpk-", "saddlebag-"]
}
```

Without this, the build fails with `SyntaxError: Unexpected token, expected ","` on every Backpack component that uses Flow syntax.

## Troubleshooting

**Build fails with Flow syntax errors** — check the `babelIncludePrefixes` config above is present in `package.json`.

**`date-fns` format token errors** — this project uses date-fns v2 tokens (`MMMM yyyy`). On v1, use `MMMM YYYY` and `dddd, Do MMMM YYYY` instead.

**Blank page in the browser** — check the terminal. A blank page almost always means a compilation failure rather than a rendering problem.

**`Could not read package.json`** — run npm commands from inside the `my-app` directory, not the repository root.

## Acknowledgements

Built following the [Skyscanner Front-End Software Engineering simulation](https://www.theforage.com/) on Forage. Backpack is Skyscanner's open-source design system.
