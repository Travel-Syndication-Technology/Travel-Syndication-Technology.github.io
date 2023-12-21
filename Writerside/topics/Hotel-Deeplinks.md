# Hotel Deep Links

Preferred deep linking API, can be used to link into a specific search result set or hotel detail page.

URL base: `/hotel/search`

#### Parameters
- `zipCode` - optional. Can be used to bypass the zipcode challenge modal.
- `location` - required. Some form of prefix+ID to identify a search location, or an exact name search.
- `checkIn` - required only if searching for airport/city. Formatted MM/DD/YYYY.
- `checkOut` - required only if searching for airport/city. Formatted MM/DD/YYYY.
- `adults` - required only if search for airport/city. Number of adults in room.
- `children` - optional. Comma-separated integers representing child ages.
- `chainFamily` - optional.  Chain family code to filter.  Below table shows the list of valid chain family codes
- `aaamem` - optional. value is 1 character  (Y indicates the request is being made on behalf of a member. Exposes CUG rates)

##### chainFamily

| chainFamily | Family Description           | Rewards Program Name     |
|-------------|------------------------------|--------------------------|
| RTR         | Accor Hotels                 | Le Club Accorhotels      |
| BWR         | Best Western International   | Best Western Rewards     |
| CW          | Carlson Rezidor Hotels       | Club Carlson             |
| EC          | Choice Hotels International  | Choice Privileges        |
| EW          | Exclusive World Hotels       | Peakpoints               |
| EH          | Hilton Worldwide             | Hilton HHonors           |
| HYR         | Hyatt Hotels                 | Hyatt Gold Passport      |
| 6C          | InterContinental Hotels      | IHG Rewards Club         |
| LOR         | Langham Hotels International | Langham Supper Club      |
| EM          | Marriott International       | Marriott Rewards         |
| NSR         | NH Hoteles                   | NH Hotel Group Rewards   |
| PV          | Preferred Hotel Group        | iPrefer                  |
| SW          | Starwood Hotels              | Starwood Preferred Guest |
| TR          | Wyndham Hotel Group          | Wyndham Rewards          |
| RLR         | Red Lion Hotels              | Hello Rewards            |
| MVR         | MGM Resorts International    | MGM Resorts - M life     |


##### location 
Parameter Prefixes

- a: | Airport code follows
- c: | City id follows
- h: | TST_Hotel_Master id follows
- tst- | TST_Hotel_Master id follows
- tp- | Travelport Apollo id follows
- ati- | ATI id follows
- gta- | GTA city and property code follow
- tco- | Tourico code follows

You can also search by exact name as seen in autocomplete (and URL encoded if you want to be safe): location=Tolarno%20Hotel,%20Saint%20Kilda,%20Australia

Examples:

Search Results Page
- Search near Hartsfield Jackson airport checking in 02/01 and out 02/03 for two adults:
`/hotel/search?location=a:ATL&checkIn=02/01/2023&checkOut=02/03/2023&adults=2`
- Search near Hartsfield Jackson airport checking in 02/01 and out 02/03 for two adults and filter for only the Hilton Worldwide chain hotels:
`/hotel/search?location=a:ATL&checkIn=02/01/2023&checkOut=02/03/2023&adults=2&chainFamily=EH`
- Search near Atlanta city center for above plus two children aged 2 and 3:
`/hotel/search?location=c:776&checkIn=02/01/2023&checkOut=02/03/2023&adults=2&children=2,3`
- Search using a full city name (use the city name exactly as it appears in the TST autocomplete function). For example a search for Seaside, OR for specific dates and travelers:
`/hotel/search?location=Seaside,%20Oregon,%20United%20States&adults=2&children=4,6&checkIn=06/15/2023&checkOut=06/17/2023`

Note Regarding Locations

One cannot take the location from the hotel product landing page location dropdown and paste it in a deeplink URL. Instead, go to https://amatravel.tstllc.net/vacation and select the Hotel+Car tab. Pick your city from that location drop down and put it in the deeplink URL.



####  Date less City Search
A city search can be performed without a date range by simply using a location parameter described above. The search will be performed for a date 5 days out for 1 traveler for 1 night

Search using a full city name (use the city name exactly as it appears in the TST autocomplete function). For example a search for Seaside, OR :

`/hotel/search?location=Seaside,%20Oregon,%20United%20States`

Search near Hartsfield Jackson airport :

`/hotel/search?location=a:ATL`


#### Hotel multi-room search
TST now supports the ability to perform multi-room search. Due to recent supplier changes, for multi room requests we always perform double occupancy searches. The sample URL format is listed below for our reference

