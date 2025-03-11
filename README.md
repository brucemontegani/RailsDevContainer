# Rails 8 DevContainer Template

## Overview
This repository serves as a **template** for setting up a fully functional **Ruby on Rails 8 development environment** using **DevContainers** and **Docker Compose**. It includes all required dependencies and allows you to develop Rails applications using **SQLite, PostgreSQL, or MySQL** within a containerized environment.

## Features
- 🐳 **Docker-based** development setup for full isolation.
- ⚡ **Supports SQLite, PostgreSQL, and MySQL** with dynamic selection.
- 🖥️ **Pre-configured VSCode DevContainer** for seamless development.
- 🛠️ **Automatic dependency installation** (Rails, databases, Zsh, Oh-My-Zsh, Pure prompt).
- 🔄 **Unique database volumes** per project to prevent conflicts.

## Getting Started
### 1️⃣ Clone the Template Repository
Click **"Use this template"** on GitHub or manually clone the repository:
```sh
git clone <repo-url> my-rails-app
cd my-rails-app
```

### 2️⃣ Set Up Environment Variables
Create a `.env` file and configure your settings:
```sh
# Define workspace name for unique volumes
LOCAL_WORKSPACE_NAME=my_rails_app

# Select database (sqlite, postgres, mysql)
DATABASE=postgres
```

### 3️⃣ Start the DevContainer
1. Open the project in **VSCode**.
2. Press **Cmd + Shift + P** (Mac) or **Ctrl + Shift + P** (Windows/Linux).
3. Select **"Dev Containers: Reopen in Container"**.

### 4️⃣ Run Rails Setup
Once inside the DevContainer, initialize your Rails application:
```sh
rails new . --force --database=$DATABASE
bin/setup
```

### 5️⃣ Start Your Rails Server
```sh
rails server -b 0.0.0.0
```
Your application will be available at `http://localhost:3000`.

## Database Support
This template dynamically starts the correct database service based on the `DATABASE` environment variable.

| Database  | Command to Start |
|-----------|-----------------|
| SQLite    | No external service required |
| PostgreSQL | `docker compose --profile postgres up --build` |
| MySQL     | `docker compose --profile mysql up --build` |

## Customization
- Modify the **`Dockerfile`** to install additional dependencies.
- Update **`devcontainer.json`** for custom VSCode settings.
- Configure **Rails credentials** or secrets management as needed.

## Cleanup
To remove the project’s database volumes:
```sh
docker volume rm ${LOCAL_WORKSPACE_NAME}_postgres-data ${LOCAL_WORKSPACE_NAME}_mysql-data
```

## Contributing
Feel free to submit issues or pull requests to improve this template!

## License
This project is licensed under the **MIT License**.

