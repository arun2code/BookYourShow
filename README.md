# BookYourShow
Ticket booking application Design

# Ticket Booking Flow
Search -> get cities on page load -> select a particular list -> Load all theatres and available movies.
    -> select a movie -> search theatre by selected movie
    -> select a theatre -> show all movies for selected theatre for next 3 days.
-> user select the date and time slot of a show -> select the seat -> call blockSeat() and return blockId
-> click on payment -> call promotional() API, and display any offer/discount of the show
-> integrate with payment API, and call BookingService to confirm the booking. -> return bookingId
-> send the notification to user and order details through Kafka


## Microservices / Core APIs
**Onboarding of Theatre and Movies/Shows
- TheatreOnboardingService - Used by third party vendor along with Admin to onboard himself as well as push their shows schedule.
- ShowCatalogService - Used by admin to update events/movide details into the system.
- SupportService - Used by admin to monitor and manage admin activities.

Databases
    - MongoDB- to store theatre details, hall layout, movies/events details, shows reviews and comments by users.
    - Oracle - to store, user account details, user booking history, list of cities, movies, theatres.
    - RedisDB - to cache the data of cities (on page load), fetched theatres, fetched movies, fetched halls details for selected theatre.
    
**Booking Service (B2C)
- SearchService  - used to get Cities, Theatres, shows details
    - List<String> getCities();
    - List<Show> getAllShows(@PathVariable String cityName);
    - List<ShowsResponse> getAvailableTheaterByShow(@RequestParam String showId);
    - List<ShowsResponse> getShowsByTheater(@RequestParam String theaterId);
    - Search parameters(keyword, city, lat_log, radius, start_datetime, end_datetime, postal_code, pagination)

-  ShowBuilderService - Consumed by SearchService to build a show (theatre+hall+layout+showtime) and create a show. Show object will be persisted in Oracle DB with required information. User will linked to booked shows.



## Database Design
Database - MongoDB
Documents - 
- Theatre : {theatreId, halls : [hall1 :{}, hall2:{}]} 
- Movie : {movieId, movieName, description, genere, movieReviews: {}}
- MovieReview : {comments : [], ratings : [], overallRating}

RadisDB
- on page load, load and save the cities, 
- keep fetched theatres, shows