`/hotel/search?location=ATL&checkIn=04/24/2023&checkOut=04/26/2023&rooms=[{"numOfAdults":2,"childAgeList":[]},{"numOfAdults":2,"childAgeList":[]}]`

TST recommends modifying the custom widget so it switches default multi-room searches to double occupancy with an accompanying message as shown below





Variable: rooms
Description: room roster specifying the number of adults as 2 and an empty list for children with ages
<note>
All destinations or properties need to be URL Encoded before being passed to the APIs. URL Encoding can be easily achieved using this online tool:

[http://www.url-encode-decode.com/](http://www.url-encode-decode.com/)

For example to perform a search for Coeur D'Alene, Idaho, United States, the URL Encoded form is Coeur+D%27Alene%2C+Idaho%2C+United+States. The full URL would be of the form

`https://travel.calif.aaa.com/hotel/search?location=Coeur+D%27Alene%2C+Idaho%2C+United+States&adults=2&children=4,6&checkIn=06/15/2022&checkOut=06/17/2022`
</note>

### Hotel Details Page

Search for hotel with id 74796:
- `/hotel/search?location=h:74796`
- `/hotel/search?location=tst-74796`
- `/hotel/search?location=Masseria%20Relais%20Del%20Cardinale,%20Madonna%20Pozzo%20Guacito,%20Italy`

Search for hotel with id 74796 and actually perform availability check:
- `/hotel/search?location=tst-74796&checkIn=02/01/2023&checkOut=02/03/2023&adults=2`

Search for hotel by TP Apollo code:
- `/hotel/search?location=tp-01747`

Search for hotel by ATI id
- `/hotel/search?location=ati-ATLBKX`

Search for hotel by GTA city and property code
- `/hotel/search?location=gta-ATL-SHE5`

Search for hotel by Tourico code
- `/hotel/search?location=tco-10393`

## Hotel TripTik Search
This was built out for integration with TripTik (and I think is our main entry point for hotel mobile). It only links to a particular hotel details page.

URL Base: /hotel/TripTikSearch

Parameters:

zip - optional. Can be used to bypass the zipcode challenge modal.
devicecd - optional. This is a string. If the string is "TB", "FP" or "SP", we'll forward you to the mobile site. Otherwise we'll deeplink you to the desktop site.
propID - required. The TDR id of the hotel you want to link to.
checkinMonth - required. The month of the check-in. We do our best to parse this value. You can enter a number, a 3 letter month abbreviation, or the full month name.
checkinDay - required. The day of check-in.
checkoutMonth - required. The month of the check-out. Same as check-in.
checkoutDay - required. The day of check-out.
adults - required. The number of adults.
children - optional. The number of children (NOTE: we don't currently support doing a search based on number of children alone. We need ages, so children > 0 will always fail.)
aaamem - optional. value is 1 character  (Y indicates the request is being made on behalf of a member. Exposes CUG rates)
Examples:

Search for hotel with TDR id 107998 for 2 adults, checking in on March 1st, and checking out on March 3rd (NOTE: we always assume this date is in the future, so we adjust the year accordingly.)
/hotel/TripTikSearch?devicecd=PC&propID=107998&checkinMonth=03&checkinDay=01&checkoutMonth=March&checkoutDay=03&adults=2
Mobile search for hotel with TDR id 107998 for 1 adult, checking in on April 1st and checking out on April 4th.
/hotel/TripTikSearch?devicecd=SP&propID=107998&checkinMonth=04&checkinDay=01&checkoutMonth=04&checkoutDay=04&adults=1
Mobile Integration update
The TripTik URL info /hotel/TripTikSearch is the same, with the addition of aaaMobileWrapped=true and refclickid=TTPMobileApp for incoming links from the mobile app.

On each page in the flow, there will be a TST object attached to window. It has the basic form of:

TST.aaaMobileData = {
page: "page-name",
error: boolean
}
page will always be present. error is optional.

Possible values for page	Notes
hotel-rates	This should be the first page you land on after a successful TripTik formatted link into our site
traveler	First step in the booking flow
review	Review the booking info before putting your money down. You can also end up here after a bad booking attempt. error will be true in that case and the back button should bail out to the map
checkout	The payment info page
booking	The booking confirmation and end of our booking flow…back to your app!
hotel-landing	It is possible (but unlikely) you could get here. If you do, I would recommend throwing immediately back out to the map view instead of waiting on a back button push
hotel-info	Also possible but unlikely. This will come up (instead of hotel-rates) if we don’t have bookable rooms that match the search. Back to the map from here
hotel-availability	I don’t think you can get here, but if you do, don’t wait for the back button. It’s essentially our version of what the mobile app exposes at the map view…a list of possible hotels
