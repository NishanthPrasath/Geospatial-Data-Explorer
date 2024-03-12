# Data-as-a-Service (DaaS) Platform for Geospatial Data Exploration

> [Application Link](http://54.88.51.70:8501) <br>
> [Codelabs Documentation](https://codelabs-preview.appspot.com/?file_id=1zG832dq7KBnSKgSkrVcLHVQBfarUR8ALQIqqGmswVhE#4)<br>
> [FastAPI Docs](http://54.88.51.70:8000/docs) <br>
> Docker Images: [Backend](https://hub.docker.com/r/subhashchandran/assignment-03-fastapi),[Frontend](https://hub.docker.com/r/subhashchandran/assignment-03-streamlit) <br>
> [Python Package](https://pypi.org/project/typernexrad-cli/0.1.0/) <br>

## Objective 

The primary objective of this project is to create a platform that provides a data retrieval service for satellite images from NOAA's GOES18 and Nexrad AWS S3 buckets. Users can either provide input for image-related attributes or directly provide a filename to generate a link to the file.

**Note:** The data is scraped from publicly accessible data in NOAA's S3 buckets - [GOES18](https://noaa-goes18.s3.amazonaws.com/index.html#ABI-L1b-RadC/) & [NEXRAD](https://noaa-nexrad-level2.s3.amazonaws.com/index.html), which are refreshed daily with Airflow DAGs.

![sevir_sample](https://user-images.githubusercontent.com/114712818/218191862-49f8f32b-bc77-4e03-ae81-9ebac16b514a.gif)

## Use Case

- Users can fetch data either by providing date and station features or by providing a valid file name.
- A downloadable link to the file is provided for both NOAA's and private buckets.
- A plot of all NEXRAD stations across the US is available.

## Tech Stack

- Backend: FastAPI, Airflow, AWS Cloudwatch
- Frontend UI: Streamlit
- Deployment: Docker + AWS EC2, Typer CLI

## Architecture Diagram

![alt text](project_2.png)

## Backend Components

The backend is designed to facilitate API calls for communication between the frontend and the backend. The REST APIs, developed with FastAPI, are restricted with JWT authentication, which generates a token with an expiry of 30 minutes for every new user. The major operations include:

- User retrieval and creation
- Token authentication
- Querying data from the database as demanded
- User activity tracking and API usage limitation
- File source URL generation

## Frontend UI

The UI was developed using the Streamlit framework, a Python library. The application follows the following flow:

- User login for existing users or sign up for new users
- Once logged in, users can access five modules:
  - GOES: Feature-based file extraction for GOES18
  - GOES file link: File name-based URL generation for GOES18
  - Nexrad: Feature-based file extraction for Nexrad
  - Nexrad file link: File name-based URL generation for Nexrad
  - Nexrad plot: Plot of all Nexrad stations in the United States
- User Dashboard: Tracks activity and remaining API hits, displaying:
  - Active plan
  - Used balance with the remaining limit
  - Charts for API usage based on modules and overall at multiple timeframes
- Logout: Allows users to exit the app
- Admin Dashboard: Tracks all activity of the application, including:
  - All users displayed based on their plan
  - Multiple charts showcasing API usage across multiple timeframes

## Deployment

- Both the backend and frontend are individually containerized using Docker. Docker Compose is used to bind the two containers and deploy them on an AWS EC2 instance.
- The application can also be used through the terminal by installing the provided wheel package.
- Usage activity is logged in AWS Cloudwatch, and a module for unit testing has been created using the Python library 'Pytest'. 

## Steps to Run the Application

1. Clone the repository.
2. Create a `.env` file with AWS Bucket and Logging credentials. Follow this format (access token automatically generates with login, but the variable should be there):
   ```
   AWS_LOG_ACCESS_KEY=<enter your Log access Key>
   AWS_LOG_SECRET_KEY=<enter your Log secret Key>

   AWS_ACCESS_KEY1=<enter your AWS access Key>
   AWS_SECRET_KEY1=<enter your AWS secret Key>

   access_token=
   ```
3. Execute the Docker Compose file (`docker-compose.yml`) with the command `docker compose up`.

#### Attestation

WE ATTEST THAT WE HAVEN'T USED ANY OTHER STUDENTS' WORK IN OUR ASSIGNMENT AND ABIDE BY THE POLICIES LISTED IN THE STUDENT HANDBOOK.

Contribution:
- Dhanush Kumar Shankar: 25%
- Nishanth Prasath: 25%
- Shubham Goyal: 25%
- Subhash Chandran Shankarakumar: 25%
