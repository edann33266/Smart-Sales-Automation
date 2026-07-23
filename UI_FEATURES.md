# Frontend UI Features Guide

## 🎨 Visual Design Elements

### Header
```
┌─────────────────────────────────────────────────────────┐
│ ⚡ SalesAI                    Backend: ● Online         │
│   AI-Powered Sales Outreach Platform                    │
└─────────────────────────────────────────────────────────┘
```
- **Logo:** Lightning bolt icon in gradient box
- **Status Indicator:** Live dot (green=online, red=offline, yellow=checking)
- **Sticky:** Stays visible when scrolling

---

### Leads Sidebar (Left)
```
┌─────────────────────────────┐
│ 👥 LEADS              [2]   │
├─────────────────────────────┤
│ ┌─────────────────────────┐ │
│ │ Atharv Bhardwaj         │ │
│ │ 🏢 Acme Analytics       │ │
│ │           ⭐92 ✓Selected│ │
│ └─────────────────────────┘ │
│ ┌─────────────────────────┐ │
│ │ Bob Smith               │ │
│ │ 🏢 Smith & Co           │ │
│ │                    ⭐78 │ │
│ └─────────────────────────┘ │
└─────────────────────────────┘
```

**Features:**
- Lead counter badge
- Hover effects with gradient
- Selected state highlighting
- Color-coded score badges (green ≥80, gray <80)
- "Selected" badge for qualified leads
- Custom scrollbar

---

### Lead Details Card (Top Right)
```
┌─────────────────────────────────────────────────────────┐
│ 👤 LEAD DETAILS                    [Generate Email]     │
├─────────────────────────────────────────────────────────┤
│ ┌──────────────┐  ┌──────────────┐                     │
│ │ 👤 Name      │  │ 🏢 Company   │                     │
│ │ Atharv       │  │ Acme         │                     │
│ └──────────────┘  └──────────────┘                     │
│ ┌──────────────┐  ┌──────────────┐                     │
│ │ ⭐ Score     │  │ 🕐 Contact   │                     │
│ │ 92           │  │ 45 days ago  │                     │
│ └──────────────┘  └──────────────┘                     │
└─────────────────────────────────────────────────────────┘
```

**Features:**
- Gradient header (blue → cyan)
- Color-coded stat cards
- Icons for each field
- Generate button in header
- Responsive grid layout

---

### Email Composer Card (Bottom Right)
```
┌─────────────────────────────────────────────────────────┐
│ ✉️ AI-GENERATED EMAIL                  [Send Email]     │
├─────────────────────────────────────────────────────────┤
│ 💬 EMAIL CONTENT                                        │
│ ┌─────────────────────────────────────────────────────┐ │
│ │ Subject: ...                                        │ │
│ │                                                     │ │
│ │ Dear Atharv,                                        │ │
│ │                                                     │ │
│ │ [AI-generated email content]                       │ │
│ │                                                     │ │
│ └─────────────────────────────────────────────────────┘ │
│                                                         │
│ ℹ️ AI MANAGER REASONING                                 │
│ ┌─────────────────────────────────────────────────────┐ │
│ │ Chose value_focus agent because...                 │ │
│ └─────────────────────────────────────────────────────┘ │
│                                                         │
│              [📤 Send Email Now]                        │
└─────────────────────────────────────────────────────────┘
```

**Features:**
- Gradient header (emerald → teal)
- Two send buttons (header + main)
- Editable email textarea
- Read-only reasoning textarea
- Large prominent send button at bottom
- Icons for visual hierarchy

---

## 🎯 Interactive Elements

### Buttons

#### Generate Email Button
- **Location:** Lead Details card header
- **State:** Disabled until lead selected
- **Loading:** Shows spinner + "Processing..."
- **Colors:** White text on semi-transparent white background
- **Hover:** Scales up slightly (1.05x)

#### Send Email Buttons (2x)
- **Location 1:** Email Composer header
- **Location 2:** Bottom of Email Composer
- **State:** Disabled until email generated
- **Loading:** Shows spinner + "Processing..."
- **Colors:** Emerald gradient background
- **Hover:** Darker gradient + scale up

