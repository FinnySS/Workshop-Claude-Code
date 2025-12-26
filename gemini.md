# GEMINI.md - Project Guidelines & Documentation

## 📚 Table of Contents

1. Executive Summary
2. Quick Start Guide
3. Project Context
4. Critical Safety Rules
5. Development Environment
6. Development Workflows
7. Context Management & Short Codes
8. Technical Reference
9. Development Practices
10. Lessons Learned
11. Troubleshooting
12. Appendices

## Executive Summary

This document provides comprehensive guidelines for the AI assistant working on the Tetris project. It establishes safe, efficient, and well-documented workflows to ensure high-quality contributions.

### Key Responsibilities

• Code development and implementation
• Testing and quality assurance
• Documentation and session retrospectives
• Following safe and efficient development workflows
• Maintaining project context and history

### Quick Reference - Short Codes

#### Context & Planning Workflow (Core Pattern)

•  ccc  - Create context issue and compact the conversation.
•  nnn  - Smart planning: Auto-runs  ccc  if no recent context → Create a detailed implementation plan.
•  gogogo  - Execute the most recent plan issue step-by-step.
•  lll  - List project status (issues, PRs, commits) ✅

#### Project Management

•  rrr  - Create a detailed session retrospective.

## Quick Start Guide

### Prerequisites

    # Check required tools
    python3 --version
    node --version    # Optional, for alternative server
    git --version
    gh --version      # GitHub CLI

### Initial Setup

    # 1. Clone the repository (if not already done)
    # git clone [repository-url]
    # cd Workshop-Claude-Code

    # 2. Start the Development Server
    # We use setsid to ensure the server persists in the background
    setsid python3 -m http.server 3000 > server.log 2>&1 &
    
    # 3. Verify Server
    sleep 1 && lsof -i :3000

    # 4. Access the Game
    # URL format: https://${CODESPACE_NAME}-3000.${GITHUB_CODESPACES_PORT_FORWARDING_DOMAIN}

### First Task

1. Run  lll  to see the current project status.
2. Run  nnn  to analyze the latest issue and create a plan.
3. Use  gogogo  to implement the plan.

## Project Context

### Project Overview

This project is a classic Tetris game implementation using vanilla Web Technologies. The goal is to create a performant, visually appealing arcade-style game that runs in a browser.

### Architecture

• **Frontend:**
  - **HTML5:** Uses the `<canvas>` element for high-performance 2D rendering.
  - **CSS3:** Flexbox for layout and custom styling for a "Neon/Arcade" aesthetic.
  - **JavaScript (ES6+):** 
    - **Game Loop:** Uses `requestAnimationFrame` for smooth rendering.
    - **Logic:** Matrix-based collision detection and piece rotation.
    - **State Management:** Simple object-based state for player score, level, and board (arena).

• **Backend / Server:**
  - **Python:** `http.server` module (Primary for local dev).
  - **Node.js:** `http` module (Fallback in `server.js`).

• **Infrastructure:**
  - GitHub Codespaces for development.

### Current Features

• **Core Gameplay:**
  - Tetromino rendering (7 shapes with unique colors).
  - Grid-based movement (Left, Right, Soft Drop).
  - Rotation logic (Wall kicks implemented simply).
  - Line clearing and scoring.
  - Game Over state (reset).

• **UI/UX:**
  - Score, Level, and Lines display.
  - Neon visual style.
  - Start/Pause functionality.

## 🔴 Critical Safety Rules

### Repository Usage

• NEVER create issues/PRs on upstream

### Command Usage

• NEVER use  -f  or  --force  flags with any commands.
• Always use safe, non-destructive command options.
• If a command requires confirmation, handle it appropriately without forcing.

### Git Operations

• Never use  git push --force  or  git push -f .
• Never use  git checkout -f .
• Never use  git clean -f .
• Always use safe git operations that preserve history.
• ⚠️ NEVER MERGE PULL REQUESTS WITHOUT EXPLICIT USER PERMISSION
• Never use  gh pr merge  unless explicitly instructed by the user
• Always wait for user review and approval before any merge

