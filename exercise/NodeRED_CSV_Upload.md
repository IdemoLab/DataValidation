# PostgreSQL Table Creation and Data Insertion

This document outlines the SQL commands for creating a PostgreSQL table that matches the structure of a provided CSV file, along with a Node.js script for inserting data from the CSV into the PostgreSQL database.

## Download Dataset from Kaggle

You can download the dataset used for this setup from Kaggle. The dataset is titled "Smart Home Dataset with Weather Information" and can be found at the following URL:

[Kaggle Dataset Link](https://www.kaggle.com/datasets/taranvee/smart-home-dataset-with-weather-information)

Please note that you might need to create an account on Kaggle to access the dataset. After logging in, you can download the dataset directly from the dataset page.

## Node-RED Flow Integration

To facilitate the data insertion from the CSV into your PostgreSQL database, a Node-RED flow is required. You can find and import a suitable Node-RED flow from the following link:

[Node-RED Flow for PostgreSQL Data Insertion](https://flows.nodered.org/flow/687918dd5cb66a3bfc2a661e15ef4237)

This flow includes the necessary nodes to process the CSV data, format it according to the database schema, and execute the SQL insert operations. Make sure to review and adjust the flow parameters to match your specific setup, such as database credentials and node configurations.

## SQL Table Creation

The SQL commands below create a new table called `home_data` that includes various measurements related to energy usage and environmental conditions:

```sql
DROP TABLE IF EXISTS home_data;
CREATE TABLE home_data (
    time TIMESTAMP NOT NULL,
    use_kw FLOAT,
    gen_kw FLOAT,
    house_overall_kw FLOAT,
    dishwasher_kw FLOAT,
    furnace1_kw FLOAT,
    furnace2_kw FLOAT,
    home_office_kw FLOAT,
    fridge_kw FLOAT,
    wine_cellar_kw FLOAT,
    garage_door_kw FLOAT,
    kitchen12_kw FLOAT,
    kitchen14_kw FLOAT,
    kitchen38_kw FLOAT,
    barn_kw FLOAT,
    well_kw FLOAT,
    microwave_kw FLOAT,
    living_room_kw FLOAT,
    solar_kw FLOAT,
    temperature FLOAT,
    icon TEXT,
    humidity FLOAT,
    visibility FLOAT,
    summary TEXT,
    apparent_temperature FLOAT,
    pressure FLOAT,
    wind_speed FLOAT,
    cloud_cover TEXT,
    wind_bearing FLOAT,
    precip_intensity FLOAT,
    dew_point FLOAT,
    precip_probability FLOAT
);

SELECT create_hypertable('home_data', 'time');
```

## Data Insertion Script

Below is a Node.js script snippet to format and insert CSV data into the `home_data` table. This script assumes the message payload contains CSV data formatted as JSON:

```javascript
if (msg.payload && msg.payload.length > 0) {
    let payload = 'INSERT INTO home_data(time, use_kw, gen_kw, house_overall_kw, dishwasher_kw, furnace1_kw, furnace2_kw, home_office_kw, fridge_kw, wine_cellar_kw, garage_door_kw, kitchen12_kw, kitchen14_kw, kitchen38_kw, barn_kw, well_kw, microwave_kw, living_room_kw, solar_kw, temperature, icon, humidity, visibility, summary, apparent_temperature, pressure, wind_speed, cloud_cover, wind_bearing, precip_intensity, dew_point, precip_probability) VALUES ';

    for (const line of msg.payload) {
        payload += `('${new Date(line.time * 1000).toISOString()}', ${line['use [kW]']}, ${line['gen [kW]']}, ${line['House overall [kW]']}, ${line['Dishwasher [kW]']}, ${line['Furnace 1 [kW]']}, ${line['Furnace 2 [kW]']}, ${line['Home office [kW]']}, ${line['Fridge [kW]']}, ${line['Wine cellar [kW]']}, ${line['Garage door [kW]']}, ${line['Kitchen 12 [kW]']}, ${line['Kitchen 14 [kW]']}, ${line['Kitchen 38 [kW]']}, ${line['Barn [kW]']}, ${line['Well [kW]']}, ${line['Microwave [kW]']}, ${line['Living room [kW]']}, ${line['Solar [kW]']}, ${line['temperature']}, '${line['icon']}', ${line['humidity']}, ${line['visibility']}, '${line['summary']}', ${line['apparentTemperature']}, ${line['pressure']}, ${line['windSpeed']}, '${line['cloudCover']}', ${line['windBearing']}, ${line['precipIntensity']}, ${line['dewPoint']}, ${line['precipProbability']}),`;
    }

    msg.payload = payload.slice(0, -1) + ';';  // Remove the last comma and end the statement with a semicolon
}
return msg;
```
