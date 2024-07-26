# Weather Application

This is a weather application built using Nuxt.js, Tailwind CSS and Docker.

## Installation

1. Clone the repository
    ```sh
    git clone https://github.com/your-repo/weather-app.git
    cd weather-app
    ```

2. Install dependencies
    ```sh
    npm install
    ```

## Running Locally

1. Start the development server
    ```sh
    npm run dev
    ```

2. Open [http://localhost:3000](http://localhost:3000) in your browser

## Docker Deployment

### Requirements

- Docker
- Docker Compose

### Build and Run

1. Build the Docker image
    ```sh
    make build
    ```

2. Start the application
    ```sh
    make up
    ```

3. View logs
    ```sh
    make logs
    ```

4. Stop the application
    ```sh
    make down
    ```

## Validation

- The input field must not be empty
- The input must be a valid city name (no numbers)