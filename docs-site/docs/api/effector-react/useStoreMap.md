---
id: useStoreMap
title: useStoreMap
---

React hook, which subscribes to [store]/apieffector/Store.md) and transforms its value with given function. Component will update only when selector function result will change

Common use case: subscribe to changes in selected part of store only

```ts
useStoreMap<State, Result>(
  store: Store<State>,
  fn: (state: State) => Result
): Result
```

::: info
Short version of `useStoreMap` introduced in `effector-react@21.3.0`
:::

**Arguments**

1. `store`: Source [store]/apieffector/Store.md)
2. `fn` (_(state) => result_): Selector function to receive part of source store

**Returns**

(_Result_)

```ts
useStoreMap<Source, Result>({
  store: Store<Source>;
  keys: any[];
  fn: (state: Source, keys: any[]) => Result;
  updateFilter?: (newResult: Result, oldResult: Result) => boolean;
  defaultValue?: Result;
}): Result
```

Overload used when you need to pass dependencies to react (to update items when some of its dependencies are changed)

**Arguments**

1. `params` (_Object_): Configuration object
   - `store`: Source [store]/apieffector/Store.md)
   - `keys` (_Array_): This argument will be passed to React.useMemo to avoid unnecessary updates
   - `fn` (_(state, keys) => result_): Selector function to receive part of source store
   - `updateFilter` (_(newResult, oldResult) => boolean_): _Optional_ function used to compare old and new updates to prevent unnecessary rerenders. Uses [createStore updateFilter]/apieffector/createStore.md) option under the hood
   - `defaultValue`: Optional default value, used whenever `fn` returns undefined

**Returns**

(_Result_)

::: info
`updateFilter` option introduced in `effector-react@21.3.0`
:::

::: info
`defaultValue` option introduced in `effector-react@22.1.0`
:::

#### Example

This hook is useful for working with lists, especially with large ones

```js
import {createStore} from 'effector'
import {useStore, useStoreMap} from 'effector-react'

const data = [
  {
    id: 1,
    name: 'Yung',
  },
  {
    id: 2,
    name: 'Lean',
  },
  {
    id: 3,
    name: 'Kyoto',
  },
  {
    id: 4,
    name: 'Sesh',
  },
]

const $users = createStore(data)
const $ids = createStore(data.map(({id}) => id))

const User = ({id}) => {
  const user = useStoreMap({
    store: $users,
    keys: [id],
    fn: (users, [userId]) => users.find(({id}) => id === userId),
  })

  return (
    <div>
      <strong>[{user.id}]</strong> {user.name}
    </div>
  )
}

const UserList = () => {
  const ids = useStore($ids)

  return ids.map(id => <User key={id} id={id} />)
}
```

[Try it](https://share.effector.dev/cAZWHCit)