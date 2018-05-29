import {createStore} from 'redux';
//Redux works as follows:

// Need a default state
var defaultState = 0; 

// Need a reducer
function amount(state = defaultState, action) { // ES6: if the state is defined, use that state, else use the defaultState
    if(action.type === 'INCREMENT') {
        return state+1;
    }
    return state;
}

// Need a reducer store - you are storing the state somewhere
var store = createStore(amount)

// Need to subscribe to that store - you are waiting for changes that happen in that store
store.subscribe(function() {
    console.log('state', store.getState());
})

// Need to dispatch events/actions - you are calling the reducer you defined to make changes on that state
store.dispatch({type: 'INCREMENT'})
store.dispatch({type: ''})
store.dispatch({type: 'INCREMENT'})

// !!! Passing data around !!!

var defaultState = {
    originAmount: '0.00'
} 

function amount(state = defaultState, action) {
    if(action.type === 'CHANGE_ORIGIN_AMOUNT') {
        return Object.assign({}, state, {originAmount: 'action.data'});
        //NOTE!!! - you use immutability - making a copy of the state to an empty object
        // You pass data from the dispatcher
    }
    return state;
}

store.dispatch({type: 'CHANGE_ORIGIN_AMOUNT', data: '300.65'})

// store.getState().originAmount -> this is how you get the state from the store


// REACT
In react, the primary way you tell a component to re-render, is to call setState

You can add the store.subscribe in ComponentDidMount() method

To pass the state from the store via props in an object, you need to subscribe to the store in ComponentDidMount and to setState in the subscription.

OR 

You can use react-redux npm module
import {Provider} from 'react-redux'

// Wrap the React component in the provider component
ReactDOM.render(<Provider store={store}><MainComponent /></Provider>, document.getElementById('container'))

// To access the store inside other components, you need to 'connect' the container component of that presentational components

import {connect} from 'react-redux';

return {
    <div>
        <ParentComponent testProp="hello">
    </div>
}

export default connect((state, props) => {
    // state is from the reducer
    // props is from the ParentComponent
    return {
        originAmount: state.originAmount
    }
})(ParentComponent)