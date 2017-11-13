# Overview

## Core Structures
### Key Manager
Responsible for managing keys and jobs

#### Requirements
- Manage jobs with more than 100,000 unique keys
- Manage jobs with a range of keys
- Assign blocks of keys to clients
- Free keys that have not been checked in by the client within a timeframe
- Invalidate keys (If a scraper misbehaved)
- Ability to store and restore state

### Data Storage
We need some central place to store the collected and processed data

#### Requirements
- A storage location that is accessible to the scraping pool
  - The client must upload the collected data to this location
  - The Key Master must be able to access the files in this location
  - Authorized users must be able to download these files

#### Example Usage
- Bob submits a job to the Key Master
- Sally's Client downloads the job and the required Scraper
- Sally's scraper fetches multiple blocks of data.
- She then compresses and encrypts them.
- As soon as she finishes a block, she uploads it and checks in the keys with the server
- The Key Master verifies that the data is there and marks her keys as Done
- Bob wants to process the data so he downloads the encrypted blocks from the Data server
- Bob processes the data into a usable form and uploads it to the Data Server
- Sarah downloads Bob's processed data to submit another scraping job

#### Suggestions
- Shared Google Drive folder with encryption

### Client
Communication between the Key Master and the Scraper

#### Requirements
- Get jobs from the Key Master
- Request a block of keys related to a specific job
- Submit a finished block of keys
  - Include a reference to the data in the Data Server
- Release a block of keys
  - If Client no longer wants to scrape them, make available to pool
- Invalidate block of keys
  - If scraper malfunction, request that the keys be freed
- Download a scraper from the Key Master to do the Job
- Upload blocks of data to the Data Storage server

### Scraper
A specific program containing the logic to scrape. Each job may have it's own scraper

#### Requirements
- Given a block/range of keys it will download the data to Local Storage
- Notify the Client of Successfull and Failed keys
- TODO: Interaction between Client and Scraper

### Data Provider
An abstraction to provide an API for the Data Storage.
This will allow us to change backends for the storage without having to modify
other components.
