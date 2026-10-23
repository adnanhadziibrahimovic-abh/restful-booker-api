## Framework Architecture

This framework uses a layered design to keep tests clean and avoid repeating code:

- **Test classes** (`tests/test_*.py`): the test steps and assertions only. No URLs, headers or JSON.
- **Base test** (`tests/base_test.py`): inherited by every test class. Sets up the request utils, authenticator and booking API once per class, provides random test data, and deletes any bookings the tests created.
- **Booking API** (`api/booking_api.py`): the service object. It takes the order from the test, builds the headers and JSON body, and sends it to the right endpoint. Optional fields are passed with `**kwargs`.
- **Authenticator** (`api/authenticator.py`): calls `/auth` once, keeps the token, and adds it as a cookie to calls that need it.
- **Request utils** (`api/request_utils.py`): the foundation. One class for GET, POST, PUT, PATCH and DELETE. All calls go through one handler that logs the request and response, and returns either the raw response or the JSON as a dict.