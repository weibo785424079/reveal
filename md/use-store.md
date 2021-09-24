* useStore 构建小型的redux

```tsx
import React from 'react';
import { createAction, handleActions } from 'redux-actions';
import { createUseStore } from '@tms/site-hook';
import Button from 'antd/es/button';

const initState = {
  id: '-',
};

const setIdAction = createAction('设置ID');

const setId = ({ dispatch }) => (id: string) => dispatch(setIdAction({ id }));

const reducer = handleActions({
  [setIdAction.toString()]:
    (state, action) => ({
      ...state,
      ...action.payload,
    }),
}, initState);

const {
  useContextState,
  useActions,
  withProvider,
} = createUseStore(reducer, initState);

const Demo = withProvider(() => {
  const fns = useActions({ setId });
  const [state] = useContextState();
  return (
    <div>
      <div>{`id: ${state.id}`}</div>
      <Button onClick={() => fns.setId(String(Math.random()))}>设置ID</Button>
    </div>
  );
});

```
* 省去繁琐的connect mapPropsToState, Context.Consumer
* useReducer + useContext 实现小型redux, 范围内的状态管理,因为模块划分不如redux