# Project Three - Icelander

# Project Brief

Working in a group of four (with [Karen Kaur](https://github.com/kkaur89), [Hamza Butt](https://github.com/HamzaaMB) and [Jahnvi Patel](https://github.com/jahnviipatell)), make a full stack app (MongoDB / Express / React / Node) with CRUD functionality in seven days.

# Link to Deployed Project

[icelander.netlify.app](https://icelander.netlify.app/)

# Overview and Concept of the Project

When discussing our ideas for the project, we discovered that two members of the team had been to Iceland several times on holiday, and had been on many tours and road trips exploring the country. They both believed strongly that Iceland is the most wonderful and photogenic travel destination in the world, and after seeing a few photos, the other two of us were in firm agreement! So we decided to make an app that would help users to plan road trips around Iceland, offering pre-made packages, functionality for saving destinations (and thus constructing custom packages), information on beauty spots and amenities, and interactive maps. Most importantly, incredible photos of Iceland would take centre stage - we knew that anything CSS can do to make a website attractive can be done better by Iceland’s breathtaking natural beauty.

![screenshot](https://github.com/PaddyCello/SEI-Project-Three/blob/1bef01aba4b15bc124bc0d3e54a92ef3330d7cb8/screenshots/Screenshot%202021-05-02%20at%2008.22.29.png)

![screenshot](https://github.com/PaddyCello/SEI-Project-Three/blob/6e74151feddb85bde414c5519d2fd0b063c0963e/screenshots/Screenshot%202021-05-02%20at%2008.23.19.png)

![screenshot](https://github.com/PaddyCello/SEI-Project-Three/blob/108665fcd3bc739216be483e7b3339c32980e857/screenshots/Screenshot%202021-05-02%20at%2008.24.04.png)

# Technologies Used

- React Hooks
- Express.js
- Node.js
- MongoDB
- Mongoose
- Axios
- JavaScript
- CSS
- React-Bootstrap
- Mapbox
- GitHub
- Netlify
- Heroku

# Approach Taken

We began by writing some detailed pseudocode, outlining our ideas and strategies for both MVP and post-MVP features. We used a Google Doc for this, for both visibility and shared editing power; this was also where we drafted and edited our models for our locations, packages and users. We also used Excalidraw to design wireframes for page layouts and user flow. Additionally, I started a Trello board to assign tasks and keep these visible within the group, which I regularly updated as tasks were completed. Trello was invaluable, as we wanted to avoid situations where team members would unknowingly overlap on tasks, knowing that this could lead to merge conflicts and a lot of wasted time. 

Merge conflicts were a particular fear for us, as this was our first experience of group collaboration on GitHub - we had all worked in pairs on the previous project, but knew that more people would involve more complication. Apart from maintaining visibility with tasks and avoiding overlaps, our other approach for minimising the risks involved us all working on separate feature branches, and only merging back into the development branch as a group, one person at a time. This meant that we could address merge conflicts before they became too multi-faceted, and that everyone was present to review any conflicts that needed fixing.

With this in mind, we began building our app as a team, making sure that we had the majority of the components on the front and back end built in skeleton form before we started dividing up tasks. We also built out the basics of our back end server as a team, including some very basic bits of data in our Places database, and only began to divide up back end work after we could successfully retrieve data in Insomnia.

One of the other main tasks that we divided up was the collection of data for our database. Even though we tested out most of our routes with the bare minimum of data, knowing that we might discover some necessary changes to our models as we built out our app, we were aware that we were offering features - such as custom packages and a map that would display the places from our database - that would necessitate a substantial data set. Accordingly, we each researched locations most evenings, and put our findings into a Google Sheet, formatted into json so that it could be copied and pasted into our seeds file at a later point. Again, we did things this way to avoid merge conflicts, and also for visibility; we also allocated different types of location to different team members to avoid repetition. I was allocated restaurants, and found TripAdvisor to be a reassuringly prolific source of information on places to eat throughout Iceland. As our model required coordinates for map rendering, I also entered the majority of the restaurant addresses into latlong.net to find these.

This project was also the first time that any of us were dependent on using documentation to understand new features that we were looking to integrate, save for some fairly lightweight docs from third-party APIs used in our previous projects. The two main documentation sources that we had to digest were for React-Bootstrap and Mapbox. React-Bootstrap was a lesser concern for me as my role within the team was directed more towards functionality than styling, although I was the one who initially directed our team towards this particular CSS framework. However, Mapbox was absolutely one of my responsibilities, and the research required proved to be far more extensive than I initially realized. This was due to many factors, some of which include:

- Vast amounts of legacy code in the snippets, tutorials and examples; much of this is in ES5, so not easy to extrapolate into something that could be used with React Hooks.
- Counterintuitive qualities to the layout and content of the documentation, to the extent that I was unable to locate installation instructions; I had to use a combination of Google and educated guessing in order to get our Mapbox package installed.
- There are many similarly named yet apparently incompatible packages under the Mapbox umbrella. The one we were using was ReactMapGL, but that didn’t prevent me from erroneously installing ReactMapboxGL, MapGL or MapboxGL at various points.

```
"react-map-gl":"^6.1.10",
"react-mapbox-gl": "^5.1.1",
```

I have since learned that none of this is particularly unusual where documentation is concerned, but at the time it was quite a shock. Fortunately, this did mean that my skills and specificity with Google and Stackoverflow improved sharply as a result, as maps were a central feature of our MVP and I needed to get them up and running.

Maps, markers, zoom controls and descriptive popups were all built and integrated successfully; however, I did hit a snag with one of our first post-MVP features. We wanted our map to display routes for road trips, with the route line snapped to actual roads, and waypoints displayed. Copious amounts of researching eventually led to the plugin that would make this possible: react-map-gl-directions.

It was deprecated, and therefore wouldn’t install, let alone run.

I emailed the developer who was managing the repository for the plugin to see if anything could be done about this, but I knew we would also need a backup plan. After some discussion, having an animated recentering and zooming in with the map whenever a map icon was clicked seemed like a cool and still relevant feature to include instead. ReactMapGL offers another feature, FlyTo, that takes care of this; we decided to implement it on just the pages that displayed full details of whichever package had been selected, as we already had onClick listeners handling the popups on the icons in our main map page. One of my teammates got this up and running, as by this point I was working navigating another unexpected obstacle.

As I mentioned earlier, when assigning solo tasks to each team member, a point was made of not having people working on the same components as each other. This did prevent many merge conflicts (but not all - we were all using the same CSS file after all!), and ensured that large sections of the app could be built over the weekend. However, we had not considered the possibility that separating out React components could have an effect on props chains - if you are developing a component in isolation, you may not have a full picture of how and from where you are receiving props, or how and to where you are passing them. A situation like this had emerged with a handful of components that used information from our Places database in different ways, and were largely accessed by the user in a certain order - and at the end of the user journey, unwanted duplicate data was being displayed. I volunteered to figure out where and how things were getting lost in translation between the various components.

I quickly deduced that the data was being duplicated because the .map() method had been used one too many times on our data set, and probably that the two .map() methods were being used in different components. This turned out to be the case, and the extra .map() was removed. However, I subsequently discovered that the data now being returned wasn’t filtering properly, and I was seeing the same set of places regardless of which holiday package had been selected. Several differing attempts at implementing .map() and .filter() in different places failed, and I eventually concluded that it would not be possible to make this work with props.

As it turned out, the solution was to use params to get the ID of the selected package, make a new GET request for the full set of places, and then filter the results:

```
const { id } = useParams()

const [locations, setLocations] = useState([])

  useEffect(() => {
    const getData = async () => {
      const { data } = await axios.get('/api/places')
      const packageData = data.filter(item => {
        return item.packages.includes(parseInt(id))
      })
      setLocations(packageData)
    }
    getData()
  }, [])
```

With this now working, I moved on to building out the functionality for the user profile. Login, authentication, and redirection to the profile upon success were fairly straightforward, boilerplate matters; but the main purpose of the user profile existing was so that the user could save places to their profile, with a view to building road trips of their own. Adding a ‘save’ button to the popups on the main map page was obviously straightforward; but creating the handler for this was more of a challenge, as at the time I couldn’t figure out how to do this with a PUT request - my attempts resulted in errors concerning incomplete data being sent in the request payload. From a UX perspective, saving places had to be simple, so there was no value in seeing if I could make it work with an input form or anything similar.

After much research, I discovered that a PATCH request was likely to do the job. This had always been presented to me as being interchangeable with a PUT request, but their use cases are actually quite different. As PATCH requests do not require the full data set for an item in order to make any changes, this was a much more suitable route. Additionally, as I was only using the PATCH request on one field in the User model (savedPlaces), the controller on the back end would not be difficult to write. Further research revealed that Mongoose even has a built-in method, .addToSet(), that would push the ID of the place to be saved into the user’s savedPlaces array - but only if that ID was not already present in the array. As I was already concerned about how to deal with duplicate entries, this was a wonderful discovery.

When I had put all of this together, it worked perfectly. Here’s how it looked:

```
const handleClick = async () => {
    setSaved('Saved! View your profile to see your saved places.')
    setButton(false)
    const token = window.localStorage.getItem('token')
    await axios.patch(`/api/places/${_id}`, { _id }, {
      headers: {
        Authorization: `Bearer ${token}`
      }
    })
```
```
export const addSavedPlace = async (req, res) => {
  console.log('REQ BODY', req.body)
  try {
    const user = await User.findById(req.currentUser._id)
    if (!user) throw new Error('Cannot find user')
    const newSavedPlace = await Place.findById(req.body._id)
    console.log('PLACE ID>>>', newSavedPlace)
    user.savedPlaces.addToSet(newSavedPlace)
    await user.save()
    return res.status(200).json(user)
  } catch (err) {
    console.log(err)
    return res.status(404).json({ message: err.message })
  }
}
```

After this, getting the items to display on the user profile went fairly smoothly. I made a new component, GetMyPlaces, which took props from UserProfile. A new GET request was made, returning all of the places; these places were then run through a .filter(), checking to see if they existed in the savedPlaces array of the user:

```
const GetMyPlaces = (props) => {
  const [allMyPlaces, setAllMyPlaces] = useState(null)

  useEffect(() => {
    const getPlaces = async () => {
      const { data } = await axios.get('/api/profile/getmyplaces')
      const myPlaces = data.filter(item => {
        return props.savedPlaces.includes(item._id)
      })
      setAllMyPlaces(myPlaces)
    }
    getPlaces()
  }, [])
  ```
  
  By this point, we were getting quite close to our deadline; however, there was still enough time for me to squeeze in one more feature first. We wanted to add functionality for users to add ratings to places, and for the average rating to be displayed. We had already added the necessary ratings schema and virtual fields to our back end model; now we just needed the corresponding code on the front end.

Finding an on-brand and workable star rating plugin actually took several goes; as it turned out, I ended up using the example on [https://www.30secondsofcode.org/react/s/star-rating](https://www.30secondsofcode.org/react/s/star-rating) to build my own. This was not without its complications, however. The StarRating component takes one destructured prop ({ value }) by design; but I needed to pass in the ID of the selected Place as well, so that I could make a POST request to the ‘/ratings’ endpoint of that particular place. After a few failed attempts, the solution was to spread in the ID, like so:

```
const StarRating = ({ value, ..._id }) => {
```

This did result in ```_id``` becoming an Object, so the POST request URL is a bit strange - but at least it works!

```
const response = await axios.post(`/api/places/${_id._id}/ratings`,
```

# Bugs, Blockers and Wins

As the reader will have seen by now, this project was not short of challenges! I have documented most of them already, but will provide a special place for deployment. We chose to deploy our back end to Heroku, and our front end to Netlify - and did this as a group, as Heroku in particular involved a whole new world of error messages to interpret. This was exacerbated by copious inconsistencies between the Heroku docs, Stackoverflow and Medium posts, and YouTube tutorials that we were consulting, and also by the recent migration of MongoDB services away from Heroku - developers now have to rely on third-party hosting via MongoDB Atlas, and somehow make this and Heroku communicate properly. Hours of trial and error finally got us to a successful deployment… apart from our interactive maps not rendering. More research revealed that changing to an older version of react-map-gl in our package.json (5.2.5, to be precise) would fix the issue.

# Future Features and Key Learning

On the whole, I am very happy with how this has turned out; but if I was to add or improve features, I would probably focus on the following:
- Find as many opportunities as possible for refactors, with a view to improving the page load speed (the downside of having so many glorious hi-res images of Icelandic scenery)
- Add functionality for users to remove places from their profiles
- Add functionality for users to compile certain saved places into different packages, and have these saved as groups
- Add onHover popups to the package map
- Find a working plugin for displaying routes, rebuilding maps from scratch if it comes to it

In terms of learning, my biggest takeaway from this project would be the improvement I have seen in myself when it comes to researching, and specifically in locating what I need to be reading in order to solve whatever problem I have encountered. This increased precision in search terms has already proven its value with subsequent projects, and I suspect will only continue to develop.

