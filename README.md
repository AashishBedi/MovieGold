# 🎬 Movie Gold

A full-stack movie browsing web application built with **React**, **Spring Boot**, and **MongoDB Atlas**. Browse movies, watch trailers, write reviews, and manage your personal watch list.

---

## 📸 Features

- 🎠 **Movie Carousel** — Browse all movies with backdrop images on the home page
- ▶️ **Watch Trailers** — Embedded YouTube trailer player
- ⭐ **Reviews** — Read and write reviews for each movie
- 🔖 **Watch List** — Save movies to a personal watch list (persisted in localStorage)
- 🔐 **Login / Register** — Frontend authentication with localStorage
- 🌐 **REST API** — Spring Boot backend exposing `/api/v1/movies` and `/api/v1/reviews`
- 📦 **MongoDB Atlas** — Cloud-hosted NoSQL database

---

## 🛠️ Tech Stack

| Layer     | Technology                          |
|-----------|-------------------------------------|
| Frontend  | React 18, React Router v6, Axios    |
| UI        | React Bootstrap, MUI, FontAwesome   |
| Backend   | Spring Boot 3.4.5 (Java 21)         |
| Database  | MongoDB Atlas                       |
| Build     | Maven Wrapper (`mvnw.cmd`)          |

---

## 📁 Project Structure

```
movies/
├── src/
│   └── main/
│       ├── java/bedi/aashish/movies/
│       │   ├── MoviesApplication.java       # Spring Boot entry point + CORS config
│       │   └── movies/
│       │       ├── Movie.java               # Movie entity (MongoDB document)
│       │       ├── MovieController.java     # GET /api/v1/movies
│       │       ├── MovieRepository.java     # MongoRepository
│       │       ├── MovieService.java
│       │       ├── Review.java              # Review entity
│       │       ├── ReviewController.java    # POST /api/v1/reviews
│       │       ├── ReviewRepository.java
│       │       └── ReviewService.java
│       └── resources/
│           ├── application.properties       # MongoDB connection config
│           └── .env                         # Environment variables (gitignored)
├── frontend/
│   └── src/
│       ├── App.js                           # Routes definition
│       ├── api/axiosConfig.js               # Axios base URL config
│       └── components/
│           ├── header/Header.js             # Navbar with Login/Register
│           ├── hero/Hero.js                 # Movie carousel with watchlist button
│           ├── home/Home.js
│           ├── trailer/Trailer.js           # YouTube embed
│           ├── reviews/Reviews.js           # Review form + list
│           ├── reviewForm/ReviewForm.js
│           ├── watchList/WatchList.js       # Watch list page
│           ├── auth/Login.js                # Login page
│           ├── auth/Register.js             # Register page
│           └── notFound/NotFound.js
├── movies.json                              # Sample movie data for MongoDB import
└── pom.xml
```

---

## ⚙️ Prerequisites

