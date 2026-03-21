# Yakamon - Enterprise REST Backend

> **Showcase Repository:** To comply with EPITA's anti-plagiarism regulations, the raw source code of this backend project is kept private. This repository serves as an architectural showcase documenting the API design, the ORM modeling, and the core layered structure.

## 📖 Project Context

**Yakamon** is an EPITA development project where the objective is to build a robust, scalable backend API for a grid-based creature-collection game. The backend manages player state, 2D map traversal, inventory, and creature encounters, enforcing strict server-side rules and cooldowns to prevent cheating. 

## 🏗️ Architectural Overview

The application is built using the **Quarkus** framework and follows a strict multi-tier architecture to ensure modularity and separation of concerns.

### 1. Presentation Layer (REST & DTOs)
* Handled by JAX-RS (Jakarta RESTful Web Services).
* Exposes standard HTTP endpoints (`/player`, `/move`, `/yakadex`, etc.).
* Implements strict **Data Transfer Objects (DTOs)** for both Requests and Responses to ensure the internal database models are never exposed directly to the client.

### 2. Business Logic Layer (Services)
* Acts as the brain of the application.
* Implements complex game rules: validating if a target tile is walkable based on the terrain type, calculating cooldowns between moves, and managing creature capture probabilities.
* **Converters** are used to translate Entities from the Data Layer into DTOs for the Presentation Layer.

### 3. Data Access Layer (Hibernate ORM)
* Manages persistence using **Hibernate ORM** with the Active Record / Repository pattern.
* Defines relational entities (Player, Game, Yakamon, Item) mapped to a PostgreSQL database.

## 🚀 Technical Highlights

### ⏱️ Server-Side Delay & Cooldown Management
To prevent clients from spamming actions (like moving or gathering resources infinitely), the backend enforces strict, environment-variable-driven delays (`JWS_MOVEMENT_DELAY`, `JWS_CATCH_DELAY`). 
The service layer calculates the time delta between the current request and the `lastMove` timestamp stored in the database, rejecting premature requests with standard HTTP error codes (e.g., `429 Too Many Requests`).

### 🗺️ Map Parsing & State Persistence
The game map is injected dynamically and parsed into a 2D grid of `TileType` objects. The backend persists the player's `X` and `Y` coordinates in the database, validating every directional movement against the grid boundaries and the walkability constraints of specific terrains (e.g., Water, Lava, Mountain).

## 🎯 API Demonstration

*(Visual proof of the API capabilities)*

### Swagger UI / OpenAPI Specification
The API endpoints are fully documented and testable via Swagger UI.
![Swagger UI Screenshot](img/API.png)
