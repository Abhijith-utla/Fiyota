# Toyota Drive Visualization

An intelligent, client-side React application that transforms vehicle shopping from transactional price comparison into strategic financial decision-making. Built with TypeScript, React, and Recharts, this tool provides personalized financing analysis, affordability calculations, and advanced financial insights to help users make informed vehicle purchase decisions.

**Live Demo**: https://lovable.dev/projects/b8bf91c8-2115-4f2e-971b-70c52d8336d0

---

## 🎯 What Makes This Different

Most car comparison tools answer: **"How much will I pay?"**

This tool answers: **"Is this a smart financial decision?"**

### Key Differentiators

- **Financial Coaching, Not Just Calculation** - Provides strategic insights on credit improvement, down payment optimization, and equity building
- **Per-Vehicle Customization** - Configure unique lease and finance terms for each vehicle in your comparison
- **Real-Time Reactivity** - All 10 charts update instantly as you adjust any financial parameter
- **Zero Backend** - Completely client-side with no APIs, databases, or server dependencies
- **Educational Focus** - Each chart includes financial insights and coaching messages

---

## 📊 Core Features

### 1. Financial Profile Management
Define your personal financial situation:
- Monthly income
- Credit score (affects interest rates)
- Maximum down payment capacity
- Preferred monthly payment target
- Trade-in vehicle value

### 2. Multi-Vehicle Comparison
- Select up to 10 Toyota vehicles simultaneously
- Each vehicle can have unique financing terms (lease and finance)
- Affordability badges show which vehicles fit your budget
- Real-time filtering based on your financial profile

### 3. Advanced Chart Visualizations

#### Basic Cost Charts (5 charts)
1. **Base Price Comparison** - Simple price bar chart
2. **Monthly Payment Chart** - Compare monthly costs across vehicles
3. **Lease vs Finance Split View** - Side-by-side detailed comparison
4. **Total Cost Analysis** - Long-term ownership cost over full term
5. **Savings Analysis** - Cost difference between lease and finance

#### Financial Insight Charts (5 charts) ⭐ NEW
6. **Affordability Index** - Gauge showing payment as % of income with risk levels
7. **Equity Build Timeline** - Line chart showing wealth accumulation vs $0 for lease
8. **Credit Score Impact Simulator** - How improving credit score reduces payments
9. **Down Payment Sensitivity** - Optimal upfront investment analysis
10. **Depreciation Curve** - 5-year vehicle value projection

### 4. Per-Vehicle Financing Controls
Each vehicle can be customized independently:

**Lease Options:**
- Term: 24-48 months
- Down payment: $0-$10,000
- Interest rate: 0-10%

**Finance Options:**
- Term: 24-84 months
- Down payment: $0-$15,000
- Interest rate: 0-15%

---

## 🏗️ Architecture & Technical Implementation

### Technology Stack

```
Frontend Framework: React 18 + TypeScript
Build Tool: Vite
UI Components: shadcn/ui (Radix UI primitives)
Styling: Tailwind CSS
Charts: Recharts (D3-based declarative charts)
State Management: React Hooks (useState, no Redux)
Routing: React Router
Data Fetching: @tanstack/react-query (available but unused - no API calls)
Icons: Lucide React
```

### Project Structure

```
toyota-drive-viz/
├── src/
│   ├── components/
│   │   ├── charts/                    # All visualization components
│   │   │   ├── AffordabilityGaugeChart.tsx
│   │   │   ├── CreditScoreImpactChart.tsx
│   │   │   ├── DepreciationCurveChart.tsx
│   │   │   ├── DownPaymentSensitivityChart.tsx
│   │   │   ├── EquityBuildChart.tsx
│   │   │   ├── LeaseVsFinanceComparison.tsx
│   │   │   ├── MonthlyPaymentChart.tsx
│   │   │   ├── PriceComparisonChart.tsx
│   │   │   ├── SavingsChart.tsx
│   │   │   └── TotalCostChart.tsx
│   │   ├── ui/                        # shadcn/ui primitives
│   │   ├── AffordabilityBadge.tsx     # Color-coded affordability indicator
│   │   ├── CarSelector.tsx            # Multi-select vehicle picker
│   │   ├── ChartNavigation.tsx        # Chart type selector with categories
│   │   ├── FinancialProfileForm.tsx   # User financial input form
│   │   ├── FinancialTipsCard.tsx      # Personalized financial advice
│   │   └── PerCarFinancingControls.tsx # Per-vehicle term customization
│   ├── data/
│   │   └── cars.ts                    # 10 hardcoded Toyota vehicles
│   ├── types/
│   │   ├── car.ts                     # Car, FinancingOption, ComparisonData
│   │   └── userProfile.ts             # UserFinancialProfile, AffordabilityAnalysis
│   ├── utils/
│   │   ├── affordability.ts           # Credit scoring, affordability logic
│   │   └── financing.ts               # Payment calculations, equity, depreciation
│   ├── pages/
│   │   ├── Index.tsx                  # Main application page
│   │   └── NotFound.tsx               # 404 page
│   └── App.tsx                        # Root component with routing
├── public/
└── package.json
```

