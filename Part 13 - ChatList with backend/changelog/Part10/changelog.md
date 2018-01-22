# Part 10
## Changelog
1. In earlier parts, we had left out a minor thing that resulted in the following - when you sign up, then log out. What is seen is that the highlighted tab is the log-in tab but the switchStatus (in LoginForm.js) remains at sign-up. As such, when we perform a successful sign-up, we need to reset the switchStatus (not required for a successful log-in because that means our switchStatus is already at log-in). So go ahead and update the states in the auth reducer.
2. Similar to point 1, when we are logged in, the profile states are set, but when we log out, we need to reset them so when a new user signs up, the edit profile page is clean again. (Not so much for if the user logs in as logging in will update the profile page.) This is done by adding a ResetProfileState() action when signing out (before or after would not matter).
3. There are some firebase issues we need to resolve. Previously, we retrived data (in Main.js componentDidMount) by using firebase.database().ref().on(). There is an actual difference between using on() and once() method. on() will set up an eventListener that continues to listen until it is turned off with the .off() method. Consequently, when a user logs out as of now, the listener continues to listen but a logged out status will result in an error in having read access (according to our firebase rules). As such, we can turn the listener off just before signing out. However, the user profile is something we just need to read updates once each time the Main component gets mounted (either after logging in or having edited profile). So we can just use the once() method instead and not have to switch off the listener. 
4.  Next up, we are soon going to be reading all other user info (which could become thousands or millions i.e. too large a snapshot to handle smoothly). Our current database structure will only allow us to take a snapshot of the users folder rather than in parts, which may be very huge in size. As such, we will swap to a new database structure - when creating a profile in the database, we push the data into a userdata/${uid}/ folder, as well as push a bool obj (a sufficiently small-sized object) placeholder (the value does not matter, it is the key that matters) under a userkey/#{uid}/ folder. So now, when retrieving data, the app can easily retrieve millions of keys at once from the userkey, under a single lightweight snapshot. From there, our plan will be to read profiles using the userkeys in certain intervals e.g. 10 profiles at a time. THEREFORE, go ahead and update the firebase paths. Also updated firebase storage path from /users/ to something more useful like /dp/ 
5. We now create a custom component called Deck and import it into Main.js under the Cards option. Over in Deck.js, we first set up a DeckSwiper components and functional buttons that controls the swiping of DeckSwiper. We add a placeholder cards dataSource as well. So right now, rendering our App should allow us to swipe through all the cards functionally.
6. The last thing we would like to do for this part would be to retrieve the userkeys from the database, when the Deck component is mounted (not when Main is mounted). This time, we use .once() method again, as we can just read all the keys in one shot rather than having to listen out for changes (new profiles do not need to be listened continuously in the context of our app and will only take up more RAM). 
7. Updated SetData to also update the validity states.

## Preview
![Preview Gif](./part10.gif)