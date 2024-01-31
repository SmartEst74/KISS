# KISS
The Kucoin Ingestion of Symbols Services written in Rust.

## Overview

KISS is a lightweight, real-time service for tracking cryptocurrency prices on KuCoin. It uses WebSockets to stream live price data, stores this data in InfluxDB for historical analysis, and provides a simple web interface to display live prices. Designed to run efficiently on Raspberry Pi, this project is ideal for personal use or for developers interested in cryptocurrency data.
[Project](https://github.com/SmartEst74/KISS/projects)

## Features

- Real-time price tracking of cryptocurrencies on KuCoin.
- Historical price data storage using InfluxDB.
- Health check and automatic reconnection to WebSockets.
- Simple web front-end to display live cryptocurrency prices.
- Lightweight and efficient, suitable for Raspberry Pi.
- Unit and integration tests for reliability.

## Installation

### Prerequisites

- Rust programming environment
- InfluxDB
- Node.js and npm (for the web front-end)

### Rust and Cargo

Ensure Rust and Cargo are installed by following the official [Rust installation guide](https://www.rust-lang.org/tools/install).

### InfluxDB

Install InfluxDB by following the instructions on the [InfluxData website](https://portal.influxdata.com/downloads/).

### Clone the Repository

```bash
git clone https://github.com/yourusername/kucoin_tracker.git
cd kucoin_tracker
```

### Install Dependencies

```bash
cargo build
```

## Usage

To start the KuCoin Tracker service:

```bash
cargo run
```

The web interface will be available at `http://localhost:3000` (adjust the port as necessary in your setup).

## Development

To contribute to the project or set up a development environment, follow the standard Rust development practices:

- Use `cargo build` to compile the project.
- Use `cargo run` to run the application locally.

## Testing

This project includes unit and integration tests to ensure functionality and reliability.

- Run unit tests with `cargo test --lib`.
- Integration tests can be run with `cargo test --test integration`.

## Contributing

Contributions are welcome! Please open an issue to discuss proposed changes or submit a pull request.

1. Fork the repo.
2. Create your feature branch (`git checkout -b feature/AmazingFeature`).
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`).
4. Push to the branch (`git push origin feature/AmazingFeature`).
5. Open a pull request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.
```

Remember to replace placeholders like `https://github.com/yourusername/kucoin_tracker.git` with your actual repository URL and adjust any specific commands or URLs to match your project's setup and naming conventions. Also, ensure you include a `LICENSE` file in your repository if you reference it in the README.
```
# AI Guidance
I'm relying heavily on AI to produce the service and I'm sharing my prompts below.

## Question
Hello again - can you help me to create a simple service that will keep a live tracker on kucoin prices. I'd also like it to store previous prices. The service must be powerful enough to ingest and retain all of the data in real time by using the web sockets. The service must be able to check the health of the web sockets and restart them if there's an issue. The idea behind this service is that I can keep the data long term without having to pay an external resource (they don't stay free). I want to run this service on my Raspberry PI so it has to be lightweight and robust. I want to be able to connect to the data from my Mac on the same network and gather the stored or live data instantly. Is this possible please? I'm happy to use go or rust as they are the most robust.

## ChatGPT4 Response
Yes, it's possible to create a lightweight and robust service to track and store live cryptocurrency prices from KuCoin using WebSockets, and to run it on a Raspberry Pi. Given your requirements, here's a high-level overview of how you could approach this in either Go or Rust, considering both languages are well-suited for building performant and efficient systems-level applications.

### Service Overview:

1. **WebSocket Client**: Connects to the KuCoin WebSocket API to stream live price data.
2. **Data Storage**: Stores historical price data locally, possibly using a lightweight database or a time-series database optimized for such use cases.
3. **Health Check & Recovery**: Periodically checks the health of the WebSocket connection and attempts to reconnect if the connection drops.
4. **Data Access**: Provides an API or a network service on your Raspberry Pi to allow your Mac to query live or historical data.

### Implementation Steps:

#### Step 1: Setup Your Development Environment

- Install Go or Rust on your Raspberry Pi and Mac.
- Ensure you have network connectivity between your Raspberry Pi and Mac.

#### Step 2: Connect to KuCoin WebSocket API

- **Go**: Use a WebSocket library like `gorilla/websocket`.
- **Rust**: Use a WebSocket library like `tokio-tungstenite` (with async support).

#### Step 3: Store Historical Price Data

- Consider using SQLite for a lightweight setup or InfluxDB if you prefer a time-series database.
- Structure your database to efficiently query historical data.

#### Step 4: Health Check & Recovery Mechanism

- Implement a periodic ping to the WebSocket server to ensure the connection is alive.
- On detecting a connection drop, implement a reconnection logic.

#### Step 5: Data Access Service

- Create a simple HTTP server in Go or Rust.
- Implement endpoints to query live data (straight from the WebSocket connection) and historical data (from your database).

#### Step 6: Ensure Lightweight and Robust Operation

- Optimize your code and database queries for performance and low resource consumption.
- Test the system under various conditions to ensure stability.

#### Step 7: Setup on Raspberry Pi

- Deploy your service on the Raspberry Pi.
- Ensure it starts on boot and runs as a background service.

#### Step 8: Accessing Data from Your Mac

- Connect to your Raspberry Pi's data access service over your local network.
- You could use a simple REST client or even a custom frontend to display the data.

### Considerations:

- **Language Choice**: Both Go and Rust are excellent choices. Go might offer a bit more simplicity and better concurrency support out of the box, while Rust provides more control over memory and safety guarantees.
- **Database**: Your choice of database will significantly impact the system's performance and storage efficiency. SQLite is very lightweight and usually sufficient for simple use cases, but a time-series database like InfluxDB might offer better performance for time-series data.
- **Security**: Ensure that your data access service is secured, especially if it's accessible over the network.
- **Power Failures**: Consider implementing a mechanism to handle unexpected shutdowns to prevent data corruption, especially important when running on devices like Raspberry Pi.

This project will require some familiarity with network programming, databases, and possibly asynchronous programming, especially if you choose Rust with an async WebSocket library. It's a great project to deepen your understanding of these areas!


## Question
Let's use Rust. I'd like to set everything up on the Mac and emulate the functional of the raspberry pi once it's working on the Mac to. The code will use tokio-tungstenite and InfluxDB. I'd like the code to include unit and integration testing. A web front-end will display the live prices to begin with. Something very simple. Can you provide the best method to prompt you so that we can work together on this journey please? I'd like you to take me step by step to setting up Rust and Influx, displaying the live data in a web page and implementing unit and integration testing.

## ChatGPT4 Response
Absolutely, I can guide you through setting up and implementing this project in Rust, using `tokio-tungstenite` for WebSocket connectivity to stream live prices from KuCoin, and `InfluxDB` for storing time-series data. We'll also set up a simple web front-end to display live prices and cover unit and integration testing to ensure the reliability of your service.

### Step 1: Setting Up Your Rust Environment

1. **Install Rust:**
   - If you haven't already, install Rust on your Mac. The easiest way is through [rustup](https://rustup.rs/), which is the official Rust toolchain installer.
   - Run the following command in your terminal: `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`
   - Follow the on-screen instructions to complete the installation.

2. **Update Rust:**
   - Ensure your Rust installation is up to date by running `rustup update` in your terminal.

3. **Install Cargo:**
   - Cargo, the Rust package manager, should be installed automatically with Rust. You can verify by running `cargo --version` in your terminal.

### Step 2: Setting Up InfluxDB

1. **Install InfluxDB:**
   - You can download InfluxDB from the [InfluxData website](https://portal.influxdata.com/downloads/). Choose the version that's compatible with your Mac.
   - Follow the installation instructions provided on the website or in the downloaded package.

2. **Configure InfluxDB:**
   - Once installed, start InfluxDB and ensure it's running by accessing the web interface, usually at `http://localhost:8086`.

3. **Create Database:**
   - Use the InfluxDB CLI or web interface to create a new database for storing your cryptocurrency prices.

### Step 3: Project Setup

1. **Create a New Rust Project:**
   - Open your terminal and navigate to the directory where you want to create your project.
   - Run `cargo new kucoin_tracker --bin` to create a new binary project named `kucoin_tracker`.
   - Navigate into your project directory using `cd kucoin_tracker`.

2. **Add Dependencies:**
   - Edit your `Cargo.toml` file to include the necessary dependencies for `tokio-tungstenite`, `InfluxDB` client for Rust, and any other libraries you might need.

   ```toml
   [dependencies]
   tokio = { version = "1", features = ["full"] }
   tokio-tungstenite = "0.15"
   serde = "1.0"
   serde_json = "1.0"
   influxdb = "0.3.0"  # This version might change; please check the latest on crates.io
   ```

### Step 4: Implementing WebSocket Connectivity

1. **WebSocket Client:**
   - Use `tokio-tungstenite` to establish an asynchronous WebSocket connection to the KuCoin WebSocket API.
   - Implement message handling to parse and process incoming price data.

2. **InfluxDB Integration:**
   - Use the `influxdb` Rust client to connect to your InfluxDB instance and insert the price data into your database.

### Step 5: Web Front-End

1. **Setup a Simple Web Server:**
   - You can use frameworks like `actix-web` or `warp` to set up a simple async web server that serves a web page.

2. **Web Page:**
   - Create a basic HTML page to display live prices. You can use WebSocket in the browser to connect to your Rust backend and display live updates.

### Step 6: Testing

1. **Unit Testing:**
   - Write unit tests for your Rust functions, focusing on the logic that doesn't depend on external services like InfluxDB or the WebSocket connection.

2. **Integration Testing:**
   - For integration testing, you might need to mock the WebSocket server and InfluxDB to test your application's behavior in a controlled environment.

### Step 7: Running and Debugging

1. **Run Your Project:**
   - Use `cargo run` to build and run your project.
   - Debug any issues that arise, using Rust's compiler errors and warnings to guide you.

### Step 8: Deployment on Raspberry Pi

- Once everything is working on your Mac, you can move your project to the Raspberry Pi. Ensure Rust and InfluxDB are set up on your Pi, then transfer your project and run it.

### Step by Step Guidance

To tackle this project effectively, break it down into smaller, manageable tasks. Focus on one aspect at a time, for example, start by establishing the WebSocket connection and logging received data. Next, integrate InfluxDB and ensure data is stored correctly. Then, develop the web front-end, and finally, focus on testing
