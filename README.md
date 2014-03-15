# Rail Canada stations data
[VIA Rail](http://viarail.ca) uses its own 4-letter codes for stations, but isn't publishing their data (yet). This aims to provide the missing data for VIA's stations.

## Data files

### Basic: `stations_via.json`

A list of VIA's own station data, obtained letter by letter using the `http://reservia.viarail.ca/GetStations.aspx?q=<letter>` endpoint (used for their reservation portal live search). Each entry looks like:

    {
        "sc": "ABBO",
        "pv": "BC",
        "sn": "ABBOTSFORD",
        "dEn": "ABBOTSFORD"
    } 

    
Where:

* `sc`: Stands for "station code"
* `pv`: Stands for "province"
* `sn`: Stands for "station name"
* `dEn`: Not sure—however it is the same as `sn` for all entries

### Full: `stations_via_full.json`

Same as  `stations_via.json` plus some extra data fetched for each station by parsing `http://www.viarail.ca/en/embedded/station/detail/<station code>`, when available. Each entry looks like:


    {
        "pv": "BC", 
        "name": "Abbotsford train station", 
        "url": "http://www.viarail.ca/en/embedded/station/detail/ABBO", 
        "long": "-122.279", 
        "lat": "49.1183", 
        "sn": "ABBOTSFORD", 
        "dEn": "ABBOTSFORD", 
        "address": "Hargitt Street, Abbotsford, BC, V3G 1M8, Canada", 
        "sc": "ABBO"
    }
   
Where:

* `name`: The long name of the station.
* `lat`: The latitude of the station, in decimal degrees. 
* `long`: The Longitude of the station, in decimal degrees. 
* `address`: The full address of the station.
* `url`:  The URL to get the full station information.

Notes:

* Any of the above can be an empty string when the information was not available.
* Some of these stations are really obscure and are not passenger stations. Little information is available for these.


## Re-generating

    pip install -r requirements

Then:    

    >>> from via_rail import VIA
    >>> VIA().save_stations(filename='stations_via.json')

Or:

    >>> VIA().save_stations(filename='stations_via_full.json', full=True)
