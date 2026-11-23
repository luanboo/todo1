# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A deliberately minimal personal to-do tracker: plain HTML/CSS/vanilla JS, **no frameworks, no build step, no tests, no package manager**. Run it by opening `index.html` in a browser. Keep it that way: the owner adds features one small request at a time, so only add what is asked.

## Files

- `index.html` – the entire app (inline `<style>` and `<script>`).
- `mockup.html` – a Pixel 10 phone-frame page that embeds `index.html?demo` in an iframe (412x915). The iframe needs a real file/http origin; it renders blank in the Claude Browser pane, which serves local files as `data:` URLs.
- `mockup-pixel10.jpg` – a static screenshot of the app at 412x915.

## Architecture (index.html)

- State is an in-memory `todos` array of `{text, cat, due, done}`. **It is not persisted**; reloading clears it. Only the theme choice is saved (localStorage, in try/catch). Import/Export is the way to keep a list.
- `render()` rebuilds the whole `<ul>` from `todos` using DOM APIs and `textContent` (never `innerHTML`). Mutations change `todos` and then call `render()`.
- Edit mode is the `editing` index; `render()` draws inline fields for that row (Enter saves, Escape cancels).
- `due` is an ISO `YYYY-MM-DD` string compared with the local-date `today()`; `fmt()` formats it for display. Overdue = not done and `due < today()`.
- Categories come from the `CATEGORIES` array; `makeSelect()` fills both the add form and edit selects, and unknown values fall back to `Others`.
- Theming: CSS custom properties on `:root`, overridden by `:root[data-theme=dark]`; `setTheme()` sets `document.documentElement.dataset.theme`, defaulting to `prefers-color-scheme`.
- Import accepts a `.json` array (items as objects or plain strings) or a text file with one item per line, and sanitizes `cat`/`due`/`done`. Export downloads `todo-YYYY-MM-DD.json` in the same format, so it round-trips.
- Opening `index.html?demo` seeds sample items (used by the mockup).

## Conventions

- Layout must stay usable at phone width (checked at 412px with no horizontal scroll); the form wraps with the text field on its own full-width row.
