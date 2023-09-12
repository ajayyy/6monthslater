# 6 Months Later

A Long-term Product Reliability Review Platform

6 Months Later a web application where various consumer goods are given reliability ratings over longer periods of time (6+ months). Most reviews written these days only cover the product at launch and not how it holds up several months (or years) later.

More information available in our [wiki](https://github.com/6monthslater/6monthslater/wiki)

## Building and Running

Folder Structure:

`├── server` The Remix web server application

`├── docker` Configuration files for running Postgres and RabbitMQ in docker

`├── scraper` The Python scraper application and it's shared files

`│   ├── crawler.py` The Python Crawler application. Starts a daemon that waits for messages in RabbitMQ

`│   ├── scraper.py` The Python Scraper application.  Starts a daemon that waits for messages in RabbitMQ

`│   ├── analyzer.py` The Python Analysis application

### Postgres and RabbitMQ

Postgres is used by the web server to store data and RabbitMQ is used by all services to communicate with eachother.

These can be started with docker using `docker-compose up`

### Web Server

The web server is a remix application.

You first must install the dependencies.

```bash
cd server
npm install
```

Then you can start the server with `npm run dev` or `npm run start` in production.

### Scraper

The scraper is a Python application and is divided into three parts.

Firstly, run the `setup.sh` bash script to install the SUTime library used by the analyzer. The script may fail if Maven is not installed on your system: if `mvn -v` is not recognized as a command, please first [install Maven](https://maven.apache.org/install.html). If the script succeeds, the folder `analyzer/jars` should now exist.

You must then install create a Python Virtual Environment and install the remaining dependencies.

```bash
cd scraper
python3 -m venv venv
source venv/bin/activate  # On Windows: .\venv\Scripts\activate

pip install -r requirements.txt
python -m textblob.download_corpora
```

Then you can start the daemons with `python crawler.py` and `python scraper.py`.

These daemons will wait for messages in RabbitMQ, and execute accordingly.

The analyzer is currently not a daemon and currently only analyzes sample reviews hardcoded in the file.
The provided sample reviews (at the end of `analyzer.py`) test various capabilities of the analyzer; you can modify them, remove or add new ones by providing the review text and posting date in the same format. Then, you can run the tests with `python analyzer.py`.