# Deliveroo Store Density Analysis in Central London

## Project Background

Over the past few years, I've worked with Deliveroo on and off during the spring and summer months. Last year, I exclusively worked in Chelsea and Westminster. I noticed that while the Deliveroo rider app provides real-time hotspot zones, I wanted to get a overview about store density and operating hours. This project aims to enhance rider efficiency by analysing store locations and operating patterns in Westminster and Chelsea.

### Key Analysis Areas:

- **Store Density Mapping**: Identifying areas with the highest concentration of potential pick up locations
- **Operating Hours Analysis**: Understanding store opening/closing patterns throughout the day
- **Geographic Coverage**: Optimising decisioning making with an e-bike battery range of 30-40 miles

![Store Density Animation](https://github.com/user-attachments/assets/71e5fe42-7bb7-422d-adee-b2e7f218348c)

## Data Structure

The project collects and processes the following data using Google Places API:

### Tables Generated:

#### Raw Establishments Data
- Name, Latitude, Longitude, Store Type, Borough
- Opening Hours, Closing Hours, Day
- Each Store had its opening and closing hours for each day of the week at an daily level

![Raw Data Structure](https://github.com/user-attachments/assets/f5415efd-05db-4b8d-b7f7-0a00f29df7f7)

#### Tableau Animation Data
- Processed data with hourly intervals
- Additional fields for visualisation
- Each Store had its opening and closing hours for each day of the week at an hourly level of detail

![Animation Data Structure](https://github.com/user-attachments/assets/81d07155-1e86-445e-a651-655099a3213f)

#### Validation Data
Quality control and data integrity checks:
- Hours Per Day: Breakdown of operating hours for each day (Should be 24 hours per day for complete coverage)
- Total Hours: Total count of operating hours (Should be 168 for complete coverage: 24 hours × 7 days)
- Complete Coverage: Boolean flag indicating if establishment has data for all hours (True if Total Hours = 168)
- Has Overnight Operations: Identifies establishments operating past midnight
- 1 row provides details about a store and their data quality

![Validation Data Structure](https://github.com/user-attachments/assets/29ada4e2-2803-4422-bd7a-dad2681cbba2)

## Technical Implementation

### Prerequisites:

1. Google Maps API key
2. Python environment with required packages:
   - python-dotenv
   - googlemaps
   - pandas
   - logging
   - argparse

### Key Components:

1. **PlacesDataCollector**: Handles API interactions and data collection
2. **PostcodeProcessor**: Manages postcode batching and borough mapping
3. **TableauDataPreparator**: Prepares data for visualisation

### Features:

- Batch processing to respect API limits
- Deduplication of establishments, due to the how close postcodes are to each other the search radius picked up the same establishments twice when searching in postcodes adjacent to each other
- Borough mapping for accurate location assignment
- Time-based analysis for operating hours

## Limitations & Challenges

### 1. API Constraints
- Rate limiting necessitates batch processing
- Expensive! Fortunately I used free credits with a Google cloud trail otherwise this project would have cost £199

### 2. Data Coverage
- I selected specific Google Places API tags, which impacted which establishments were included in the final dataset
- Notable limitations include missing some popular Deliveroo partners (e.g., Zapp, which I frequently received orders from) as they may be classified differently in the Places API
  - restaurant
  - meal_takeaway
  - fast_food_restaurant
  - food
  - meal_delivery
  - cafe
  - grocery_or_supermarket
  - supermarket
  - convenience_store

### 3. Data Quality
- Data does not include direct mapping to Deliveroo partnership status
- Possible missing establishments due to missing tagging

### 4. Processing Limitations
- Time-intensive due to API rate limiting

## Insights & Visualizations

The analysed data is visualised in Tableau, offering:

- Store density Hex maps
- Time-based animations of store operations (Opening and Closing Times)
- [Link to Dashboard](https://public.tableau.com/app/profile/valerie.madojemu/viz/DeliverooHotspots/DeliverooHotspot)

![Store Density Visualization](https://github.com/user-attachments/assets/71e5fe42-7bb7-422d-adee-b2e7f218348c)

## Takeaways

### Strategic Positioning - High-Value Zones
Based on my personal experiance and findings from the dashboard:
- **South Kensington**: Provided a consistent order flow, anchored by reliable partners such as Wingstop/Nandos which is highlighted on the density map with a higher number of stores in this location
- **Notting Hill**: High store density supporting continuous operations
- **Chelsea**: Strong base for bidirectional order flows between Notting Hill/Paddington/Victoria - I was constanlty going between these areas when delivering around London.

### Strategic Shift Pattern
- I primarily work morning shifts on weekends 07:00-till my battery dies during/after the lunch rush.
- Starting at 7am the Riders app regularly signalled that its not busy, but I got consistent orders. Visualising stores operating hours I saw there were a lot more stores open than I thought!
- From personal experience there is not a lot of traffic and riders on the road at that time, which has the possibility to lead to a busy morning. However this always isn't the case, of course there are other factors that affect the demand.

## Future Improvements

### 1. Data Enrichment
- Find a way to tag stores which do partner with Deliveroo
- Research more optimal tags to bring back all stores of interest

### 2. Technical Enhancements
- Automated data refresh pipeline
- Fix bug where Borough cannot be mapped to postcode (non-essential as its not used in visualisation)
- Add in more parameters for more efficient testing

## License

This project is licensed under the MIT License - see the LICENSE file for details.