### File Operations

• Never use  rm -rf  - use  rm -i  for interactive confirmation.
• Always confirm before deleting files.
• Use safe file operations that can be reversed.

### General Safety Guidelines

• Prioritize safety and reversibility in all operations.
• Ask for confirmation when performing potentially destructive actions.
• Explain the implications of commands before executing them.
• Use verbose options to show what commands are doing.

## Development Environment

### Environment Variables

#### Codespace Variables
    CODESPACE_NAME=           # Used for URL generation
    GITHUB_CODESPACES_PORT_FORWARDING_DOMAIN= # Used for URL generation

## Development Workflows

### Testing Discipline

#### Manual Testing Checklist

Before pushing any changes:

[ ] Run the server: `setsid python3 -m http.server 3000 > server.log 2>&1 &`
[ ] Verify the game loads in the preview URL.
[ ] Test controls (Arrow keys: Left, Right, Up, Down).
[ ] Check for collision bugs (pieces going through walls/floors).
[ ] Verify line clearing and score updates.
[ ] Check browser console for JavaScript errors.

### GitHub Workflow

#### Standard Development Flow

    # 1. Update main branch
    git checkout main && git pull

    # 2. Create a branch
    git checkout -b feat/description

    # 3. Make changes & Test

    # 4. Commit
    git add -A
    git commit -m "feat: Description"

    # 5. Push and PR
    git push -u origin feat/description
    gh pr create

## Context Management & Short Codes

(See Executive Summary for core codes: ccc, nnn, lll, rrr, gogogo)

## Technical Reference

### Game Logic Details
- **The Arena:** 12x20 Matrix. 0 = empty, 1-7 = colors.
- **Collision:** `collide(arena, player)` checks overlap/boundaries.
- **Rotation:** Transpose + Reverse.

## Development Practices

### Code Standards

• **JavaScript:** Use ES6+ features (const/let, arrow functions).
• **Formatting:** Consistent indentation (4 spaces).
• **Comments:** Explain *why*, not *what*.

## Lessons Learned

### Project-Specific Findings (Tetris)

• **Background Process Persistence:**
  - **Issue:** `python3 -m http.server 8080 &` often terminates when the CLI tool finishes.
  - **Solution:** Use `setsid python3 -m http.server 3000 > server.log 2>&1 &` to detach the process from the current session.

• **Port Visibility & URLs:**
  - **Issue:** `localhost` is not accessible from outside the Codespace.
  - **Solution:** Construct URLs dynamically: `https://${CODESPACE_NAME}-${PORT}.${GITHUB_CODESPACES_PORT_FORWARDING_DOMAIN}`.

• **Canvas Coordinate System:**
  - **Tip:** Using `context.scale(20, 20)` allows logic to work in "grid units" (1x1) while rendering in pixels (20x20), simplifying math.

### General Patterns (from CLAUDE.md)

• **Planning:** Break complex tasks into small, verifiable steps.
• **Anti-Pattern:** Trying to implement everything at once.
• **Pattern:** Use parallel agents (if available) for analysis.

## Troubleshooting

### Common Issues

#### Server Not Running
**Symptoms:** Connection refused or site not loading.
**Fix:**
1. Check process: `ps aux | grep python`
2. Check logs: `cat server.log`
3. Restart: `pkill -f http.server && setsid python3 -m http.server 3000 > server.log 2>&1 &`

#### "ERR_EMPTY_RESPONSE" or 404
**Symptoms:** Browser can't find the page.
**Fix:**
1. Ensure you are using the correct Codespace URL (not localhost).
2. Check if the port (3000) is correct.
3. Verify file structure (`index.html` exists).

## Appendices

### A. Glossary
• **Tetromino:** The geometric shapes used in the game (I, J, L, O, S, T, Z).
• **Arena:** The game board matrix.