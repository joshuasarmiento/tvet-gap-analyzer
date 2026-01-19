# TVET-Employment Gap Analyzer (Frontend)

A modern, data-driven dashboard built with **Vue 3** and **Tailwind CSS** to visualize the gap between vocational graduates and industry labor demand in the Philippines.

## 🎨 Design Features
- **Dynamic Dashboard:** Real-time filtering between Regions, Provinces, and Industrial Sectors.
- **Opportunity Scoring:** Visual indicators (Emerald/Amber/Slate) highlighting which sectors offer the highest "Employment Potential."
- **Educational Modal:** An onboarding overlay that explains data sources (TESDA) and calculation methodology (DOLE Multipliers).
- **Responsive UI:** Fully optimized for mobile and desktop viewing using Tailwind's utility-first classes.

## 🛠️ Tech Stack
- [Vue 3 (Composition API)](https://vuejs.org/) - Frontend framework.
- [Tailwind CSS](https://tailwindcss.com/) - Styling and Layout.
- [Vite](https://vitejs.dev/) - Frontend tooling.

## 📥 Installation

1. Install dependencies:
   ```bash
   npm install

```

2. Start the development server:
```bash
npm run dev

```


3. **Configure API URL:**
Ensure the `fetch` call in `App.vue` points to your running backend (default: `http://localhost:3000`).

## 📊 Data Logic

* **Supply:** Derived from "Graduates" or "Certified" counts in TESDA's 2024 records.
* **Demand:** Estimated by applying industry-specific weights (e.g., ICT = 2.8x) to the supply, representing the "talent shortage" reported by DOLE.
* **Gap:** The numerical difference between estimated vacancies and available certified workers.