---

## 🧮 Financial Calculation Logic

### 1. Monthly Payment Calculation

Uses standard amortization formula:

```typescript
monthlyPayment = (principal × monthlyRate) / (1 - (1 + monthlyRate)^(-termMonths))

Where:
  principal = basePrice - downPayment
  monthlyRate = annualInterestRate / 12 / 100
  termMonths = loan term in months
```

**Implementation**: [src/utils/financing.ts:3-14](src/utils/financing.ts)

### 2. Total Cost Calculation

```typescript
totalCost = (monthlyPayment × termMonths) + downPayment
```

**Implementation**: [src/utils/financing.ts:16-22](src/utils/financing.ts)

### 3. Equity Build Over Time

Calculates principal paydown each month:

```typescript
For each month:
  interestPayment = remainingPrincipal × monthlyRate
  principalPayment = monthlyPayment - interestPayment
  remainingPrincipal -= principalPayment
  equity = basePrice - remainingPrincipal
```

**Implementation**: [src/utils/financing.ts:58-92](src/utils/financing.ts)

### 4. Depreciation Curve

Industry-standard depreciation rates:

```typescript
Year 1: -20% (new car becomes "used")
Year 2: -15%
Year 3: -10%
Years 4-5: -8% per year
```

**Implementation**: [src/utils/financing.ts:95-125](src/utils/financing.ts)

### 5. Affordability Analysis

Financial health scoring based on payment-to-income ratio:

```typescript
budgetImpact = (monthlyPayment / monthlyIncome) × 100

Status Levels:
  < 10%  → Excellent (green)
  < 15%  → Good (blue)
  < 20%  → Caution (yellow)
  ≥ 20%  → Overextended (red)
```

**Implementation**: [src/utils/financing.ts:128-160](src/utils/financing.ts)

### 6. Credit Score Impact

Interest rate adjustments by credit tier:

```typescript
Credit Score → Interest Rate Adjustment
  750+     → Base rate - 1.0%  (Excellent)
  700-749  → Base rate - 0.5%  (Good)
  650-699  → Base rate + 0.0%  (Fair)
  600-649  → Base rate + 1.5%  (Poor)
  < 600    → Base rate + 3.0%  (Very Poor)
```

**Implementation**: [src/utils/financing.ts:163-178](src/utils/financing.ts)

### 7. Down Payment Sensitivity

Calculates impact of varying down payments (0% to 50% of base price):

```typescript
For each down payment amount (6 steps):
  principal = basePrice - downPayment
  monthlyPayment = calculate()
  totalInterest = (monthlyPayment × term) - principal
  monthlyReduction = previousMonthly - currentMonthly
```

**Implementation**: [src/utils/financing.ts:212-253](src/utils/financing.ts)

---

## 🔄 Data Flow Architecture

### State Management

All application state is managed in [src/pages/Index.tsx](src/pages/Index.tsx) using React hooks:

```typescript
// Core State
const [selectedCarIds, setSelectedCarIds] = useState<string[]>([])
const [activeViews, setActiveViews] = useState<ChartView[]>(['price'])
const [carFinancingOptions, setCarFinancingOptions] = useState<Map<string, {
  lease: FinancingOption
  finance: FinancingOption
}>>()
const [userProfile, setUserProfile] = useState<UserFinancialProfile>({
  monthlyIncome: 5000,
  creditScore: 720,
  maxDownPayment: 5000,
  preferredMonthlyPayment: 500,
  hasTradeIn: false,
  tradeInValue: 0
})
```

