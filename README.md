<h1 align="center">
Reducers
<h1>

The Redux store had a reducer function for updating its state.

The reducer function receives the current state and an action, updates the state appropriately based on the action.type, and returns the next state.

<h1 align="center">
Updating the reducer to handle additional action types
<h1>

The reducer from Fruit Stand App;

```js
    const fruitReducer = (state=[], action) => {
        switch (action.type) {
            case 'ADD_FRUIT':
                return [...state, action.fruit];
            default:
                return state;
        }
    }
```

When the store is created, it called its reducer with an undefined state, allowing the reducer to dictate the store's initial value via the state parameter's default value.

First the reducer decides what logic to implement based on the action.type switch.

Then, it creates and return a new object representing the next state if any if the info needs to be changed.

The state is returned unchanged if no cases match the action.type, meaning that the reducer doesnt care about that actions.

In the above example, the reducer's initial state is set to an empty array([])

The reducers returns a new array with action.fruit appended to the previous.state if action.type is "ADD_FRUIT". Otherwise, it returns the state unchanged.

Additional case claused can be added to update the reducer to handle the following action types:

    * "ADD_FRUITS" - add an array of fruits to the inventory of fruits

    * "SELL_FRUITS" - remove the first instance of a fruit if available

    * "SELL_OUT" - Someone bought the whole inventory of fruit! Return an empty array

