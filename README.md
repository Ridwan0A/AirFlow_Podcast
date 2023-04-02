# Project Overview

This is an Airflow DAG that scrapes a podcast RSS feed and downloads the audio files for new episodes. It then transcribes the audio files into text and stores the data in a database.

## Requirements

- Airflow
- Vosk
- PyDub
- xmltodict
- requests
- SQLite
- Setup

## Setup

1. Install the required packages
2. Copy the code snippet into a Python file and save it
3. Configure Airflow to recognize the DAG file by either copying it to the dags folder or adding its directory to the DAGS_FOLDER variable in the Airflow config file
4. Create a SQLite database and set its connection ID in the code
5. Update the PODCAST_URL and EPISODE_FOLDER variables if necessary
6. Run Airflow

# DAG Details

This DAG has the following tasks:

## create_table_sqlite

This task creates a SQLite table to store the podcast data if it does not exist.

## get_episodes

This task makes a GET request to the podcast RSS feed, parses the XML response into a Python dictionary, and returns a list of episodes.

## load_episodes

This task loads the podcast episodes into the SQLite database. It checks if each episode already exists in the database and only inserts new episodes. It also generates a filename for each episode's audio file.

## download_episodes

This task downloads the audio files for the podcast episodes that were inserted in the previous task. It checks if each audio file already exists in the specified folder and only downloads new files.

## speech_to_text

This task transcribes the audio files into text using Vosk and saves the results in the SQLite database. It only transcribes episodes that do not have a transcript yet.

## Dependencies

create_table_sqlite is the first task in the DAG and has no dependencies. get_episodes depends on create_table_sqlite. load_episodes depends on get_episodes. download_episodes depends on load_episodes. speech_to_text depends on download_episodes.

The DAG is scheduled to run daily and does not backfill past dates.