### Data Flow Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                    User Interactions                         │
│  (CarSelector, FinancialProfileForm, PerCarFinancingControls)│
└─────────────────────┬───────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────┐
│                 State Updates (Index.tsx)                    │
│  • selectedCarIds                                            │
│  • carFinancingOptions (Map<carId, {lease, finance}>)       │
│  • userProfile                                               │
│  • activeViews                                               │
└─────────────────────┬───────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────┐
│            Utility Functions (Real-time Calc)                │
│  • calculateFinancing() → monthly payment                    │
│  • calculateEquityOverTime() → equity timeline               │
│  • calculateDepreciationCurve() → resale value               │
│  • calculateCreditScoreImpact() → payment scenarios          │
│  • calculateDownPaymentSensitivity() → sensitivity curve     │
│  • calculateAffordabilityImpact() → budget analysis          │
└─────────────────────┬───────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────┐
│                  Chart Components                            │
│  Each chart receives:                                        │
│  • cars: Car[]                                               │
│  • carFinancingOptions: Map                                  │
│  • userProfile: UserFinancialProfile                         │
│                                                              │
│  Charts recalculate on EVERY prop change                     │
└─────────────────────┬───────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────┐
│              Recharts Rendering (SVG)                        │
│  LineChart, BarChart, AreaChart, RadialBarChart              │
└─────────────────────────────────────────────────────────────┘
```

### Key Data Structures

#### Car Interface
```typescript
interface Car {
  id: string              // Unique identifier
  name: string            // "Corolla", "Camry", etc.
  model: string           // "LE", "Base", etc.
  year: number            // 2023, 2024
  basePrice: number       // MSRP in dollars
  image: string           // Placeholder image path
  category: string        // "Sedan", "SUV", "Truck", "Minivan"
  priceSource?: string    // Dealership source
}
```

#### FinancingOption Interface
```typescript
interface FinancingOption {
  type: 'lease' | 'finance'
  term: number              // Months (24-84)
  downPayment: number       // Dollars
  interestRate: number      // Annual percentage (0-15)
  monthlyPayment?: number   // Calculated
  totalCost?: number        // Calculated
}
```

#### UserFinancialProfile Interface
```typescript
interface UserFinancialProfile {
  monthlyIncome: number
  creditScore: number           // 300-850
  maxDownPayment: number
  preferredMonthlyPayment: number
  hasTradeIn: boolean
  tradeInValue: number
}
```

---

## 🎨 Component Architecture

### 1. CarSelector Component
**Purpose**: Multi-select vehicle picker with affordability indicators

**Props**:
```typescript
{
  cars: Car[]
  selectedCarIds: string[]
  onSelectionChange: (ids: string[]) => void
  userProfile: UserFinancialProfile
  financingOption: FinancingOption
  recommendedCarIds: string[]
}
```

**Key Features**:
- Checkbox-based multi-select
- Affordability badges (green/yellow/red)
- Visual feedback for recommended vehicles
- Vehicle images and pricing

**Implementation**: [src/components/CarSelector.tsx](src/components/CarSelector.tsx)

---

### 2. PerCarFinancingControls Component
**Purpose**: Accordion-based per-vehicle financing customization

**Props**:
```typescript
{
  cars: Car[]
  carFinancingOptions: Map<string, { lease: FinancingOption; finance: FinancingOption }>
  onFinancingChange: (carId: string, type: 'lease' | 'finance', option: FinancingOption) => void
}
```

**Key Features**:
- Collapsible panel to save screen space
- Tabs for Lease vs Finance
- Accordion per vehicle
- Sliders for term, down payment, interest rate
- Real-time badge updates

**Implementation**: [src/components/PerCarFinancingControls.tsx](src/components/PerCarFinancingControls.tsx)

---

### 3. ChartNavigation Component
**Purpose**: Chart selector with categorization

**Key Features**:
- Grouped into "Financial Insights" and "Basic Comparisons"
- Multi-select with minimum 1 chart active
- Visual badges highlighting premium insights
- Icon-based navigation

**Chart Types**:
```typescript
type ChartView =
  | 'price' | 'monthly' | 'total' | 'savings' | 'comparison'  // Basic
  | 'affordability' | 'equity' | 'creditImpact'               // Insights
  | 'downPayment' | 'depreciation'                            // Insights
