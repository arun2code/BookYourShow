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

**Booking Service (B2C)
- SearchService  - used to get Cities, Theatres, shows details
    - List<String> getCities();
    - List<Show> getAllShows(@PathVariable String cityName);
    - List<ShowsResponse> getAvailableTheaterByShow(@RequestParam String showId);
    - List<ShowsResponse> getShowsByTheater(@RequestParam String theaterId);   
-  ShowBuilderService - Consumed by SearchService to build a show (theatre+hall+layout+showtime);


## Database Design
Database - MongoDB
Documents - 
- Theatre : {theatreId, halls : [hall1 :{}, hall2:{}]} 
- Movie : {movieId, movieName, description, genere, movieReviews: {}}
- MovieReview : {comments : [], ratings : [], overallRating}
