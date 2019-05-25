---
title: 使用 typesafe-actions 创建类型安全的 redux
tags:
  - redux RN
categories:
  - RN
---


### typesafe-actions 是什么

typesafe-actions 是一款专门为 ts 设计的库，能够帮助开发者减少 redux 样板代码，并自动为开发者创建好类型。

> 大佬直接异步 [官方仓库](https://github.com/piotrwitek/typesafe-actions#createasyncaction)

这个仓库是 React 和 Redux 生态的一部分，so，如果想换个方式写 action、reducer 和 type，可以继续看下去。



### 如何安装使用

在保证已经安装好 react 和 redux 的情况下，使用以下两种方式安装：

```bash
// NPM
npm install typesafe-actions

// YARN
yarn add typesafe-actions
```



### 常用 API 介绍

#### action 创建方式一：

```javascript
import { action, createAction } from 'typesafe-actions';

export const add = (title: string) => action('todos/ADD', { id: cuid(), title, completed: false });
// add: (title: string) => { type: "todos/ADD"; payload: { id: string, title: string, completed: boolean; }; }

export const add = createAction('todos/ADD', action => {
  // Note: "action" callback does not need "type" parameter
  return (title: string) => action({ id: cuid(), title, completed: false });
});
// add: (title: string) => { type: "todos/ADD"; payload: { id: string, title: string, completed: boolean; }; }
```

####  action 创建方式二：

```javascript
import { createStandardAction } from 'typesafe-actions';

export const toggle = createStandardAction('todos/TOGGLE')<string>();
// toggle: (payload: string) => { type: "todos/TOGGLE"; payload: string; }

export const add = createStandardAction('todos/ADD').map(
  (title: string) => ({
    payload: { id: cuid(), title, completed: false },
  })
);
// add: (payload: string) => { type: "todos/ADD"; payload: { id: string, title: string, completed: boolean; }; }
```

值得注意的是，以上通过这两个 api 创建的 action，是符合 Flux 定义的 action，即 ` { type, paylaod, meta, error }`这适用于绝大多数情况。当官方值不能满足的情况下，你也可以采用自定义 action 的方式。

####  action 创建方式三（自定义）：

```javascript
import { createCustomAction } from 'typesafe-actions';

const add = createCustomAction('todos/ADD', type => {
  return (title: string) => ({ type, id: cuid(), title, completed: false });
});
// add: (title: string) => { type: "todos/ADD"; id: string; title: string; completed: boolean; }
```



#### 在 reducer 中使用 action

在reducer中，我们有 `getType` api 来帮助我们获取 action 的 type：

```javascript
// reducer.ts
switch (action.type) {
    case getType(todos.add):
      // below action type is narrowed to: { type: "todos/ADD"; payload: Todo; }
      return [...state, action.payload];
    ...
```

除了官方创建 reducer 的方式外， `createReducer` api 也可帮助创建：

```javascript
// using action-creators
const counterReducer = createReducer(0)
  // state and action type is automatically inferred and return type is validated to be exact type
  .handleAction(add, (state, action) => state + action.payload)
  .handleAction(add, ... // <= error is shown on duplicated or invalid actions
  .handleAction(increment, (state, _) => state + 1)
  .handleAction(... // <= error is shown when all actions are handled

  // or handle multiple actions using array
  .handleAction([add, increment], (state, action) =>
    state + (action.type === 'ADD' ? action.payload : 1)
  );

// all the same scenarios are working when using type-constants
const counterReducer = createReducer(0)
  .handleAction('ADD', (state, action) => state + action.payload)
  .handleAction('INCREMENT', (state, _) => state + 1);

counterReducer(0, add(4)); // => 4
counterReducer(0, increment()); // => 1
```



⬆ 对于对代码质量有要求的人来说，上面这种相对于默认创建方式的满屏 case return 更优雅



当然除了优雅，上面这种方式还能获得 ts 的类型增强，而不需要额外多处的引用，我们只需要



```javascript
// types.d.ts
import { StateType, ActionType } from 'typesafe-actions';

declare module 'typesafe-actions' {
  export type Store = StateType<typeof import('./store').default>;

  export type RootState = StateType<typeof import('./store/root-reducer').default>;

  export type RootAction = ActionType<typeof import('./store/root-action').default>;

  interface Types {
    RootAction: RootAction;
  }
}
```

所有类型都自动创建完成，即可减少类型定义方面的代码。



### 总结

这个库减少我们写样板代码和随处可见的类型定义，如果你使用 redux 和 ts，请尝试一下这个库。