```

**Implementation**: [src/components/ChartNavigation.tsx](src/components/ChartNavigation.tsx)

---

### 4. Chart Components (10 Total)

#### Basic Charts

**PriceComparisonChart**
- Simple bar chart of base prices
- No financing calculations
- Useful for quick visual comparison

**MonthlyPaymentChart**
- Grouped bar chart (lease vs finance)
- Uses calculated monthly payments
- Updates when financing terms change

**LeaseVsFinanceComparison**
- Side-by-side detailed view
- Shows all terms, payments, total costs
- Includes "Better Deal" badge

**TotalCostChart**
- Line chart comparing total ownership cost
- Includes down payment + all monthly payments

**SavingsChart**
- Bar chart showing cost difference
- Highlights which option saves money

#### Financial Insight Charts

**AffordabilityGaugeChart**
- Radial gauge (0-30% of income)
- Color-coded risk levels
- Shows actual monthly payment
- Educational guideline (< 15% recommended)
- **Multi-car support**: Separate gauge per vehicle

**EquityBuildChart** ⭐ MOST IMPACTFUL
- Line chart showing equity growth over time
- Finance lines grow (building wealth)
- Lease line stays at $0 (no ownership)
- Summary boxes showing final equity
- **Multi-car support**: Multiple equity curves

**CreditScoreImpactChart**
- Line chart across credit score ranges (600-800)
- Shows how monthly payment decreases with better credit
- Calculates potential savings
- **Multi-car support**: Compare credit impact across vehicles

**DownPaymentSensitivityChart**
- Line chart showing monthly payment vs down payment amount
- Demonstrates diminishing returns
- Shows interest savings
- **Multi-car support**: Sensitivity curves for each vehicle

**DepreciationCurveChart**
- Area chart showing vehicle value over 5 years
- Industry-standard depreciation rates
- Educational insights about investment vs expense
- **Multi-car support**: Overlaid depreciation curves

---

## 🧪 Example Calculation Walkthrough

### Scenario: 2024 Corolla Hybrid Finance

**Vehicle**: Corolla Hybrid 2024 - $23,500

**User Profile**:
- Monthly Income: $5,000
- Credit Score: 720 (Good)
- Max Down Payment: $5,000

**Finance Terms**:
- Term: 60 months
- Down Payment: $5,000
- Base Interest Rate: 5.5%

### Step-by-Step Calculations

#### 1. Credit Score Adjustment
```
Credit Score: 720 (Good range: 700-749)
Adjustment: Base rate - 0.5%
Final Rate: 5.5% - 0.5% = 5.0%
```

#### 2. Monthly Payment
```
Principal = $23,500 - $5,000 = $18,500
Monthly Rate = 5.0% / 12 / 100 = 0.004167
Term = 60 months

Monthly Payment = 18,500 × (0.004167 × (1.004167)^60) / ((1.004167)^60 - 1)
                = 18,500 × 0.01887
                = $349.09
```

#### 3. Total Cost
```
Total Cost = ($349.09 × 60) + $5,000
           = $20,945.40 + $5,000
           = $25,945.40
```

#### 4. Affordability Analysis
```
Budget Impact = ($349.09 / $5,000) × 100
              = 6.98%

Status: Excellent (< 10%)
Message: "Well within budget - excellent financial position"
```

#### 5. Total Interest Paid
```
Total Interest = Total Cost - Base Price
               = $25,945.40 - $23,500
               = $2,445.40
```

#### 6. Equity at 36 Months (3 years)
```
After 36 payments:
  Remaining Principal: ~$7,200
  Equity Built: $23,500 - $7,200 = $16,300
```

#### 7. Depreciation at 3 Years
```
Year 1: $23,500 × 0.80 = $18,800
Year 2: $18,800 × 0.85 = $15,980
Year 3: $15,980 × 0.90 = $14,382

