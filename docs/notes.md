# REACT Basics

- Everything is a component.
- A component is a reusable piece of your website. 

- Capital in all components because they can be used more than once.
- All components need at least one method and that is the render method.
- The html part which hooks react up is called the MountPoint
- No need to import all ReactDOM if we only need the render method.
- Best practice is to have each component in a separate file
- you need to import ES6 modules for every component
- JSX is the templating language recommended to be used.

- At the end of the component adding `export default ComponentName`allows to import the component in another component.
- To import this component in another file, you need to add the relative path to the file. Otherwise it will look in the `node_modules/` folder. No need to specify extension. It will be assumed

JSX is the templating language used, where components are rendered between <tags/>. Previously, there was the need to concatenate strings when we wanted to have html inside javascript. Then, with ES6 Templating - `<p>${hello}</p>` - this was better but not perfect. hence JSX.
JSX allows us to write html right inside of our javascript. 


## JSX
******
JSX is the templating language recommended to be used in react. About it:
- Add a parenthesis () to the return statement inside render so you can use multiline.
- Given that classes in JSX cannot use `class` since that is a reserved word in JS, it needs to be className.
- You can only ever return one parent element. So need to wrap a div in child elements
- You need to self close your html tags. 
- Writing comments inside a jsx block -> `{/* A comment */}`. Anywhere else, normal js tags (`//` a comment). Don't put them at top level, otherwise it will try to return that and error.
- jsx has no logic build into it. There are no loops, if statements.


## CSS in React
***************
Can be done multiple ways. 
- Link tag with a stylesheet hooked up to the template.
- Use Webpack. Load them into the entrance file index.js
Can have different css files per component. That allows to scope the css to that specific component. 


## REACT browser extension
**************************
ReactConsole tool will have the object assigned to a variable like '$r'. That way it is possible to debug and check the object in the console.
Normal console, in the inspector select element and then go to the console and do `$0`.


## STATELESS FUNCTIONAL COMPONENTS
**********************************
Real simple components which only render stuff into the DOM. It is not actually needed the full react component which one gets with `class componentName extends React.Component`.
It is only needed in those circumnstances a *Stateless functional component*. If you don't need other methods other than render it does not make sense to have the full react component.
In *Stateless functional components, props are passed as an argument and since its not binded to a component, `this` cannot be used.


## REACT
********
In React you want to stay way from touching the DOM as much as possible. Because the way react works is that we modify the data, we render out the JSX and are handsoff touching the DOM, letting react do that for us.

To create a component:
```
class ComponentName extends React.Component {
    // if you need to add methods out of render or initialize
    constructor(){ 
        super();
    }
    
    //any methods

    render(){
        return ()
    }
}

export default ComponentName;
```


Other characteristics of React worth keeping in mind:
- Dynamically providing data to components -> Use *props*
Inside a component, *this* is the component.
- render() is bounded to the component that it is going to be rendered. All the other methods in the class, they are not going to be bound to the actual component.
- To reference parts of the DOM, use 'ref'. Beforehand string refs would be used to reference elements. Now its moving towards function refs. For example, <input ref={(input) => {this.storeInput = input}} />, will add the ref storeInput to the input. It can then be referenced like $r.storeInput.
- When REACT changed to ES6 extend syntax, it does not implicit bind all the methods on our component to the component itself. There are several ways to achieve this, one is to use the constructor of the component. The constructor of the component is the thing that runs when that component is created (see below). The other possible way is to go to where the event is declared and add `onSubmit={this.goToStore.bind(this)}`. This will bind the goToStore to the this, which inside render() is the component itself. Another way to do it, its to use the arrow function, onSubmit={(e} => this.goToStore(e)}. The downside of doing it on the event call is that if we were to render 7 or 8 submits on a page is that an individual function for every single component that gets rendered. As a thumb rule, everytime `this` should be used inside your methods, you need to add the binding to the component.
-To add default value when 1st loading do not use property `value` because that will be tied to the state. Use instead `defaultValue=""`.

JS: Helper functions. When i have some functionality that is not linked to any specific component and should be reused accross the application. Create helpers functions.


