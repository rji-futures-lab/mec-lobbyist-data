# MEC Lobbyist Data

This repo contains data and code for exploring lobbyist disclosure data in Missouri state government.

## About the data

We acquired the data from the Missouri Ethics Commission (MEC).

### Lobbyist Principle Report Data

We downloaded two Excel files from [this page](https://mec.mo.gov/MEC/Lobbying/LobPrinReportData.aspx).

- [LobPrinReportData.xls](data/orig/LobPrinReportData.xls) with "Lobbyist/Principle Report" selected
- [PrinLobReportData.xls](data/orig/PrinLobReportData.xls) with "Principle/Lobbyist Report" selected
- We saved these original files in the [`data/orig/`](data/orig/) directory of this repository.
- We opened each file in Microsoft Excel, exported them in .csv format and saved them in [`data/prep/`](data/prep/)

## About the app

This project is built with [Datasette](https://datasette.io/), a tool for exploring and publishing data.

Datasette is built on top of [SQLite](https://www.sqlite.org/index.html), so we needed to load the separate csv files into a SQLite database. 

The [`csvs-to-sqlite`](https://datasette.io/tools/csvs-to-sqlite) command-line tool, which is part of the Datasette ecosystem, made this task rather easy.

```sh
csvs-to-sqlite data/prep/*.csv data/mec-lobbyists.db
```

## About the deployment

We [deployed](https://mec-data-3xsfsqto6a-uc.a.run.app/) our instance of Datasette on [Google Cloud Run](https://cloud.google.com/run).

We're used the <mujschool2020@gmail.com> Google account, which already had Google Cloud Services enabled for a previous RJI-related project. 

To set up a new project, we had to log into the [Google Cloud Console](https://console.cloud.google.com), create the project (named "mec-data") and enable a couple of APIs for the project.  (Not clear if these steps need to be repeated for each deployed instance of Datasette).

Then we needed to install [gcloud command-line interface](https://cloud.google.com/sdk/gcloud), initialize it and configure our credentials and project settings. These steps need to be repeated for each contributor's machine.

Then anyone can deploy or update the deployment.

```sh
datasette publish cloudrun data/mec-lobbyists.db
```