| Tool | Version | Download |
|------|---------|----------|
| Java JDK | 21 | [adoptium.net](https://adoptium.net) |
| Node.js | 16+ | [nodejs.org](https://nodejs.org) |
| MongoDB Atlas Account | — | [mongodb.com/atlas](https://www.mongodb.com/atlas) |

---

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone <your-repo-url>
cd movies
```

### 2. Set Up MongoDB Atlas

1. Create a free cluster at [cloud.mongodb.com](https://cloud.mongodb.com)
2. Create a database named **`movie-api-db`** with a collection named **`movies`**
3. Import `movies.json` into the `movies` collection:
   - In MongoDB Compass → Connect to your cluster → `movie-api-db` → `movies` → **Add Data → Import JSON**
4. Whitelist your IP under **Network Access** in the Atlas dashboard

### 3. Configure the Backend

Edit [`src/main/resources/application.properties`](src/main/resources/application.properties):

```properties
spring.application.name=movies
spring.data.mongodb.database=movie-api-db
spring.data.mongodb.uri=mongodb+srv://<USERNAME>:<PASSWORD>@<CLUSTER>.mongodb.net/movie-api-db?retryWrites=true&w=majority
```

> ⚠️ **Important:** If your password contains special characters like `@` or `:`, URL-encode them:
> - `@` → `%40`
> - `:` → `%3A`

### 4. Run the Backend

**Windows (PowerShell):**
```powershell
$env:JAVA_HOME = "C:\Program Files\Java\jdk-21.0.10"
.\mvnw.cmd spring-boot:run
```

**macOS / Linux:**
```bash
./mvnw spring-boot:run
```

Backend starts at: **`http://localhost:8080`**

Verify it's working:
```
http://localhost:8080/api/v1/movies
```

### 5. Run the Frontend

Open a **new terminal**:

```bash
cd frontend
npm install
npm start
```

Frontend opens at: **`http://localhost:3000`**

---

## 🌐 API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/v1/movies` | Get all movies |
| `GET` | `/api/v1/movies/{imdbId}` | Get a single movie by IMDb ID |
| `POST` | `/api/v1/reviews` | Create a review for a movie |

### POST `/api/v1/reviews` — Request Body

```json
{
  "reviewBody": "An amazing movie!",
  "imdbId": "tt3915174"
}
```

---

## 📋 Frontend Routes

| Path | Component | Description |
|------|-----------|-------------|
| `/` | `Home` | Movie carousel |
| `/Trailer/:ytTrailerId` | `Trailer` | YouTube trailer player |
| `/Reviews/:movieId` | `Reviews` | Movie reviews page |
| `/watchList` | `WatchList` | Saved watch list |
| `/login` | `Login` | Login page |
| `/register` | `Register` | Register page |

---

## 🔖 Watch List Feature

- Click the **bookmark icon** on any movie card to add it to your Watch List
- The icon turns **gold** when a movie is saved
- A **toast notification** confirms every add/remove
- Visit **Watch List** in the navbar to see all saved movies
- Each card shows: poster, genres, release date, trailer link, and reviews button
- Movies persist in `localStorage` (survive page refresh)

---

## 🗄️ Sample Data

The `movies.json` file in the project root contains **10 movies** ready to import:

| Title | IMDb ID |
|-------|---------|
| Puss in Boots: The Last Wish | `tt3915174` |
| Avatar: The Way of Water | `tt1630029` |
| M3GAN | `tt8760708` |
| Troll | `tt11116912` |
| Black Adam | `tt6443346` |
| Avatar | `tt0499549` |
| Roald Dahl's Matilda the Musical | `tt3447590` |
| Black Panther: Wakanda Forever | `tt9114286` |
| Strange World | `tt10298840` |
| The Woman King | `tt8093700` |

---

## 🐛 Common Issues & Fixes

| Error | Cause | Fix |
|-------|-------|-----|
| `'.' is not recognized` | Wrong syntax on Windows | Use `.\mvnw.cmd` not `./mvnw` |
| `release version 21 not supported` | Wrong `JAVA_HOME` | Set `$env:JAVA_HOME` to JDK 21 path |
| `invalid user information` | Special char in password | URL-encode `@` as `%40` |
| `${MONGO_CLUSTER}` not resolved | `.env` in wrong location | Hardcode values in `application.properties` |
| Whitelabel Error on `/` | No route at root — **this is normal** | Visit `/api/v1/movies` instead |
| "This page could not be found" | Wrong URL format | Use `/Reviews/tt3915174`, not `/tt3915174` |
| CORS error in browser | Missing `@CrossOrigin` | Already fixed on both controllers |

---

## 🔒 Security Notes

- The `application.properties` file contains your MongoDB password — **do not commit it to a public repository**
- Add `src/main/resources/application.properties` to `.gitignore` for production
- The login/register feature uses `localStorage` for demo purposes — not suitable for production use

---

## 🤝 Acknowledgements

- Movie data and posters sourced from [TMDB](https://www.themoviedb.org/)
- Tutorial inspiration: [freeCodeCamp Full Stack Spring Boot + React](https://www.youtube.com/watch?v=5PdEmeopJVQ)

---

## 📄 License

This project is for educational purposes. Feel free to fork and extend it.
