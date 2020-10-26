# timeout-loop
Calls a function for a specified number of times

# Installation
```
yarn add timeout-loop
```

## Example

```js
import timeoutLoop from 'timeout-loop'

let loopCounter = 0;

(async () => {

  await timeoutLoop(() => {
    loopCounter += 1;
  }, 1000, 3);

  console.log(loopCounter) // Logs 3 after 3 seconds

})
```