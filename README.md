# Crypto Event Monitor

An automated blog application that fetches news and price data about Dogecoin and generates blog posts using AI. This project is built for a Zentyal server environment.

## Key Features

* **Automated Content Creation:** Fetches data from external APIs (NewsAPI, CoinGecko).
* **AI-Powered Posts:** Uses the Google Gemini API to write blog posts based on collected data.
* **Simple Web Interface:** Displays posts on a clean, server-rendered frontend.
* **Scheduled Updates:** Uses cron jobs to automatically check for news and create new content.

## Technology Stack

* **Backend:** Python 3.13, Flask
* **Frontend:** HTML, CSS, Jinja2
* **Database:** MariaDB
* **Server:** Zentyal with Apache
* **APIs:** Google Gemini API, NewsAPI.org, CoinGecko API

## Getting Started

Instructions on how to set up and run the project locally.

### Prerequisites

* Python 3.8+
* pip & venv
* Git

### Installation

1.  Clone the repo:
    ```sh
    git clone [https://github.com/Jezuit_dev/dogecoin-news-blog.git](https://github.com/twoja-nazwa-uzytkownika/dogecoin-news-blog.git)
    ```
2.  Navigate to the project directory:
    ```sh
    cd dogecoin-news-blog
    ```
3.  Create and activate a virtual environment:
    ```sh
    python -m venv venv
    source venv/bin/activate  # On Windows use `venv\Scripts\activate`
    ```
4.  Install required packages:
    ```sh
    pip install -r requirements.txt
    ```
5.  Set up your environment variables (e.g., API keys) in a `.env` file.

## Usage

To run the application locally:
```sh
flask run
```