## REACT constructor
********************
```
    constructor{
        super();
        this.goToStore = this.goToStore.bind(this)
    }
```
- `super()` has to be added when the constructor is called in a component. This is to run ReactComponent so we can extend with our component features.
- `this.goToStore = this.goToStore.bind(this)` -> Here, it looks for the `goToStore()` method, and sets to its own self, but binding it to this, which inside the constructor, is the component itself. This has to be done to all methods of the constructor. 


## REACT EVENTS
***************
- Are wrapped in a cross-browser wrapped called `Syntetic Event`.
- Events are done inline.


## ROUTING WITH REACT
*********************
Routing the URL to the right component/view to be loaded. ReactRouter is not part of react. 
Using ReactRouter4.
Allows you to show and hide components anywhere inside of the application depending on if you are on the page you wish to view.
The router, like everything else in react is a component.
```
<BrowserRouter>
    <Match exactly pattern="/" component={StorePicker} />
</BrowserRouter>
```
When pattern matches exactly `/`, load component StorePicker.
`<Miss />` is used for when the url is not routed to any match.


### REACT ROUTER 4
**************
There are two ways to move to another the page with react router, one way is to use `<Redirect to="/somewhere/else" />`. Another is to use the the imperative API (`.transitionTo()`) which allows to transition from one page to other. 
- When we have the router in the components' context, we can find `context.router.transitionTo()` in the context and use it to navigate to the page we want. In react all is client side, so it uses HTML5 pushState, which allows for pages to transition without reloading.

- how to use the Router in a component. Because the `<BrowserRouter />` is the parent of the whole structure, it is possible to access it from any child component in application. In order to transition from a component we need to surface the router from the component. The way to do this in react is through context. What it does is to make available to the lower level when declared to the top level. But not to be abused since it will create global components.


## REACT CONTEXT
**************** 
Go to the component where you want to fetch something from the global context and add at the end
```
ComponentName.contextTypes = {
router: React.PropTypes.object
}
```
This tells react that the StorePicker component expects something called *router* so it will be made available to the component.


## REACT STATE
**************
- State is a representation of all of the data in our application.
- Each component can have its own state, but it should seen as an object that holds all the data that relates to a piece of our application. State is always tied to a specific component. 
- Whenever changes are needed on the page, these should be done to the state and then react will handle the html/dom for you. 
- When you need the state to be shared between multiple components, put the state on the parent component of those who will need it and then pass it down.

#### Use state on react component:
* Get the initial state: react needs to know what state your component will have and what type it will be. This is done in a constructor. Add to the constructor `this.state={state1:{}, state2:"one"};}`
* To get the child state to reach parent component, make a method on the parent component to both update and set the state. This method needs to be added to the constructor to be bounded to the component.(ie. `this.addFish = this.addFish.bind(this);`)
* React needs to be told explicitly which state we have updated and want to set. The whole state shouldn't be passed so that React does not have to check all of them to update in the DOM.
* When `this.setState({toBeUpdated})` is invoked, the state that is to be updated,will be updated for every instance React finds of that state in the DOM.
* Finally, to have the children components have their state being shares, on the parent component, when invoking the child component, pass the `prop` you want to share. In this case, on the parent `render()` method, pass the method which allows for the state to be updated, to the child component `<ChildComponent addFish={this.addFish}/>`.
* The previous step needs to be repeated down the stream until the component where the change that triggers the state update, is called. However, on the child component, the method can be found in its props, so on the child component `render()` function, the "grand"-child component is called like `<GrandChildComponent addFish={this.props.addFish}/>`.
* When we are on the component where the object is modified, we need to call the method we created to update the state and had been passed down through `props`. This needs to be done in the place where the object is modified. 

#### Update and Set State:
- You can't directly update your state by assigning it a value. It is a best practice to take a copy from current state and then update your actual state. This is done for performance and to avoid race conditions. 
- To pass a variable up from the child component that triggers the method which updates the state and it declared in the parent component, add an inline arrow function when making the call inside the child component and add the argument to be passed. If this `<button onClick={this.props.myMethod('toBePassed')}>` was done, it would be run on page load. Instead, the following options can be done:1) `onClick={this.options.myMethod.bind(null, 'toBePassed')}`, or 2) `onClick={() => this.props.MyMethod('toBePassed')}`. If no variable are passed, the method can be simply called directly.
- The `key` variable should not be used by the user. If there is the need to pass down an object identifier, you need to pass the value explicitily using another variable. This is also the case even if you are using the same property as for `key`.

