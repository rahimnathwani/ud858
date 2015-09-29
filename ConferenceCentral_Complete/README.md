App Engine application for the Udacity training course.

## Products
- [App Engine][1]

## Language
- [Python][2]

## APIs
- [Google Cloud Endpoints][3]

## Setup Instructions
1. Update the value of `application` in `app.yaml` to the app ID you
   have registered in the App Engine admin console and would like to use to host
   your instance of this sample.
1. Update the values at the top of `settings.py` to
   reflect the respective client IDs you have registered in the
   [Developer Console][4].
1. Update the value of CLIENT_ID in `static/js/app.js` to the Web client ID
1. (Optional) Mark the configuration files as unchanged as follows:
   `$ git update-index --assume-unchanged app.yaml settings.py static/js/app.js`
1. Run the app with the devserver using `dev_appserver.py DIR`, and ensure it's running by visiting your local server's address (by default [localhost:8080][5].)
1. (Optional) Generate your client library(ies) with [the endpoints tool][6].
1. Deploy your application.

## Notes/ Design Choices
### Sessions and Speakers
* Sessions are implemented as models, in `models.py`.  `speaker` is an attribute of the `Session` object, and it's just a string rather than a profile ID, so that we can include speakers who are not users of the site.  We could break it out into a new entity in future if we want to store more details about each speaker.
* `typeOfSession` is also free text.  We could use an enum, but that would entail admin overhead.  We could use session type entity, but that would require us to decide who could create new types, and would further complicate the UI.
* Start time and duration are implemented using `datetime` so that we can add them if we ever need to calculate the end time in future.
### Additional queries
* `getAllConferencesSessionsByType` takes a typeOfSession and returns all sessions across _all_ conferences, that match.  So, the user can choose sessions without knowing which conferences are happening first.
* `getConferencesWithWishlistedSessions` goes through the user's session wishlist, and shows the set of conferences for which the user will need to register.  This is useful for the user who chooses sessions first, then registers for the best conferences.
### Query problem
'Datastore rejects queries using inequality filtering on more than one property.'[according to the documentation][7].  This is due to the way indexes work.  We work around this in `getNonWorkShopsStartingBefore7pm`by querying to get all the results after 7pm, and then loop through them to pick out the ones which are not workshops.
### References
[1]: https://developers.google.com/appengine
[2]: http://python.org
[3]: https://developers.google.com/appengine/docs/python/endpoints/
[4]: https://console.developers.google.com/
[5]: https://localhost:8080/
[6]: https://developers.google.com/appengine/docs/python/endpoints/endpoints_tool
[7]: https://cloud.google.com/appengine/docs/python/ndb/queries#neq_and_in