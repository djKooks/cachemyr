[![CircleCI](https://circleci.com/gh/djKooks/cachemyr.svg?style=svg)](https://circleci.com/gh/djKooks/cachemyr)

# cachemyr
Cachemyr[/kaʃˈmɪə] is simple in-memory cache storage module for node.

## Why started?
I was using [node-cache](https://github.com/ptarjan/node-cache) before. While using, there was some error and we were suspecting memory overflow as one of the reason. Finally, that was not the reason, but I thought it would be good if possible to let user know when heap memory usage is close to allowed memory in node(usually 1.4G).
Also, there was bit of performance upgrade:

> State:
>> 1. Write 100,000 key-value to storage
>> 2. Read 100,000 data from storage with random key
>> 3. Repeat 100 times

(node-cache)
```
====== Memory usage ====== 
heap total:88.4765625
heap used:51.40773010253906

======= R/W performance(Avg. of 100 times test) ==========
Write Avg: 53.61ms
Read Avg: 52.71ms
```
(cachemyr)
```
====== Memory usage ====== 
heap total:75.91796875
heap used:29.33899688720703

======= R/W performance(Avg. of 100 times test) ==========
Write Avg: 36.84ms
Read Avg: 53.12ms
```

It depends on state, but usually shows bit slower performance on reading, but way more fast on writing.

But yet `node-cache` has been almost 3~4 years and proved by lot of projects, this has only been a few month and needs more test and improvement to become stable. Hope you could help!

## How to:
### Install
```
// for 'npm' user
$ npm i cachemyr

// for 'yarn' user
$ yarn add cachemyr
```

### Use 
#### // TODO:

This is example for user:
```javascript
const mc = require('cachemyr')

mc.put('key-1', 1)
console.log(mc.get('key-1')) // 1

mc.delete('key-1')
console.log(mc.get('key-1')) // null

mc.put('key-2', 2, 6000, (key, value) => {
    // called when data with key `key-2` has been expired (after 6 second)
})

console.log(mc.getLength()) // 1 (<'key-2', 2>)
```

### API
#### put: void
Put key-value data to storage

Variable | Type | Description
--- | --- | ---
key | string | Key of KV(key-value) data
value | any  | Value of KV data
duration | number(optional) | Length of duration(ms) to keep this data
timeoutCB | Function(optional) | Callback function to be called when added data has been expired
overflowCB | Function(optional) | Callback function when memory usage of storage has overrun the value defined in configuration


#### get: any | null
Get value of key from storage. Returns `null` when there is no key.

Variable | Type | Description
--- | --- | ---
key | string | Key of KV data
instantRemoval | boolean(optioanl) | Remove after get when set to true


#### remove: void
Remove key-value set from storage

Variable | Type | Description
--- | --- | ---
key | string | Key of KV data


#### drop: void
Remove all data in storage


#### getLength: number
Return number of key-value set in storage


## Contribution
Improvements, bug report & fix, document updates, suggestions, and stars are all welcome!

### Build command:
```
$ yarn build // build module

$ yarn test // test module
```

