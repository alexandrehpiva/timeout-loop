[![npm](https://img.shields.io/npm/v/timeout-loop.svg)](https://www.npmjs.com/package/timeout-loop) [![npm](https://img.shields.io/npm/dy/timeout-loop)](https://www.npmjs.com/package/timeout-loop)

# timeout-loop
Calls a function for a specified number of times


# Installation
```
yarn add timeout-loop
```

## Dist target
Since 1.2.0 version, the project is released in ES6 for compatibility (previous versions were targeted to ESNext).

## Examples

The nicer thing is that it can be awaited:


```js
import timeoutLoop from 'timeout-loop'

let loopCounter = 0;

(async () => {

  // Executes the callback function every 1 second
  await timeoutLoop(() => {
    loopCounter += 1;
    console.log(`Call number ${loopCounter}.`)
  }, 1000, 3);

  console.log('Finish') // Logs 'Finish' after 3 seconds

})
```

It can also be used in unit tests (Jest). For example, I used this lib to test one of my projects: [exec-latest](https://github.com/alexandrehpiva/exec-latest)

```js
import timeoutLoop from 'timeout-loop'
import execLatest from '../execLatest'

type Exec = {
  loopCounter: number,
  execCounter: number
}

describe('Execute only the latest call received in a determined time in milliseconds', () => {
  it('Should execute only the latest call in 3 calls made in 100 milliseconds each', async () => {
    let loopCounter = 0
    let execCounter = 0
    const execTrace: Exec[] = []

    await new Promise(async resolveExecLatest => {
      await timeoutLoop(() => {
        loopCounter += 1

        execLatest(() => {
          execCounter += 1

          execTrace.push({ loopCounter, execCounter })
          resolveExecLatest()
        })
      }, 100, 3)
    })

    // Three executions
    expect(loopCounter).toEqual(3)

    // Only the latest call stored in execTrace
    expect(execTrace).toEqual([{ loopCounter: 3, execCounter: 1 }])
  })
})
```

## Contributing
If you'd like to contribute to this project, please feel free to open a pull request.

Enjoy!
