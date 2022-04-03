+++
author = "Abijith B"
title = "Proper way to update nested state in React"
date = 2022-03-12T17:46:37+05:30
tags=[
    "react",
    "frontend",
    "javascript"
]
+++

# State updation 101
We only need to re-render when a state have changes, for more clarity, when
values changes.
```js
/* wrong way */
const [data, setData] = useState([]);

const addData = (newValue) => {
    data.push(newValue);
    setData(data);
}
```

But this won't trigger a re-render, why?
we have added a element to array, so it should change right? But it's
expensive to make such comparison. To improve performance react is using
shallow comparison. It only check if the reference had changed. If this
seems confusing checkout [blog](https://daveceddia.com/javascript-references/)
to understand more.
```js
/* proper way */
const [data, setData] = useState([]);

const addData = (newValue) => {
    cosnt newData = [...data, newValue];
    setData(newData);
}
```
Now state updation will trigger re-render, because `newData` have a different reference.

# What about nested data
```js
const [address, setAddress] = useState({
    office: {
	coordinates: {
	    lat: 0,
	    long: 0
	}
    }
})
```
Now we have a nested state, what is the right way to update this?

## Changing the reference alone
```js
const updateCoord = (lat, long) => {
    const newAddress = {...address};
    newAddress.office.coordinates.lat = lat;
    newAddress.office.coordinates.long = long;
    setAddress(newAddress);
}
```
Yep, this will trigger a re-render as expected. But there is an issue.
```js
<Map coord={address.office.coordinates} />

const Map = ({coord}) => {
    useEffect(() => {
	// fetch data when coord changes
	getNewData(coord);
    }, [coord]);

    reutrn (
	<div> ... </div>
    );
}
```
When `coord` change we should trigger API call, but useEffect is using
shallow comparison, as we mutated the `coordinates` object API call will
not be triggered.

## Change reference in all levels
we need to create a copied and updated object for each level of nesting that is affected.

```js
const updateCoord = (lat, long) => {
    const newAddress = {
    ...address
    office: {
	...address.office,
	coordinates: {
		lat,
		long
	    }
	}
    };
    setAddress(newAddress);
}
```
As reference changed API call will be triggered.

# Reference
- https://stackoverflow.com/questions/71360185/immutable-update-pattern-in-react-for-nested-data
- https://reactjs.org/docs/update.html