### Lead Selection
- **Click:** Selects lead
- **Visual:** Gradient background + border highlight
- **Effect:** Smooth transition
- **Feedback:** Updates detail panel immediately

---

## 🎨 Color Palette

### Gradients
```
Primary:   Indigo (#6366f1) → Purple (#8b5cf6)
Secondary: Blue (#3b82f6) → Cyan (#06b6d4)
Success:   Emerald (#10b981) → Teal (#14b8a6)
Warning:   Orange (#f97316) → Red (#ef4444)
```

### Backgrounds
```
Main:      Slate → Blue → Indigo gradient
Cards:     White with 90% opacity + backdrop blur
Headers:   Gradient specific to each card
```

### Text
```
Primary:   Gray-900 (#111827)
Secondary: Gray-600 (#4b5563)
Muted:     Gray-500 (#6b7280)
Light:     Gray-400 (#9ca3af)
```

---

## 🔔 Toast Notifications

### Success Toast
```
┌─────────────────────────────────┐
│ ✓ Email generated successfully! │
└─────────────────────────────────┘
```
- **Icon:** Green checkmark
- **Duration:** 3 seconds
- **Animation:** Slide up from bottom-right

### Error Toast
```
┌─────────────────────────────────┐
│ ✗ Error generating email: ...   │
└─────────────────────────────────┘
```
- **Icon:** Red X
- **Duration:** 3 seconds
- **Animation:** Slide up from bottom-right

### Info Toast
```
┌─────────────────────────────────┐
│ ℹ Backend connection checking... │
└─────────────────────────────────┘
```
- **Icon:** Blue info circle
- **Duration:** 3 seconds
- **Animation:** Slide up from bottom-right

---

## 🎭 Animations

### Hover Effects
- **Buttons:** Scale 1.05x + shadow increase
- **Lead items:** Gradient background fade-in
- **Cards:** Subtle lift (translateY -2px)

### Loading States
- **Spinner:** Rotating circle animation
- **Status dot:** Pulse animation when checking
- **Buttons:** Disabled state with reduced opacity

### Transitions
- **All elements:** 200ms ease timing
- **Background colors:** Smooth fade
- **Transforms:** Smooth scale/translate

---

## 📱 Responsive Design

### Desktop (≥1024px)
- 3-column layout (1 sidebar + 2 main)
- Full feature visibility
- Hover effects enabled

### Tablet (768px - 1023px)
- 2-column layout
- Sidebar collapses to dropdown
- Touch-friendly buttons

### Mobile (<768px)
- Single column stack
- Full-width cards
- Larger touch targets
- Simplified navigation

---

## ✨ Special Features

### Custom Scrollbar
- **Width:** 8px
- **Track:** Light gray with rounded corners
- **Thumb:** Indigo → Purple gradient
- **Hover:** Darker gradient

### Glass-morphism
- **Effect:** Backdrop blur + semi-transparent white
- **Usage:** All cards and header
- **Result:** Modern, layered appearance

### Gradient Text
- **Usage:** Main title "SalesAI"
- **Effect:** Indigo → Purple gradient clipped to text
- **Result:** Eye-catching header

---

## 🎯 User Flow

1. **Page Load**
   - Backend status checks automatically
   - Leads load from API
   - Sidebar populates with lead cards

2. **Select Lead**
   - Click lead in sidebar
   - Lead highlights with gradient
   - Details populate in right panel
   - Generate button enables

3. **Generate Email**
   - Click "Generate Email"
   - Button shows loading spinner
   - API calls backend
   - Email populates in textarea
   - Reasoning shows in bottom box
   - Send buttons enable

4. **Send Email**
   - Review/edit email if needed
   - Click either send button
   - Button shows loading spinner
   - API sends email
   - Success toast appears

---

## 🎨 Design Philosophy

### Principles
1. **Clarity:** Clear visual hierarchy
2. **Feedback:** Immediate response to actions
3. **Beauty:** Modern, professional aesthetics
4. **Simplicity:** Intuitive workflow
5. **Delight:** Smooth animations and transitions

### Inspiration
- Modern SaaS dashboards
- Glass-morphism trend
- Gradient design systems
- Micro-interactions

---

Enjoy the beautiful new interface! 🎉