#### Loading data into State onCLick:
- The method that loads the data into the components needs to happen where our state lives. In this case, this is in the parent component where the state was instantiated.

#### Display State with JSX:
- JSX has no logic build into it. To do this, you need to use regular javascript. Normal way to loop over something in react is using `map`, but this is for arrays and the state is an object. To this effect we can use `Object.keys()`. By using `Object.keys(MyObject)`, an array with all the keys is return so I can map over the object using the keys.
- elements of the same type should have an unique identifier so that react knows which to update. 
- Implementation wise, to load all the state into a component with its own data, the following should be done:
    ```
    Object
      .keys(this.state.MyObject)
      .map(key => <Component key={key} details={this.state.MyObject[key]}/>)
    ```
    Here, `map`, takes in a list of keys and returns a list of components making use of each `key` to fetch the correct element of the object.

    >tip: 
        If you want to add a variable to a tag attribute, no need for quotes around the variable. ie. `<img tag={this.props.name}/>`. When there is the need to re-use props all over again, a variable can be declared to hold these inside `render()`. ie, `const { details } = this.props`. See ES6 notes for destructuring syntax. 

#### Render Function:
Sometimes it doesn't make sense to create a new component where everything has to be passed down. Shell out some of the work to another render function that lives inside the current component.

### DELETE ITEMS FROM STATE:
To remove from state:
- If firebase is not connected, a simple `delete object[key_to_delete]` followed by `setState({})` will suffice.
- If firebase is connected, the element to be deleted needs to be set explicitily to `null`.

## REACT LIFECYCLE
******************
Entry point to the component. When a component is being mounted, or rendered, there are different entry points to the component.

`componentWillMount()`: Allows to hook up to the moment right before the component is rendered to the screen and allows to sync the component state with for instance, firebase. 

`componentWillUnmount()`: Hookup to the moment the component is going to be destroyed. 

`componentWillUpdate()`: This a state for when the data is updated. Should be used when you want watching data change for the component avoiding to have to add watchers to every place the state may update. It runs whenever `props` or `state`, changes.

`shouldComponentUpdate()`: Invoked before rendered when new props of state are being received. This allows the rendering to wait before receiving these new props and avoid multiple rendering while waiting for the different data inputs. Returns `true` or `false`, should or not component update.


## BIDIRECTIONAL DATA FLOW AND LIVE STATE EDITING
*************************************************

React will raise errors if the state into inputs, unless there is a handler to update it. If the value to be put in the input is not tied to the state and will be immutable, `defaultValue` should be use to add the value to the tag. If state is added, through the `value` property, instructions on how the state should be updated, should be provided. 

To see what is the change triggered by the event, use `event.target`. This way it is easy to know what should be targeted on the copied state to be updated on the method that handles the input changes.


## ANIMATION IN REACT
*********************
Import `CSSTransitionGroup` from `react-addons-css-transition-group`.
Replace the tag of the element to add animations to by the `CSSTransitionGroup` component and give it the property of the tag it corresponds to in html. This is done by adding `component="ul"`. If the element to be styled used to be an unordered list. Other properties like `transitionName`
`transitionEnterTimeout`, `transitionLeaveTimeout`, can be added. React is putting some classes in the DOM in order for these to be hooked up with the css.

When adding a transition where react may create adjacent components which are the same type, there is the need to add a unique key. 

> note:
In css animations, a starting and ending point is needed.
`transition all 0.5s` -> this will give the transition from start to end point the duration of 0.5s
`transform translateX(-120%)` -> this provides a sliding in animation


## PROPTYPES
************
Allows you to validata the data that is comming into our components (props)by validating that: 
a) Data is passed
b) Data that is passed is of the correct type.

After the component is defined add:
```
    Component.propTypes = {
        value_component_takes : React.PropTypes.string
    }
```
This will force that if we call `Component` with `value_component_takes` being anything else than a `string`, react will raise validation errors. These errors are only displayed in development.
There are addional verifications that can be added. For istance `React.PropTypes.string.isRequired`, will ensure the type of the prop is a `string` and this prop is mandatory to be specified. 

