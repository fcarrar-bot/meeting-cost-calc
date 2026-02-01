# Meeting Cost Calculator

## Overview
A simple, fun web app that calculates the real-time cost of a meeting based on the number of attendees and their hourly rates. Shows a live counter ticking up as the meeting progresses.

## Why This?
- Meetings are expensive and often unnecessary
- Visual shock value: seeing "$487.23 and counting..." makes the point
- Highly shareable on social media
- Simple to build, memorable to use

## Target Users
- Managers who want to justify meeting audits
- Employees who want to push back on unnecessary meetings
- HR/Operations looking at meeting culture

## Core Features

### MVP (Tonight)
1. **Attendee Setup**
   - Add attendees with role/title presets OR custom hourly rate
   - Quick presets: Junior ($35/hr), Mid ($55/hr), Senior ($85/hr), Manager ($110/hr), Director ($150/hr), Executive ($250/hr)
   - Show attendee count and combined hourly rate

2. **Live Timer**
   - Start/pause/reset meeting timer
   - Shows elapsed time
   - Shows cost ticking up in real-time (updates every 100ms for drama)
   - Animated dollar counter

3. **Summary**
   - Total meeting cost
   - Cost per minute
   - "This meeting cost more than X" comparisons (coffee, lunches, monthly subscriptions)
   - Share button (generates shareable image or link)

### Design Notes
- Dark mode default (looks more dramatic)
- Large, bold numbers for the cost counter
- Subtle animation on the ticking dollars
- Mobile-responsive
- Single page app (no backend needed for MVP)

## Tech Stack
- **Frontend**: React or vanilla JS with Tailwind CSS
- **Hosting**: GitHub Pages or Vercel
- **No backend needed** for MVP (all client-side)

## Team Assignments

### Scout (Research) ✓ DONE
- Market research on meeting cost tools
- Salary data for presets

### Ink (Copy)
- Headline and tagline
- Preset role names
- Comparison phrases ("This meeting cost more than...")
- CTA copy
- Meta/OG description for sharing

### Pixel (Design)
- Color scheme and typography
- Layout mockup
- Counter animation specs
- Mobile responsive approach
- Share card design

### Bolt (Dev)
- Full implementation
- GitHub repo setup
- Deployment to GitHub Pages
- README

## Deliverables
- Live app on GitHub Pages
- Clean GitHub repo with README
- Shareable on social media

## Timeline
- Tonight: Full MVP shipped and deployed

---

Created: 2026-02-01
Status: In Progress
