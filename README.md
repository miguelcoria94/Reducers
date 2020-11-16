<h1 align="center">
Reducers
</h1>

The Redux store had a reducer function for updating its state.

The reducer function receives the current state and an action, updates the state appropriately based on the action.type, and returns the next state.

<h1 align="center">
Updating the reducer to handle additional action types
</h1>

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

```js
    const fruitReducer = (state=[], action) => {
        switch (action.type) {
            case'ADD_FRUIT':
                return [...state, action.fruit];
            case'ADD_FRUITS':
                return [...state, action.fruits];
            case'SELL_FRUIT':
                const index = state.indexOf(action.fruit);
                if(index !== -1){
                    //remove the first instance of action.fruit
                    return [...state.slice(0, index), ...state.slice(index + 1)];
                }
                return state; //if action.fruit is not a in state, return previous state
            case 'SELL_OUT':
                return [];
            default:
                return state;

        }
    }
```

<h1 align="center">
Reviewing how Array.slice works
</h1>

```js
    [...state.slice(0, index), ...state.slice(index + 1)]
```

The Array.slice method return a new array containing a shallow copy of the array elements indicated by the start and end args.

The start arguement is the index of the first element to include and the end argument is the index of the element to include up to but not including.

If the end arguement isnt provided, all the array elements up to the end of the array will be included.

The original array will not be modified.

By combining calls to the Array.slice method into a new array, a copy of an array can be created that omits an element at a specific index (index):

    * state.slice(0, index) - returns a new array containing the elements starting from index 0 up to index

    * state.slice(index + 1) - returns a new array containing the elements starting from index + 1(one past the index to omit the element at index) up through the last element in the array.

Then the spread syntax is used to spread the elements in the slices into a new array.

```js
    const fruits = ['apple', 'apple', 'orange', 'banana', 'watermelon'];

    // The index of the 'orange' element is 2.
    const index = fruits.indexOf('orange');

    // `...fruits.slice(0, index)` returns the array ['apple', 'apple']
    // `...fruits.slice(index + 1)` returns the array ['banana', 'watermelon']
    // The spread syntax combines the two array slices into the array
    // ['apple', 'apple', 'banana', 'watermelon']
    const newFruits = [...fruits.slice(0, index), ...fruits.slice(index + 1)];
```

This approach to removing an element from an array is just one way to complete the operation without modifying or mutating the orginal array.

<h1 align="center">
Avoiding state mutations
</h1>

Inside a Redux reducer, you must never mutate its arguments (state and action).

Your reducer must return a new object if the state changes.

Here's an example of a bad reducer which mutates the previous state.

```js
    const badReducer = (state = {count: 0}, action) => {
        switch (action.type){
            case 'INCREMENT_COUNTER':
                state.count++;
                return state;
            default:
                return state;
        }
    }
```

and here's an example of a good reducer which uses Object.assign to create a shallow duplicate of the previous state:

```js
    const goodReducer = (state = {count: 0}, action) => {
        switch (action.type){
            case 'INCREMENT_COUNTER':
                const nextState = Object.assign({}, state);
                nextState.count++
                return nextState;
            default:
                return state;
        }
    }
```