Add these to all your components.


## FIREBASE
***********
Backend service. Uses HTML5 websockets. Allowing to update the database real time having it synced with your application.

#### Persisting of data with Firebase:
- Go to `Databases` > Rules. Add security options to allow data read/write.
- On the project, choose the objects to be synced with Firebase. For this, Rebase is used. To use rebase, a file specifying the component responsible for the database connection has to be created (see `base.js`).
- On the Firebase console, click 'Add Firebase to webapp'. Fetch *apiKey*, *authDomain* and *databaseUrl* and add them to the configuration file `base.js`.
- To hide these keys which are being put on the client side, firebase authentification rules come in.

`syncState()` in the rebase API allows us to sync current state. It is recommended to only sync the needed parts of the state instead of the whole state. Need to pass what we want to sync with, and an object with the context and also the state we actually want to sync. When we move to another address, we need to stop syncing to the current one. 

`removeBinding()` is the rebase API call that allows us to remove the syncing bounded to a certain element. Should be used in `componentWillUnmount()`.

#### AUTHENTIFICATION

Firebase provides some methods to authentification. 
`AuthWithOAuthPopup()` allows to use OAuth to trigger the authentification APIs. The provider used is the first argument and the callback function to be called to handle the authentification output (success or error).

On the authentification handler function, the information from the log in should be stored in the state, 
 
To connect to the firebase database and get all the info stored there from the objects, such as use all firebase APIs, the method `base.database()` can be used. To grab a specific part of the database, use `base.database().ref(<to_be_grabbed>)`.

To query once the reference which was retrieved with the previous step do 
```
ref_to_database.once('value', (snapshot) => {
    const data = snapshot.val() || {};
});
```
`snapshot` is firebase object of all the data, `snapshot.val()` is the data itself. 

To have some persistence on the authentification, attach the authentification listener `base.onAuth()` to the react lifecycle component `componentDidMount()`. With the proper handle to check if the user exists and its logged in and will reauthenticate just before the component re-renders. 

To logout, use the firebase API call `base.unauth()`, which will close any open connections to firebase.

To prevent external parties to access your data go to Firebase > Security Rules. 


## HTML5 LOCAL STORAGE
**********************
Storing the data from the user in the browser. Alternative to cookies.
LocalStorage is tied to the hostname. It is a key,value pair. 

To set item:
`localStorage.setItem('item_key', 'item_value')`

To get item:
`localStorage.getItem('item_key')`

You can't store Objects inside localStorage. Only strings can be stored. If you need to turn it into a string, use json: `JSON.stringify(object)`. When retrieving back from localStorage and put into object, the inverse method has to be done `JSON.parse(string)` which will turn the string back into the object.

Triggering the fetch of local storage on `componentWillMount()` has a small delay due to the "double" rendenring of waiting for the component to be fully loaded and then fetch the localStorage. To avoid this, `shouldComponentUpdate()` could be used.


## ES6 notes 
************
- `{...this.state.fishes}` -> `{...}` this is a *spread* in ES6. What it does its to take all the data from the object and spread it to the new object. *this.state.fish* is our current set of fishes in state, and with the spread, a copy of each existent fish is put in the new object. 
- In ES6, `{fishes: fishes}` can be written as `{fishes}`
- ES6 desctructuring, `const details = this.props.details` can be destructured to `const { details } = this.props`.

    > tip: 
    Arrow function: `(input) => { statements }` is equivalent to `function (input): return statements;`
- reduce():
The reducer loops over an array and allows you to return something. It needs a starting value. The syntax is `[array].reduce(callback, <starting value>)`

- copy an object:
1. Take `Object.assign({}, <to_be_copied>)`, thus creating a new object which is a copy of the first one. 
2. Use the ES6 *spread* notation, {...<to_be_copied>}

- update a copied object:
Using the *spread* notation from ES6, it is possible to overlay the newly copied object properties with the desired values. If needed, *computed properties* can be used for this effect. The syntax is as follows:
```
    const updatedObject = {
        ...<to_be_copied, 
        [property_key_to_change]: new_property_value
        }

```

- When its not a default export, you need to use name import. so it needs to be imported like `import {thingToImport} from './blabal'`