Projected Value: $14,382
Your Equity: $16,300
Net Position: +$1,918 (you own more than it's worth - underwater)
```

---

## 🎯 User Workflows

### Workflow 1: First-Time Buyer

1. **Set Financial Profile**
   - Enter monthly income: $4,000
   - Credit score: 680 (Fair)
   - Max down payment: $2,000

2. **View Affordability Badges**
   - Green badges: Corolla, Corolla Hybrid (affordable)
   - Yellow badges: Camry, RAV4 (stretching budget)
   - Red badges: Highlander, 4Runner (not affordable)

3. **Select Affordable Vehicles**
   - Choose Corolla and Corolla Hybrid

4. **Open Affordability Index Chart**
   - See payment as 12% of income (Good status)
   - Read coaching: "Comfortable payment - good financial balance"

5. **Check Credit Impact Chart**
   - See potential savings: $3,200 if credit improves to 750+
   - Decision: Work on credit before purchasing

---

### Workflow 2: Lease vs Finance Decision

1. **Select Vehicle**: 2024 RAV4

2. **View Equity Build Chart**
   - Finance: $28,675 equity after 60 months
   - Lease: $0 equity after 36 months
   - Insight: "You're buying an asset vs renting"

3. **Check Total Cost Chart**
   - Finance: $34,200 total (you own the vehicle)
   - Lease: $14,500 total (you own nothing)

4. **Open Monthly Payment Chart**
   - Finance: $520/month
   - Lease: $380/month

5. **Decision**: Choose finance for long-term ownership despite higher monthly

---

### Workflow 3: Optimizing Down Payment

1. **Select Vehicle**: 2024 Camry

2. **Open Down Payment Sensitivity Chart**
   - Current ($3,000 down): $450/month
   - Increased ($8,000 down): $360/month
   - Savings: $90/month = $5,400 over 60 months

3. **Adjust Down Payment Slider**
   - Move from $3,000 → $8,000
   - Watch all charts update in real-time

4. **Decision**: Put more down to reduce monthly burden

---

## 📦 Data Storage & Persistence

### Current Architecture: **No Persistence**

This is a **purely client-side application** with:
- ❌ No backend server
- ❌ No database (SQL, NoSQL)
- ❌ No external APIs
- ❌ No localStorage/sessionStorage
- ❌ No cookies

**Consequence**: All state is lost on page refresh.

### Static Data Source

**Vehicle Data**: [src/data/cars.ts](src/data/cars.ts)

10 hardcoded Toyota vehicles (2023-2024 model years):
- Corolla 2023 LE - $21,550
- Camry 2023 LE - $26,220
- RAV4 2023 LE - $27,575
- 4Runner 2023 SR5 - $40,155
- Sienna 2023 LE - $35,385
- Corolla Cross 2024 Base - $23,860
- RAV4 2024 Base - $28,675
- Highlander 2024 Base - $39,270
- Corolla Hybrid 2024 Base - $23,500
- Tundra 2024 Base - $39,965

Prices sourced from real Toyota dealerships (noted in `priceSource` field).

### Why No Backend?

**Design Decision**: Keep deployment simple
- ✅ Deploy to static hosting (Vercel, Netlify, GitHub Pages)
- ✅ No server costs
- ✅ Instant load times
- ✅ 100% client-side privacy
- ✅ Works offline after initial load

---

## 🚀 Getting Started

### Prerequisites

- Node.js 16+ and npm (install with [nvm](https://github.com/nvm-sh/nvm#installing-and-updating))
- Google Gemini API key (for AI chatbot feature - get one at [Google AI Studio](https://aistudio.google.com/app/apikey))

### Installation

```sh
# Clone the repository
git clone <YOUR_GIT_URL>
cd toyota-drive-viz

# Install dependencies
npm install

# Set up environment variables
cp .env.example .env

# Edit .env and add your Gemini API key
# VITE_GEMINI_API_KEY=your_api_key_here

# Start development server
npm run dev
```

Development server runs at `http://localhost:5173`

### Build for Production

```sh
npm run build
```

Outputs optimized static files to `dist/` directory.

---

## 🏦 Pre-Approval Check

Get an instant estimate of your loan approval chances based on your financial profile.

### How It Works

Click the **Pre-Approval** button in the top-right corner to run a comprehensive analysis that evaluates:

1. **Credit Score** (Weight: 2.0 points)
   - Excellent (720+): Best rates and terms
   - Good (660-719): Competitive rates
   - Fair (610-659): Higher rates, may need larger down payment
   - Poor (<610): Likely to be declined

2. **Income to Payment Ratio** (Weight: 2.0 points)
   - Income should be at least 2.5x your monthly payment
   - Higher ratios (3.5x+) significantly improve approval odds

3. **Down Payment** (Weight: 1.5 points)
   - 20%+ down: Excellent approval chances
   - 15-20% down: Good standing
   - 10-15% down: Meets minimum requirements
   - <10% down: May face difficulties

4. **Trade-In Value** (Weight: 0.5 points)
   - $1,000+ trade-in helps reduce financed amount
   - Improves overall approval score

### Approval Ratings

- **Very Likely** (4.5+ points): Strong approval probability with favorable terms
- **Likely** (3.5-4.4 points): Good chance of approval
- **Possible** (2.0-3.4 points): May be approved with higher rates or conditions
- **Unlikely** (<2.0 points): Significant improvements needed

### Features

- **Detailed Factor Breakdown**: See exactly how each factor affects your approval
- **Personalized Recommendations**: Get specific steps to improve your approval odds
- **Visual Progress Bars**: Understand your score at a glance
- **Terminal Logging**: Full analysis printed to console for debugging

### Example Output

```
🏦 PRE-APPROVAL CHECK
================================================================================
Status: Very Likely
Score: 4.8/5
Vehicle: 2024 Toyota RAV4 Base
Estimated Price: $28,675

Factor Breakdown:
  Credit Score: 2.0/2 - excellent
  Income Ratio: 2.0/2 - excellent
  Down Payment: 1.5/1.5 - excellent
  Trade-In: 0.5/0.5 - helpful
================================================================================
```

**Important**: This is an estimate only and not a guarantee of approval. Actual lending decisions depend on additional factors including employment history, debt-to-income ratio, and lender-specific criteria.

---

## 🤖 AI Chatbot Assistant

This application includes an intelligent AI chatbot powered by Google Gemini that helps users make informed vehicle financing decisions.

### Features

- **Context-Aware Assistance**: The chatbot has full knowledge of:
  - All 10 Toyota vehicles with detailed specifications
  - Your selected vehicles and their configurations
  - Your financial profile (income, credit score, down payment)
  - Current financing terms (interest rates, loan periods)
  - Financing basics (leasing vs financing, down payment impact, etc.)

- **Smart Recommendations**: The chatbot can:
  - Answer questions about specific vehicles
  - Compare lease vs finance options
  - Explain affordability calculations
  - Provide credit score improvement advice
  - Suggest optimal down payment amounts
  - Clarify financing terminology

- **Easy Access**: Click the chat icon in the top-right corner to open the sidebar and start a conversation.

### Setting Up the Chatbot

1. **Get a Gemini API Key**:
   - Visit [Google AI Studio](https://aistudio.google.com/app/apikey)
   - Sign in with your Google account
   - Click "Create API Key"
   - Copy the generated key

2. **Add the Key to Your Environment**:
   - Create a `.env` file in the project root (or copy from `.env.example`)
   - Add your API key:
     ```
     VITE_GEMINI_API_KEY=your_actual_api_key_here
     ```

3. **Restart the Development Server**:
   ```sh
   npm run dev
   ```

The chatbot will now be fully functional. If the API key is not configured, the chatbot will display a helpful message explaining how to set it up.

### Chatbot Context Data

The chatbot uses a comprehensive knowledge base stored in [src/data/chatbotContext.json](src/data/chatbotContext.json) containing:

- Complete vehicle specifications and descriptions
- Financial guidelines and affordability rules
- Credit score impact explanations
- Lease vs finance decision frameworks
- Common questions and answers
- Financial tips and best practices

### Example Questions to Ask the Chatbot

- "Which vehicle is best for my budget?"
- "Should I lease or finance the RAV4?"
- "How does my credit score affect my monthly payment?"
- "What down payment should I make on the Corolla?"
- "Is the 4Runner a good financial decision for me?"
- "How can I improve my affordability?"
- "What's the difference between the 2023 and 2024 RAV4?"

---

## 🐛 Troubleshooting

### Charts Not Updating

**Issue**: Charts don't reflect changes to financing controls

**Solution**: Ensure `carFinancingOptions` Map is passed correctly
```typescript
// ✅ Correct
<MonthlyPaymentChart
  cars={selectedCars}
  carFinancingOptions={carFinancingOptions}  // Map is passed
/>

// ❌ Wrong
<MonthlyPaymentChart
  cars={selectedCars}
  // carFinancingOptions not passed - uses defaults
/>
```

### Affordability Badges Not Showing

**Issue**: All vehicles show same affordability

**Solution**: Pass `userProfile` to CarSelector
```typescript
<CarSelector
  cars={toyotaCars}
  selectedCarIds={selectedCarIds}
  onSelectionChange={setSelectedCarIds}
  userProfile={userProfile}  // Required for calculations
  financingOption={defaultOptions.lease}
  recommendedCarIds={recommendedCarIds}
/>
```

---

## 🎓 Learning Resources

### Understanding the Financial Calculations

- [Investopedia: Amortization](https://www.investopedia.com/terms/a/amortization.asp)
- [MyFICO: Credit Score & Loan Rates](https://www.myfico.com/credit-education/calculators/loan-savings-calculator)
- [Edmunds: Depreciation](https://www.edmunds.com/car-buying/how-fast-does-my-new-car-lose-value-infographic.html)

### React & TypeScript

- [Official React Hooks Docs](https://react.dev/reference/react/hooks)
- [Recharts Documentation](https://recharts.org/)

---

## 📄 License

Built with [Lovable](https://lovable.dev) platform.

---

**Made with ❤️ to transform car shopping from transactional to strategic.**
